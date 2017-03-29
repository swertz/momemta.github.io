# The configuration file

## Configuring modules

Declaring a module named `karl` of type `Gaussian` in Lua is simple:

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

A **parameter** can be an integer (eg.: `2`), a floating point number, (eg.: `172.5`, `13000.` - note the decimal point *must* be present), a boolean (`true` or `false`) or a string (eg.: `hello`). Parameters might have default values and be optional, or be mandatory.

An **input** is set through a so-called "**input tag**" of the form `other_module::its_output[/i]`, where `other_module` is the name of another module responsible for producing the quantity `its_output`, and `/i` has to be used when the produced quantity in question is a vector and only a specific entry of that vector is needed as input. 

Some modules expect **vectors** of input tags, where the order of the entries may or nor matter. Vectors in Lua are easily declared:

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

When declared, a module tells MoMEMta how many dimensions it adds to the integration (for instance, adding a transfer function on the energy of a jet means integrating over the energy of the corresponding parton). If a module adds a dimension, it also means it requires a component of the phase-space point to be specified as input (see below). MoMEMta keeps track of the total number of dimensions needed for the integration. 

The order in which the modules are declared in the script does not matter: MoMEMta automatically executes the module in an order such that inputs are always well-defined (obviously, circular dependencies are forbidden). By default, modules whose outputs are not used by any other module, and modules whose inputs are not produced by any other modules are not executed.

## Default inputs, outputs and parameters <a name=defaults></a> 

Some inputs and outputs are not defined by any module *per se* but are used by MoMEMta to pass and retrieve important quantities:

Inputs:

* `input::particles`: Vector of LorentzVectors passed as argument to `computeWeights()`

* `input::met`: LorentzVector of the observed MET (optional argument to `computeWeights()`)

* `cuba::ps_points`: Vector of doubles between 0 and 1. Phase-space point, ie. set of random numbers launched by Cuba to perform the integration. The length of the vector is the total number of integration dimensions declared by the modules. Instead of manually specifying the `cuba::ps_points/i` input tags, it is possible to use to `getpspoint()` function (which automatically increases “i” each time it is called).

* `cuba::ps_weight`: (double) Weight associated with the phase-space point

Outputs:

* `integrands`: Vector of doubles. This is the value taken to be the integrand, returned to Cuba to compute the integral. Exactly **one** declared module **must** produce this quantity.

Parameters:

* `cuba`

* `parameters`
