In MoMEMta, the integration is defined by linking modules together in a Lua script. MoMEMta is shipped with a set of modules covering the most common needs and whose parameters, inputs and outputs are documented [here](https://momemta.github.io/MoMEMta/dev/group__modules.html). For more information on how to write the configuration file, see [here](configuration-file.md).

!!! note
    A module is just a C++ class deriving from the `Module` virtual class.

[//]: # ( How to write a new module and make it available for the calculation is described (here). )

Once the configuration file is defined, it can be loaded by MoMEMta’s configuration reader:
```cpp
ConfigurationReader my_reader("relative_path_to_lua_file.lua");
```

At this point, the user might wish to modify some parameters defined in the file from the code itself (see also [here](parameters)):
```cpp
my_reader.getGlobalParameters().set("top_mass", 173.);
```

Then, MoMEMta can be instantiated using a "frozen" configuration set:
```cpp
Configuration my_config = my_reader.freeze();
MoMEMta weight(my_config);
```

!!! note
    MoMEMta is instantiated using a `Configuration` object, describing a frozen configuration: parameters cannot be modified at this stage, the execution flow of the computation is fully fixed. Parameters can however be changed in the ConfigurationReader, and a new `MoMEMta` instance must then be constructed by calling `ConfigurationReader::freeze()` again.

The weight can finally be computed by calling the [`computeWeights()`](https://momemta.github.io/MoMEMta/dev/classMoMEMta.html#a7022a8573e8232c75cda198f59a64cbe) method of the MoMEMta object, passing the observed particles as arguments:

```cpp
ROOT::Math::LorentzVector<ROOT::Math::PxPyPzE4D<double>> p1, p2, met;
momemta::Particle part1 { "part1", p1, 0 };
momemta::Particle part2 { "part2", p2, 0 };
std::vector<std::pair<double, double>> weights = weight.computeWeights({part1, part2}, met);
```

!!! note
    The MET is an optional argument (defaults to a null vector)

The `momemta::Particle` type consists of three parts: a name (string) allowing to identify the particle in the configuration file, a 4-vector object, and a type (int). The type is not used by MoMEMta at the moment, but is present for future features.

!!! tip
    MoMEMta provides the useful typedef `LorentzVector` for Lorentz Vectors in the file `momemta/Types.h`

!!! warning
    MoMEMta checks that your inputs are physical, i.e. have non-negative energy and mass. Due e.g. to machine precision issues, it is possible your inputs have slightly negative masses without you noticing it. However, MoMEMta can produce nonsensical output or even crash in this case (*garbage in, garbage out*). It's up to the user to correct the inputs in those cases.

The function `computeWeights()` starts the Monte-Carlo integration: the integrand function is called a large number of times, each time passing as input a phase-space point vector (where the length of the vector is the dimensionality of the integrated phase-space) with elements between 0 and 1, and returning as output the integrated function evaluated on this point.

`computeWeights()` returns a vector of pairs (weight, uncertainty). In most cases, this vector will contain only one entry, but MoMEMta allows the possibility to define *vector* integrands, i.e. integrate multi-valued functions, and return a weight and uncertainty for each component.

[//]: # (> *The phase-space points are associated with a weight, given that they are not distributed uniformly in the phase-space, but according to the importance function defined by Vegas. In most use cases this weight is not needed by the user since Cuba automatically takes it into account when computing the integral, however it can still be retrieved through the `cuba::ps_weight` input tag (see [here](configuration-file.md#defaults)).*)

## Python bindings

Calling MoMEMta from python is also possible, provided the python bindings have been built (see the [build options](../getting-started.md#build-options)). If you have not installed MoMEMta system-wide but in a user-defined directory (say `MOMEMTA_DIR`), you should make sure the two following conditions are satisfied:

  * `$MOMEMTA_DIR/lib[64]/pythonX/site-packages/` (exact path depends on your system) should be in your `PYTHONPATH`
  * `$MOMEMTA_DIR/lib/` should be in your `LD_LIBRARY_PATH`

Then, you might call MoMEMta from your python analysis script in this manner:
```python
import momemta

# Restrict logging to "error" level
momemta.set_log_level(momemta.log_level.error)

momemta_cfg = momemta.ConfigurationReader("path_to_config.lua")
momemta_computer = momemta.MoMEMta(momemta_cfg.freeze())

# px, py, pz, E
p1 = [10, 20, 30, 200]
p2 = [-10, 20, -30, 200]

## Using ROOT 4-vectors is also possible
# import ROOT
# LorentzVector = ROOT.Math.LorentzVector(ROOT.Math.PxPyPzE4D("double"))
# p1 = LorentzVector(10, 20, 30, 200)
# p2 = LorentzVector(-10, 20, -30, 200)

result = momemta_computer.computeWeights([p1, p2])
print("Weight: {} +- {}".format(result[0][0], result[0][1]))
```

More information about the Python bindings can be found in the [technical documentation](https://momemta.github.io/MoMEMta/dev/group__Python.html).
