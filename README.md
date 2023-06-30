# Korg.py

This provides instructions on how to execute [Korg.jl](https://github.com/ajwheeler/Korg.jl), a 1D LTE stellar spectral synthesis code for FGK stars in Julia, from Python.

[Code paper](https://ui.adsabs.harvard.edu/abs/2023AJ....165...68W/abstract): Wheeler et al. (2023). Please cite this if you use Korg.

[Tutorial](https://github.com/ajwheeler/Korg.jl/blob/main/misc/Tutorial%20notebooks/Tutorial.ipynb)

[Documentation](https://ajwheeler.github.io/Korg.jl/stable/)

## Installation

The following code snippet will install and link Julia 1.9.1 to your current Python environment, and install the latest version of [Korg.jl](https://github.com/ajwheeler/Korg.jl).

```
wget https://raw.githubusercontent.com/abelsiqueira/jill/v0.4.0/jill.sh
sudo bash jill.sh -y -v 1.9.1
export PYTHON=$(which python)
julia -e 'using Pkg; Pkg.add("PyCall")'
pip install julia
python -c 'import julia; julia.install()'
unset PYTHON
julia -e 'using Pkg; Pkg.add("Korg")'
```

## Example

This repository contains the example line list and model atmosphere necessary to execute the example below.

```python
import matplotlib.pyplot as plt
from julia import Korg

lines = Korg.read_linelist("linelist.vald", format="vald")
atm = Korg.read_model_atmosphere("s6000_g+1.0_m0.5_t05_st_z+0.00_a+0.00_c+0.00_n+0.00_o+0.00_r+0.00_s+0.00.mod")
A_X = Korg.format_A_X(0)
sol = Korg.synthesize(atm, lines, A_X, 5160, 5176)

fig, ax = plt.subplots(figsize=(12, 4))
ax.plot(sol.wavelengths, sol.flux, "k-")
ax.set_xlabel("$\lambda$ [Å]")
ax.set_ylabel("$F_\lambda/R_\mathrm{star}^2$ [erg s$^{-1}$ cm$^{-5}$]")
fig.savefig("example.png")
```
![example](https://github.com/andycasey/Korg.py/assets/504436/92110745-fce1-4c21-881d-b98cf296998d)

# FAQ / Issues

I have some kind of linking errors.

> Try `python-jl` instead of `python`, or `python-jl -m IPython` instead of `ipython`.
> Can also run this within python/ipython before doing anything:
> from julia.api import Julia; jl = Julia(compiled_modules=False)

I am having trouble installing on an ARM (M1/M2) Mac with anaconda
* This doesn't yet work with ARM, so change it to use x86. Create a new environment _without_ installing anything, then run:
> conda config --env --set subdir osx-64

This makes your environment install with osx-64. Once that is done, then install python. To get the example to work:
```
conda create -n korg
conda activate korg
conda config --env --set subdir osx-64
conda install conda install python matplotlib scipy astropy numpy jupyter ipython
```
Then in ipython:
```python
from julia.api import Julia; jl = Julia(compiled_modules=False) # takes a moment

import matplotlib.pyplot as plt
from julia import Korg

lines = Korg.read_linelist("linelist.vald", format="vald")
atm = Korg.read_model_atmosphere("s6000_g+1.0_m0.5_t05_st_z+0.00_a+0.00_c+0.00_n+0.00_o+0.00_r+0.00_s+0.00.mod")
A_X = Korg.format_A_X(0)
sol = Korg.synthesize(atm, lines, A_X, 5160, 5176)

fig, ax = plt.subplots(figsize=(12, 4))
ax.plot(sol.wavelengths, sol.flux, "k-")
ax.set_xlabel("$\lambda$ [Å]")
ax.set_ylabel("$F_\lambda/R_\mathrm{star}^2$ [erg s$^{-1}$ cm$^{-5}$]")
fig.savefig("example.png")
```
Recall that Korg will take a while to run the first spectrum, because it needs to compile. But if you run it again, it will be very fast each subsequent time, so you win when making a large number of spectra.
