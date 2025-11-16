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

Finally at the inference stage we bring the model from the Model registry on Hopsworks, predict the upcoming days based on selected features and weather forecasts and save the results. We additionally compare previous predictions to the actual data on air quality through hindcasts.

For grade E, we got a MSE of 338. For grade C however, we tried improving the prediction by additionally providing the model with features based on lagged values as well as a rolling average. The lagged values are specifically three separate features, one each for the air quality (pm25) that is either actually recorded or predicted of the previous three days. The rolling average is simply an average of these three air quality measurements or predictions. This resulted in a MSE that was significantly better at 159. In other words, this additional information regarding the previous days gives a lot better of an understanding of the current days air quality. As the feature importance shows, the rolling average was a feature that the model found largely more important than the three separate lagged values for the previous days. This could be due to a number of examples, two of which might be that it simply gives more information as a singular feature (compared 1 to 1 to the three lagged values). It shows more of a trend than one single days value. It could also be due to the fact that one previous day doesn't give the model too much information since it already has the rolling average of all three of them.

For grade A, we expanded the pipeline to take in, train on and predict air quality for multiple sensors. The sensors chosen were originally supposed to be of a Swedish city, but since the major ones were taken we explored alternatives around the world. We landed on Tokushima prefecture, in Japan, since it was relatively close in both size and amount of sensors as a larger city in Sweden. Additionally, all sensors in this area had sufficient data covering multiple years (specifically enough data for recent years), while also spanning a slightly larger area. We first tried a different place in Japan, but since it was a smaller city with sensors very close to one another the results, predictions and hindcasts were very similar between the sensors.
