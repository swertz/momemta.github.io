# The MoMEMta project

**MoMEMta** is a software package designed in a modular way to cover the needs of the experimental analysis workflows at the LHC. MoMEMta provides working examples for the most common final states (ttbar, WW, ...). If you are expert user, be prepared to feel the freedom of configure your MEM computation at all levels.

MoMEMta is based on:

 - C++, ROOT, Lua scripting language
 - Cuba library for integration
 - External PDFs (LHAPDF by default)
 - External Matrix Element (provided with MadGraph C++ exporter)

## Getting started

You first need to install MoMEMta. A guide on how-to install from source is available [on our github repository](https://github.com/MoMEMta/MoMEMta/blob/prototype/README.md#install). You can then browse our [documentation](http://momemta.github.io/MoMEMta/) to learn more about MoMEMta.

## Usage example

```cpp
#include <momemta/ConfigurationReader.h>
#include <momemta/Logging.h>
#include <momemta/MoMEMta.h>
#include <momemta/Utils.h>

int main(int argc, char** argv) {

    ConfigurationReader configuration("tt_fullyleptonic.lua");
    MoMEMta weight(configuration.freeze());

    LorentzVector p3(16.17, -13.79, -3.43, 21.53);
    LorentzVector p4(-55.79, -111.59, -122.14, 174.66);
    LorentzVector p5(-18.90, 10.09, -0.60, 21.43);
    LorentzVector p6(71.39, 96.01, -77.25, 142.50);

    std::vector<std::pair<double, double>> weights = weight.computeWeights({p3, p4, p5, p6});

    std::cout << "Result: " << weights.back().first << " +- " << weights.back().second;
    
    return 0;
}
```

More examples can be found [here](https://github.com/MoMEMta/MoMEMta/tree/prototype/examples).

## Our team

 - Sébastien Brochet
 - Alessia Saggio
 - Miguel Vidal
 - Sébastien Wertz
