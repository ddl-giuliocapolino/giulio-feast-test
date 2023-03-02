## Domino feature store sample workflow: select the driver based on the performance


## Section 1 - Set up the feature store

### Set up the feature store using snowflake offline store and online store
https://dominodatalab.atlassian.net/wiki/spaces/~802751909/pages/2042331195/Datasources+and+Where+to+Find+Them#Snowflake
Note, raw data has been added to the Snow flake data source FEAST database table DRIVER_STATS


## Section 2 - Enable feature store to a demo project


## Section 3 - Define and publish features

Start a workspace, access the mounted feast git repo, add the feature definitions and publish the features
https://github.com/ddl-joyce-zhao/feast-test-data/blob/main/feast-snowflake/features.py



## Section 4 - Create the driver performance model

In the workspace, create a folder under ``/mnt/driver_stats_performance`.

Create the train script which will use the historical features.
https://github.com/ddl-joyce-zhao/feast-test-data/blob/main/driver_stats_performance/train.py

Create the predict script which will use the online features
https://github.com/ddl-joyce-zhao/feast-test-data/blob/main/driver_stats_performance/predict.py


Make sure to sync those changes to domino.


## Section 5 - Train the model

Start a job to run the `train.py` script

## Section 6 - Periodically materialize data to online store

Copy the script file https://github.com/ddl-joyce-zhao/feast-test-data/blob/main/driver_stats_performance/materialize.sh to the folder /mnt/driver_stats_performance.

Start a scheduled job to periodically materialize data to online store.

## Section 7 - Publish the model to use online features for prediction

Create a Model API to call the function `predict` of the file `driver_performance/predict_model.py`

In the model's Settings page, make sure adding the required environment variables

* FEAST_SNOWFLAKE_USER
* FEAST_SNOWFLAKE_PASSWORD
* FEAST_SNOWFLAKE_ONLINE_USER
* FEAST_SNOWFLAKE_ONLINE_PASSWORD


The required environment variables can be found in the feature_store.yaml file. In the domino workspace and job, these have been set properly from user environment variables. But in the model API, they have to be set manually.


## Section 8 - Model monitoring