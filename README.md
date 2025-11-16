# mlfs-book
O'Reilly book - Building Machine Learning Systems with a feature store: batch, real-time, and LLMs


## ML System Examples


[Dashboards for Example ML Systems](https://featurestorebook.github.io/mlfs-book/)


# Run Air Quality Tutorial

See [tutorial instructions here](https://docs.google.com/document/d/1YXfM1_rpo1-jM-lYyb1HpbV9EJPN6i1u6h2rhdPduNE/edit?usp=sharing)
    
# Create a conda or virtual environment for your project before you install the requirements
    pip install -r requirements.txt


##  Run pipelines with make commands

    make aq-backfill
    make aq-features
    make aq-train
    make aq-inference
    make aq-clean

or 
    make aq-all



## Feldera


mkdir -p /tmp/c.app.hopsworks.ai
ln -s  /tmp/c.app.hopsworks.ai ~/hopsworks
docker run -p 8080:8080 \
  -v ~/hopsworks:/tmp/c.app.hopsworks.ai \
  --tty --rm -it ghcr.io/feldera/pipeline-manager:latest


## Introduction to ML
I wrote a brief introduction to machine learning [here](./introduction_to_supervised_ml.pdf)

## Report of labwork, as requested in the instructions

For the E part/grade, we first of all simply followed the instructions and ran the code. This part mainly consisted of getting the necessary APIs, keys and secrets set up to make the already implemented code work together. We did have to uncomment some code to properly create the scheduling part besides the secrets added to actions in github. We also downloaded air-quality data from AQICN, the website with the necessary weather station data used in the lab, and put in the data folder as csv files. The code itself also downloads necessary weather data from Open Meteo. This data is registered to the feature store at Hopsworks as feature groups. 

The next part of the base (E-grade) concerns the current days worth of data, to allow for daily updates without having to worry too much about the previous data. It takes in data through the APIs and updates the feature groups at Hopsworks.

With the data and features at hand through the feature store, we can then train a XGBoost regressor on the selected features of our choice as well as the training data which is a split of the original data based on a hard coded date. We save the model to Hopsworks together with the feature importance, MSE and R2 values.

Finally at the inference stage we bring the model 