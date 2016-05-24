In MoMEMta, the integration is defined by linking modules together in a Lua script. MoMEMta is shipped with a set of modules covering the most common needs and whose parameters, inputs and outputs are documented [here](https://momemta.github.io/MoMEMta/group__modules.html). For more information on how to write the configuration file, see [here](configuration-file.md).

> *A module is just a C++ class deriving from the `Module` virtual class. How to write a new module and make it available for the calculation is described (here).*

Once the configuration file is defined, it can be loaded by MoMEMta’s configuration reader:
```cpp
ConfigurationReader my_reader("relative_path_to_lua_file.lua");
```

At this point, the user might wish to modify some parameters defined in the file from the code itself:
```cpp
configuration.getGlobalParameters().set("top_mass", 173.);
```

Then, MoMEMta can be instantiated using a "frozen" configuration set:
```cpp
ParameterSet my_config = my_reader.freeze();
MoMEMta weight(my_config);
```

> *MoMEMta is instantiated using a `ParameterSet`, describing a fixed configuration: parameters cannot be modified at this stage. Parameters can be changed in the ConfigurationReader, and a new `MoMEMta` instance must then be constructed by calling `ConfigurationReader::freeze()` again.*

The weight can finally be computed by calling the computeWeights() method of the MoMEMta object, passing the observed particles as a set of LorentzVectors:

```cpp
LorentzVector p1, p2, p3, p4, met;
std::vector<std::pair<double, double>> weights = weight.computeWeights({p1, p2, p3, p4}, met);
```

> *The MET is an optional parameter*

> *The LorentzVectors are expected to be expressed in the `PxPyPzE<double>` basis*

The function starts the Monte-Carlo integration: the integrand function is called a large number of times, each time passing as input a phase-space point vector (where the length of the vector is the dimensionality of the integrated phase-space) with elements lying between 0 and 1, and returning as output the integrated function evaluated on this point.

`computeWeights()` returns a vector of pairs (weight, uncertainty) (at the moment, this vector only has one entry) which can then be stored by the user.

> *The phase-space points are associated with a weight, given that they are not distributed uniformly in the phase-space, but according to the importance function defined by Vegas. In most use cases this weight is not needed by the user since Cuba automatically takes it into account when computing the integral, however it can still be retrieved through the `cuba::ps_weight` input tag (see [here](configuration-file.md#defaults)).*