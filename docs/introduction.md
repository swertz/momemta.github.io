# Introduction

## The Matrix Element Method

The Matrix Element Method (MEM) is a multivariate method in which a per-event likelihood ("weight") is computed from first principles. This weight can be expressed, given a certain theoretical hypothesis, by the convolution of the matrix element (squared) corresponding to this hypothesis, the parton distribution functions, and the transfer functions and efficiencies describing the evolution from a parton-level configuration to the observed event:

$$P(\mathbf{x} | \alpha ) = \frac{1}{A_{\alpha} \sigma_{\alpha}} \int  \text{d} \Phi(y) \frac{\text{d} x_1 \text{d} x_2}{x_1 x_2 s} \, f(x_1) f(x_2) \, |\mathcal{M}_{\alpha}(y,x_1,x_2)|^2 \, W(\mathbf{x}|y) \, \epsilon_{\alpha}(y) \tag{1} \label{MEM}$$

Where:

* $\mathcal{M}_{\alpha}$ is the matrix element (usually LO) under hypothesis $\alpha$.

* The transfer function $W(\mathbf{x}|y)$ is the probability density that $y$ ends up as the measured event $\mathbf{x}$ in the detector. It describes parton shower, hadronization, and detector and reconstruction effects. It is normalised as $\int \text{d} \mathbf{x} \, W(\mathbf{x}|y) = 1$ while the probability that $y$ ends up as a selected event is included in the efficiency term $\epsilon_{\alpha}(y)$.

* $f$ and $x_i$ are the Parton Distribution Functions (PDFs) and Björken-x; $\text{d} \Phi(y)$ is the phase-space density term; $s$ is the hadronic centre-of-mass energy.

* The denominator $A_{\alpha} \sigma_{\alpha}$, including the total cross-section of the process $\alpha$ and the overall acceptance $A_{\alpha}=\langle \epsilon_{\alpha}(y)\rangle$ ensures that $P$ is correctly normalised as a likelihood. In some use cases, this normalisation factor can be neglected.

This per-event likelihood has two main analysis use-cases:

* Constructing the sample likelihood $\mathcal{L}=\prod_{i \in \text{data}} P(\mathbf{x}_i|\alpha)$ and measuring a parameter using the maximum likelihood method

* Constructing a Neyman-Pearson discriminant $P(\mathbf{x}|\alpha)/P(\mathbf{x}|\beta)$ to discriminate a process (signal) against others (backgrounds), useful either for conducting discrete hypothesis testing or for searches for rare processes or New Physics signals

However, computing an integral like ($\ref{MEM}$) is a difficult numerical problem, due to the presence of sharp peaks in the integrand (propagators in the matrix element, transfer functions on angles and energies). Solving it in a reasonable amout of time requires the use of both adaptive Monte-Carlo integration algorithms _and_ smart phase-space parametrisations.

## MoMEMta

MoMEMta is a modular C++ framework designed with the aim to allow a quick and efficient computation of weights under a large number of theoretical hypotheses, while remaining easily configurable by the user, enabling easy interfacing with existing analysis workflows.

The modularity of MoMEMta consists in configuring the integrand of ($\ref{MEM}$) by linking modules together. A module is responsible for a small part of the computation: transfer functions, phase-space parametrisation, matrix element, …, each module’s output being fed to other modules as input. This configuration is realised by the user through a file written in the Lua scripting language. The integration is then carried out by MoMEMta using Monte-Carlo algorithms (such as Vegas) as implemented in the Cuba library. MoMEMta ships modules convering most needs, but it is possible for the user to write additional modules to perform specific tasks.

It should be noted that MoMEMta provides dedicated phase-space parametrisations especially suited for the computation of MEM integrals, greatly improving the speed of the calculations compared to a naive approach. The philosophy of these parametrisations is mostly based on results described in [^1], with a few additions. 

[^1]: [Pierre Artoisenet, Vincent Lemaitre, Fabio Maltoni and Olivier Mattelaer, *“Automation of the matrix element reweighting method”*, JHEP *12* (2010) *068*, p. 17](http://arxiv.org/pdf/1007.3300v2.pdf)
