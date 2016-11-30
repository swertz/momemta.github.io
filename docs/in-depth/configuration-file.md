# The configuration file

## Configure modules

Different *types* of modules are available on MoMEMta, each designed to perform a specific task. 
Declaring an instance of a module named `karl` of type `Gaussian` in Lua is simple:

```lua
Gaussian.karl = {}
```

Each module can be configured by setting its parameters and its inputs:

```Lua
Gaussian.karl = {}
Gaussian.friedrich = {
    -- Some fixed parameters
    age = 25,
    size = 1.75,
    place = "Gottingen",
    dead = true,
    -- Define inputs using other modules’ outputs
    input = "karl::output"
}
```

A **parameter** is strongly typed and can be an integer (eg.: `2`), a floating point number, (eg.: `172.5`, `13000.` - note the decimal point *must* be present), a boolean (`true` or `false`) or a string (eg.: `hello`). Parameters might have default values and be optional, or be mandatory.

An **input** is set through a so-called "**input tag**" of the form `other_module::its_output[/i]`, where `other_module` is the name of another module responsible for producing the quantity `its_output`, and `/i` has to be used when the produced quantity in question is a vector and only a specific entry of that vector is needed as input. 

Some modules expect **vectors** of input tags, where the order of the entries may or may not matter. Vectors in Lua are easily declared:

```Lua
-- Take first and third entry of vector output `output` of module `that_module`
inputs = { "that_module::output/1", "that_module::output/3" }
```

Accessing entries of previously defined vectors is also possible:
```Lua
temp_vector = { "first", "second", "third" }
-- Use only { "first", "third" }:
my_inputs = { temp_vector[1], temp_vector[3] }
```

!!! warning
    The indexing of Lua vectors or input tags starts at **1, not 0**.

The use of some modules requires adding a dimension to the volume that is being integrated over. In other words, those modules need a component of the phase-space point to be specified as input. This is done using the function `add_dimension()`, which returns an input tag linking to a component of the phase-space point, and notifies MoMEMta to add an integration dimension.

The order in which the modules are declared in the script does not matter: MoMEMta automatically executes the modules in an order such that inputs are always well-defined (obviously, circular dependencies are forbidden). By default, modules whose outputs are not used by any other module, and modules whose inputs are not produced by any other modules are not executed.

A full list of modules available out-of-the-box, along with documentation about the modules' inputs, outputs and parameters, is available at MoMEMta's [technical documentation](https://momemta.github.io/MoMEMta/dev/group__modules.html).

## Declare input particles

Particles passed as input to the `computeWeights()` function (see [Calling MoMEMta](calling-momemta)) can be linked with the modules by registering them in the Lua configuration. For instance, for a particle declared in C++ as `momemta::Particle m_part1 { "part1", p1, 0 }`, one would do:
```Lua
local l_part1 = declare_input("part1") -- Pass as argument the name string of m_part1
```
The Lua object `l_part1` now contains the input tag needed to access the particle's 4-vector. For instance, defining a transfer function on the energy of `part1` can be done as:
```Lua
GaussianTransferFunctionOnEnergy.tf_part1 = {
  ps_point = add_dimension(), -- A transfer function integrates over a variable (the particle's energy), so we need a new dimension in the integrated volume
  reco_particle = l_part1.reco_p4, -- Pass the input tag corresponding to the experimentally reconstructed 4-vector of the particle, passed to 'computeWeights()'
  sigma = 0.10 -- Take 10% resolution on the energy
}
```
Since the transfer function integrates over the particle's energy, it generates new values for its energy. This defines a new, 'parton'-level 4-vector for `part1`, which can now be passed e.g. to a matrix element. In this example, the parton-level 4-vector can be accessed directly through the `tf_part1::output` input tag. It is also possible to register it with the `l_part1` object:
```Lua
l_part1.set_gen_p4("tf_part1::output")
```
When declaring further modules, instead of using `tf_part1::output`, it is now possible to use `l_part1.gen_p4` to access the parton-level 4-vector, i.e. these are equivalent:
```Lua
P4Printer.printer1 = { input = "tf_part1::output" }
P4Printer.printer2 = { input = l_part1.gen_p4 }
```

Finally, the missing transverse energy 4-vector (passed optionally to `computeWeights()`) can either be accessed directly through the `met::p4` input tag, or declared as a Lua object as above:
```Lua
local met = declare_input("met")
```

## Declare the integrand

When the integration is fully configured, i.e. all the final module defining the integrand value is defined, the integrand has to be registered with MoMEMta to be able to compute the integral. 
For instance, if the module called `final_module` with output `output` defines the integrand, one has to call:
```Lua
integrand("final_module::output")
```

It is possible in MoMEMta to integrate multi-valued functions (caution: is only efficient if the components of the function are not too different). For two components, the integrand would then be declared as:
```Lua
integrand("final_module_1::output", "final_module_2::output")
```

## Define parameters <a name="parameters"></a>

In MoMEMta, once the `ConfigurationReader` has been "frozen" to give a `Configuration` object, it is not possible anymore to modify anything in the integration. However, some parameters can be modified in the `ConfigurationReader` from the C++ code (allowing e.g. to carry out mass scans easily). In order to do that, the parameters have to be put in the `parameters` table in Lua:
```Lua
parameters = {
  my_mass = 173.,
  my_width = 2.5
}
```
These parameters can then be linked with the modules' parameters using the `parameter()` Lua function:
```Lua
BreitWignerGenerator.m_prop = {
  ps_point = add_dimension(),
  mass = parameter('my_mass'), -- Access the value through the "parameter" function, allowing it to be modified from the C++ code
  width = parameters.my_width -- Access the value as a regular Lua table. It will NOT be possible to modify it later on
}
```
Changing `my_mass` from C++ before freezing the configuration is now possible using `ConfigurationReader::getGlobalParameters()`, see [calling momemta](calling-momemta).

## Configure the integration algorithm

The integration algorithm can be configured from within Lua using the `cuba` table, for instance:
```Lua
cuba = {
  integration_algorithm = "divonne", -- default is "vegas"
  relative_accuracy = 0.01
}
```
Four different integration algorithms are available, and each has several parameters than can be tweaked to adjust the precision and convergence speed of the calculations. 
For a full list of the available methods and corresponding options, please consult the documentation of the numerical integration library used in MoMEMta: Cuba[^1].

The integration parameters can be changed from the C++ in a manner similar to the parameters in the previous section, using the method `ConfigurationReader::getCubaConfiguration()`.

## Pass arguments to the configuration

Once the `ConfigurationReader` has parsed the Lua configuration file, it is not possible anymore to change the resulting computation flow (the only changes possible are the above parameters). 
It is however possible to pass arguments from C++ to the Lua script when it is read by the `ConfigurationReader`, to influence its execution. 

Defining the arguments in C++:
```cpp
ParameterSet lua_parameters;
lua_parameters.set("text1", "hello, world!");
lua_parameters.set("text2", "always look on the bright side of life!");
lua_parameters.set("question", true);
ConfigurationReader configuration("example.lua", lua_parameters);
```
Makes them available in Lua, i.e.:
```Lua
if question then
  print(test1)
else
  print(test2)
end
```
Would print "hello, world!".

[^1]: [T. Hahn, *"Cuba - a library for multidimensional numerical integration"*](https://arxiv.org/abs/hep-ph/0404043)
