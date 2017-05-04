## Defining parameters

In MoMEMta, once the `ConfigurationReader` has been "frozen" to give a `Configuration` object, it is not possible anymore to modify anything in the integration. However, some parameters can be modified in the `ConfigurationReader` from the C++ code (allowing e.g. to carry out mass scans easily). In order to do that, the parameters have to be put in the special `parameters` table in Lua:
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

## Configuring the integration algorithm

The integration algorithm can be configured from within Lua using the `cuba` table, for instance:
```Lua
cuba = {
  algorithm = "divonne", -- default is "vegas"
  relative_accuracy = 0.01
}
```
Four different integration algorithms are available, and each has several parameters than can be tweaked to adjust their behaviour, tweak the precision of the calculations, ...
The integration parameters can be changed from the C++ in a manner similar to the "global" parameters (previous section), using the method `ConfigurationReader::getCubaConfiguration()`.
The available algorithms and corresponding options are briefly listed here; for a full description, please consult the documentation of the numerical integration library used in MoMEMta: Cuba[^1]. 

Bear in mind the default values for these parameters are not guaranteed to be optimal for your particular case! It's your job to ensure the algorithm is tweaked to work well for the events and the functions you'll be integrating.

Integration algorithm:

| Parameter | Allowed values | Default value |
| --------- | -------------- | ------------- |
| `algorithm` | `vegas`, `suave`, `divonne`, `cuhre` | `vegas`|

Common arguments:

| Parameter | Allowed values | Default value |
| --------- | -------------- | ------------- |
| `relative_accuracy` | floats | `0.005` |
| `absolute_accuracy` | floats | `0.` |
| `seed` | integers | `0` |
| `min_eval` | integers | `0` |
| `max_eval` | integers | `500000` |
| `grid_file` | paths | `` |
| `verbosity` | integers | `0` |
| `subregion` | booleans | `false` |
| `retainStateFile` | booleans | `false` |
| `levels` | integers | `0` |
| `ncores` | integers | `0` |
| `pcores` | integers | `1000000` |

Vegas-specific arguments:

| Parameter | Allowed values | Default value |
| --------- | -------------- | ------------- |
| `takeOnlyGridFromFile` | booleans | `true` |
| `n_start` | integers | `25000` |
| `n_increase` | integers | `0` |
| `batch_size` | integers | `min(n_start, 50000)` |
| `grid_number` | integers | `0` |
| `smoothing` | booleans | `true` |

Suave-specific arguments:

| Parameter | Allowed values | Default value |
| --------- | -------------- | ------------- |
| `n_new` | integers | `1000` |
| `n_min` | integers | `2` |
| `flatness` | floats | `0.25` |

Divonne-specific arguments:

| Parameter | Allowed values | Default value |
| --------- | -------------- | ------------- |
| `key1` | integers | `47` |
| `key2` | integers | `1` |
| `key3` | integers | `1` |
| `maxpass` | integers | `5` |
| `border` | floats | `0.` |
| `maxchisq` | floats | `10.` |
| `mindeviation` | floats | `0.25` |

Cuhre-specific arguments:

| Parameter | Allowed values | Default value |
| --------- | -------------- | ------------- |
| `key` | integers | `0` |

## Passing arguments to the configuration

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

