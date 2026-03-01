# Machine Learning-Driven Prediction and Optimization of Lifetime Performance and Cost in Solid-State Batteries

The global effort to reduce global emissions relies on the research for better energy storage options. Batteries which store electrical energy are especially important, as green power sources like solar or wind may not be generating electricity 100% of the time. Therefore, optimizing battery performance is key to transitioning towards green power.

This project uses machine learning algorithms to help identify and predict the lifetime and cost of solid-state batteries by using a scoring method to determine battery performance based on bandgap energy, crystal volume, formation energy, and stability. The cost feature is estimated using the working ion cost per hour for the battery and scaling it by the energy density of the battery. **WIP**

## Setup
Our machine-learning model is trained on data from [Materials Project](https://next-gen.materialsproject.org/), so you are not required to obtain your own data. However, the model requires certain packages to be installed to function properly. Additional instructions for using the data scrapers and building your own datasets can be found in [Link Text](#Customization)

To bulk install them all, download `requirements.txt` and open terminal to the folder in which it is located:
```
pip install -r requirements.txt
```
or if you don't want to download `requirements.txt`:
```
pip install numpy, matplotlib, json, pandas, paretoset, pymatgen, scikit-learn, scipy, sympy, mp_api
```

For conda users, run:
```
conda install -c conda-forge numpy, matplotlib, json, pandas, paretoset, pymatgen, scikit-learn, scipy, sympy, mp_api
```

## Usage
### Training data
`MATSCI_176_Project_Data.xlsx` **CHANGE WHEN NECESSARY**
Training data is obtained from the [Materials Project](https://next-gen.materialsproject.org/) by running 
`Download_from_MP_API_updated.ipynb`. The first 6 columns are for battery categorization via working ion, elements, formula charge, etc. Columns 7-14 are scraped from Materials Projeect and are available features for performance estimation. 

However, to reduce the number of features in our model which may cause overfitting, we decided to combine features from Materials Project into more intuitive desciptors: 
- `Electrode_density`: `capacity_vol`/`capacity_gravimetric`.
- `Energy_density`: `Electrode_density`*|`Energy_gravimetric`|
- `Battery_cost_per_L`: `Energy_density`*Working_ion_cost_per_kWh`/1000
  
### Testing data 
**WIP, add when we get the training data set**

## Training and using the model: 
Machine learning and usage occurs in `machine_learning.ipynb`. 
Before running, the following line of code in cell $3$ needs to be changed to access the [training data](#Training data) on your device:
```
pdata = pd.read_excel("MATSCI_176_Project_Data.xlsx")
```
Run `machine_learning.ipynb` to:
- Clean training data by removing all nan, empty, or invalid samples. Subsequently, normalizes remaining samples.
- Compare different dimensionality reduction techniques (PCA, pareto front analysis, etc.)
- Trains models using ridge and elsatic net regression.
- Determines battery score based on chosen features.
- Evaluates models by comparing $R^2$ values, mean average error, and mean standard error.
- Applies trained model to test dataset and returns best material based on score. 
## Customization


## Contact
For inquiries regarding the project or bugs in our code, feel free to email us at:
-
- Allen Qiang, aqiang@stanford.edu
- 
Additionally, don't hestitate to open up issues for this repository!
