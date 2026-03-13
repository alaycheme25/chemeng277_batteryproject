# A Physics-Informed Machine Learning Framework for Predicting Performance of Battery Electrodes

The global effort to reduce greenhouse gas emissions relies on research for better energy storage options. Batteries that store electrical energy are especially important, as green power sources like solar or wind may not be generating electricity 100% of the time. Therefore, optimizing battery performance is key to transitioning towards green power.

This project uses Ridge and ElasticNet regression in addition to XGBoost to identify and predict the effective gravimetric energy density (GED) of various battery electrodes. The training data was obtained from the [Materials Project](https://next-gen.materialsproject.org/) database using the mp-api Python package and filtered to include the materials that have Li, Na, Mg, or Ca as the working ion. The model is trained to predict the absolute gravimetric energy density for proposed battery materials. 

## Setup
Our machine-learning model is trained on data from [Materials Project](https://next-gen.materialsproject.org/), so you are not required to obtain your own data. However, the model requires certain packages to be installed to function properly. Additional instructions for using the data scrapers and building your own datasets can be found in [Customization](#Customization)

To bulk install them all, download `requirements.txt` and open terminal to the folder in which it is located:
```
pip install -r requirements.txt
```
or if you don't want to download `requirements.txt`:
```
pip install numpy, matplotlib, pandas, pymatgen, scikit-learn, scipy, sympy, mp_api, xgboost
```

For conda users, run:
```
conda install -c conda-forge numpy, matplotlib, pandas, pymatgen, scikit-learn, scipy, sympy, mp_api, xgboost
```

## Usage
### Training data
`mp_battery_training_data.xlsx`

Training data is obtained from the [Materials Project](https://next-gen.materialsproject.org/) by running 
`Download_from_MP_API_updated.ipynb`. The first 6 columns are for battery categorization via working ion, elements, formula charge, etc. Columns 7-14 are scraped from Materials Project and are available features for performance estimation. An additional three columns corresponding to the lattice constants are added to the training dataset, which are obtained from the structure column.

The final training dataset consists of the following features: 'max_delta_volume', 'average_voltage', 'capacity_grav', 'stability_discharge', 'material_density', 'formation_energy_per_atom', 'band_gap', 'a', 'b', 'c'.

### Testing data 
`mp_battery_test_data.csv`

The testing dataset is obtained by splicing together gravimetric capacity, gravimetric energy density, voltage, and maximum change in volume data from [Moses, I.A. et al](https://www.sciencedirect.com/science/article/pii/S0378775322009570) and the Materials Project database. A subset of the paper's samples were chosen to use as testing data. The remaining features present in the training data were obtained by scraping the features from Materials Project using the chosen samples' Materials Project ID. The true absolute gravimetric energy densities are stored for model evaluation and are unused for testing.

## Training and using the model
Model training and testing occur in `machine_learning.ipynb`. 
Before running, confirm the following lines of code in cell $2$ are present for the program to access the [training data](#Training data) and the [testing data](#Testing data) on your device:
```
pdata = pd.read_excel("mp_battery_training_data.xlsx")
test_data = test_data = pd.read_csv("mp_battery_test_data.csv")
```


Run `machine_learning.ipynb` to:
- Clean training data by removing all NaN, empty, or invalid samples. Normalize the data before training.
- Trains our Ridge, ElasticNet, and XGBoost models.
- Evaluates models by comparing $R^2$ values, mean average error, and the mean standard error from the training dataset.
- Applies trained models to the test dataset and returns predicted gravimetric energy densities as well as feature weights and model evaluation metrics from the test dataset.
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

Additionally, don't hesitate to open up issues for this repository!
