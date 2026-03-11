# A Physics-Informed Machine Learning-Framework for Predicting Performance in Solid-State Batteries

The global effort to reduce global emissions relies on the research for better energy storage options. Batteries which store electrical energy are especially important, as green power sources like solar or wind may not be generating electricity 100% of the time. Therefore, optimizing battery performance is key to transitioning towards green power.

This project uses Ridge and Elasticnet regression in addition to XG-Boost to identify and predict the effective gravimetric energy density (GED) of solid-state batteries. The data was obtained from Materials Project [Materials Project](https://next-gen.materialsproject.org/) database using the api.client Python package and is filtered to include the materials that have Li, Na, Mg, Ca, and Ag as a working ion. The model is trained to predict the absolute gravimetric energy density for proposed battery materials. 

Our training datasets consists of the following features: max_delta_volume, average_voltage, capacity_grav, stability_discharge, material_density, formation_energy_per_atom, band_gap, lattice constants ('a', 'b', 'c'). The input dataset has the following features dropped to avoid direct correlation with gravimetric energy density: battery working ion cost per kWh, volumetric energy density, gravitational energy, electrode density, stability charge, volumetric energy, and volumetric capacity.

Our testing data composes of DFT-derived features: max delta volume, average voltage, gravimetric capacity, stability discharge, material density, formation energy per atom, energy bandgap, and lattice constants ('a', 'b', 'c').

## Setup
Our machine-learning model is trained on data from [Materials Project](https://next-gen.materialsproject.org/), so you are not required to obtain your own data. However, the model requires certain packages to be installed to function properly. Additional instructions for using the data scrapers and building your own datasets can be found in [Customization](#Customization)

To bulk install them all, download `requirements.txt` and open terminal to the folder in which it is located:
```
pip install -r requirements.txt
```
or if you don't want to download `requirements.txt`:
```
pip install numpy, matplotlib, pandas, paretoset, pymatgen, scikit-learn, scipy, sympy, mp_api, xgboost
```

For conda users, run:
```
conda install -c conda-forge numpy, matplotlib, pandas, paretoset, pymatgen, scikit-learn, scipy, sympy, mp_api, xgboost
```

## Usage
### Training data
`MATSCI_176_Project_Data.xlsx`

Training data is obtained from the [Materials Project](https://next-gen.materialsproject.org/) by running 
`Download_from_MP_API_updated.ipynb`. The first 6 columns are for battery categorization via working ion, elements, formula charge, etc. Columns 7-14 are scraped from Materials Projeect and are available features for performance estimation. An additional three columns corresponding to the lattice constants are added to the training dataset, which are obtained from the structure column.

The final training dataset consists of the following features: 'max_delta_volume', 'average_voltage', 'capacity_grav', 'stability_discharge', 'material_density', 'formation_energy_per_atom', 'band_gap', 'a', 'b', 'c'.

### Testing data 
`mp_battery_test_data.csv`

Testing dataset is obtained by splicing together gravimetric capacity, gravimetric energy density, voltage, and maximum change in volume data from [Moses, I.A. et al](https://www.sciencedirect.com/science/article/pii/S0378775322009570) and the Materials Project database. A subset of the paper's samples were chosen to use as testing data. The remaining features present in the training data were obtained by scraping the features from Materials Project using the chosen samples' Materials Project ID. The true absolute gravimetric energy densities are stored for model evaluation and are unsued for testing.

## Training and using the model
Model training and testing occurs in `machine_learning.ipynb`. 
Before running, confirm the following lines of code in cell $2$ are present for the program to access the [training data](#Training data) and the [testing data](#Testing data) on your device:
```
pdata = pd.read_excel("MATSCI_176_Project_Data.xlsx")
test_data = test_data = pd.read_csv("mp_battery_test_data.csv")
```


Run `machine_learning.ipynb` to:
- Clean training data by removing all nan, empty, or invalid samples. Subsequently, normalizes remaining samples.
- Trains our Ridge, ElasticNet, and XG-Boost models.
- Evaluates models by comparing $R^2$ values, mean average error, and the mean standard error from training dataset.
- Applies trained models to test dataset and returns predicted gravimetric energy densities as well as feature weights and model evaluation metrics from test dataset.
## Customization

### Changing what datasets to use for training 
To change what datasets are used to train the models, change `Download_from_MP_API_updated.ipynb` to scrape different data from Materials Project. To ensure proper functionality of `Download_from_MP_API_updated.ipynb`, please change the following variable definition in cell 1 to your API key:

````
API_KEY = "YOUR API KEY"
````

You can obtain an API key by signing into Materials Project and going to your dashboard, which will have your public API key displayed.

To extract only a subset of materials based on `working_ion`, change the `working_ion` by adding or removing working ions from the list. To change the extracted datasets, change the `fields` list by adding or subtracting available dataset names. See the [Materials Project API List](https://api.materialsproject.org/docs#/Materials%20Summary) for additional details on available datasets from Materials Project.  

## Contact

For inquiries regarding the project or bugs in our code, feel free to email us at:

- Nandagopal Pradeep Kumar, nandu02@stanford.edu
- Allen Qiang, aqiang@stanford.edu
- Alay Shah, alayshah@stanford.edu
Additionally, don't hestitate to open up issues for this repository!
