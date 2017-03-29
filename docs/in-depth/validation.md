# Validation

The main blocks responsible for the change of variables are validated using two differents method:

 - **Computation of the cross-section**: the block is used to compute the cross-section of a given process. The result is then compared to the cross-section computed using `MadGraph` for the same process.
 - **Computation of the phase-space volume**: only the phase-space density is integrated (no PDF or matrix element), and the result is compared to a hand-made calculation.

## Block B

!!! warning
    This block is included in MoMEMta, but is not yet validated. Be careful if you decide to use it.

## Block D

The process is $pp \rightarrow t\bar{t}$ fully-leptonic

 - `MadGraph` cross-section: $6.73 \pm 0.02$ pb
 - MoMEMta cross-section: $6.69 \pm 0.03$ pb

### Phase-space volume

 - Theoritical phase-space volume[^1] : $694$ $\text{GeV}^6$
 - MoMEMta phase-space volume: $694.993 \pm 3.93901\ \text{GeV}^6$

## Block F

### Cross-section

The process is $pp \rightarrow W^+W^- \rightarrow \mu^+\mu^-\nu\bar{\nu}$

 - `MadGraph` cross-section: $0.8666 \pm 0.0053$ pb
 - MoMEMta cross-section: $0.8636 \pm 0.00433\,10^{-3}$ pb

### Phase-space volume

 - Theoritical phase-space volume[^1] : $0.0166$ $\text{GeV}^2$
 - MoMEMta phase-space volume: $0.0166 \pm 2.6985\,10^{-5}\ \text{GeV}^2$

[^1]: [Pierre Artoisenet, Vincent Lemaitre, Fabio Maltoni and Olivier Mattelaer, *“Automation of the matrix element reweighting method”*, JHEP *12* (2010) page 17](http://arxiv.org/pdf/1007.3300v2.pdf)
