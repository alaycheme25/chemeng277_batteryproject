# Machine Learning-Driven Prediction and Optimization of Lifetime Performance and Cost in Solid-State Batteries

The global effort to reduce global emissions relies on the research for better energy storage options. Batteries which store electrical energy are especially important, as green power sources like solar or wind may not be generating electricity 100% of the time. Therefore, optimizing battery performance is key to transitioning towards green power.

This project uses machine learning algorithms to help identify and predict the lifetime and cost of solid-state batteries by using a scoring method to determine battery performance based on bandgap energy, crystal volume, formation energy, and stability. The cost feature is estimated using the working ion cost per hour for the battery and scaling it by the energy density of the battery. **{WIP}**

## Setup
Our machine-learning model is trained on data from [Materials Project](https://next-gen.materialsproject.org/), so you are not required to obtain your own data. However, the model requires certain packages to be installed to function properly. Additional instructions for using the data scrapers and building your own datasets can be found in [Link Text](#Customization)

To bulk install them all, download `requirements.txt` and open terminal to the folder in which it is located:
```
pip install -r requirements.txt
```
or if you don't want to download `requirements.txt`:
```
pip install numpy, ast, matplotlib, json, pandas, paretoset, pymatgen, scikit-learn, scipy, sympy
```

For conda:
```
conda install -c conda-forge numpy, ast, matplotlib, json, pandas, paretoset, pymatgen, scikit-learn, scipy, sympy
```

## Usage

### Training data

### Testing data

## Customization

## Contact
