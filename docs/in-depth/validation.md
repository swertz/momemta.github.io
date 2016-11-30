# Validation

The phase-space parametrisations in terms of blocks are validated using two differents methods:

 - **Computation of a cross-section**: the block is used to compute the cross-section of a given process. The result is then compared to the cross-section computed using MadGraph[^1] for the same process.
 - **Computation of a phase-space volume**: only the phase-space density is integrated (no PDF or matrix element), and the result is compared with an analytical result[^2].

In both cases, MoMEMta's phase-space parametrisations are not efficient for the problem, but by increasing the number of integrand evaluations to a very large number, sufficient precision can be reached nonetheless.

## Block A

### Cross-section

The process used is $pp \rightarrow W^+W^- \rightarrow \mu^+\mu^-\nu\bar{\nu}$

 - MadGraph cross-section: $0.866(5)$ pb
 - MoMEMta cross-section: 

### Phase-space volume

Volume computed: production of 3 massless particles at $\sqrt{s} = 1$ TeV

 - Theoretical phase-space volume: $6.3 \cdot 10^{-15}$
 - MoMEMta phase-space volume: $6.299(6) \cdot 10^{-15}$


## Block B

### Cross-section

The process used is $pp \rightarrow W^+W^- \rightarrow \mu^+\mu^-\nu\bar{\nu}$

 - MadGraph cross-section: $0.866(5)$ pb
 - MoMEMta cross-section: $0.868(8)$ pb

### Phase-space volume

Volume computed: production of 3 massless particles at $\sqrt{s} = 1$ TeV

 - Theoretical phase-space volume: $6.3 \cdot 10^{-15}$
 - MoMEMta phase-space volume: $6.297(6) \cdot 10^{-15}$


## Block D

The process used is $pp \rightarrow t\bar{t}$ fully-leptonic

 - MadGraph cross-section: $6.73(2)$ pb
 - MoMEMta cross-section: $6.69(3)$ pb

### Phase-space volume

Volume computed: production of 6 massless particles at $\sqrt{s} = 1$ TeV

 - Theoretical phase-space volume : $694 \text{GeV}^6$
 - MoMEMta phase-space volume: $695(4) \text{GeV}^6$


## Block F

### Cross-section

The process used is $pp \rightarrow W^+W^- \rightarrow \mu^+\mu^-\nu\bar{\nu}$

 - MadGraph cross-section: $0.866(5)$ pb
 - MoMEMta cross-section: $0.864(4)$ pb

### Phase-space volume

Volume computed: production of 4 massless particles at $\sqrt{s} = 1$ TeV

 - Theoretical phase-space volume: $0.0166 \text{GeV}^2$
 - MoMEMta phase-space volume: $0.0166(3) \text{GeV}^2$


## Block G

### Cross-section

The process used is $pp \rightarrow W^+W^- \rightarrow \mu^+\mu^-\nu\bar{\nu}$

 - MadGraph cross-section: $0.866(5)$ pb
 - MoMEMta cross-section: $0.864(4)$ pb

### Phase-space volume

Volume computed: production of 4 massless particles at $\sqrt{s} = 1$ TeV

 - Theoretical phase-space volume: $0.0166 \text{GeV}^2$
 - MoMEMta phase-space volume: $0.01659(3) \text{GeV}^2$


## Secondary Block C/D

### Cross-section

The process used is $pp \rightarrow W^+W^- \rightarrow \mu^+\mu^-\nu\bar{\nu}$ <br/> Main block used for computation: B

 - MadGraph cross-section: $0.866(5)$ pb
 - MoMEMta cross-section: $0.868(8)$ pb

### Phase-space volume

Volume computed: production of 4 massless particles at $\sqrt{s} = 1$ TeV <br/> Main block used for computation: A

 - Theoretical phase-space volume: $0.0166$ $\text{GeV}^2$
 - MoMEMta phase-space volume: $0.01663(6) \text{GeV}^2$


[^1]: [J. Alwall et al., *“The automated computation of tree-level and next-to-leading order differential cross sections, and their matching to parton shower simulations”*, JHEP *07* (2014) *079*](https://arxiv.org/pdf/1405.0301v2)
[^2]: [Pierre Artoisenet, Vincent Lemaitre, Fabio Maltoni and Olivier Mattelaer, *“Automation of the matrix element reweighting method”*, JHEP *12* (2010) *068*, p. 17](http://arxiv.org/pdf/1007.3300v2.pdf)
