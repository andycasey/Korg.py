A Python wrapper for [Korg.jl](https://github.com/ajwheeler/Korg.jl), a 1D LTE stellar spectral synthesis code for FGK stars in pure Julia.

[Code paper](https://ui.adsabs.harvard.edu/abs/2023AJ....165...68W/abstract) (please cite this if you use Korg)

[Tutorial](https://github.com/ajwheeler/Korg.jl/blob/main/misc/Tutorial%20notebooks/Tutorial.ipynb)

[Documentation](https://ajwheeler.github.io/Korg.jl/stable/)

## Installation

The following code snippet will install and link Julia to your current Python environment.

```bash
wget https://raw.githubusercontent.com/abelsiqueira/jill/v0.4.0/jill.sh
bash jill.sh -y -v 1.9.1
export PYTHON=$(which python)
julia -e 'using Pkg; Pkg.add("PyCall")'
pip install julia
python -c 'import julia; julia.install()'
unset PYTHON
```

## Example

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
fig.savefig("example.pdf")
```


# FAQ / Issues

I have some kind of linking errors.

> Try `python-jl` instead of `python`, or `python-jl -m IPython` instead of `ipython`.

