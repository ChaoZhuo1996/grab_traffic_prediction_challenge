# grab_traffic_prediction_challenge

The code first add a ‘hypothetical’ time index column for the convenience of modelling and add lat, lon column as well, which returns a dataframe(df1). Then it adds the missing timestamps for all 1329 geohash zones and restructure the data to a new table with rows of different timestamps and columns of different zones(df35). This will be the source of the input array feeding into the LSTM model. 

Since there are so many geohash zones, simply predicting one unified demand or predicting demand for each one of them are both unreasonable. It is necessary to separate and group zones. 

The idea is to separate zones based on quantile-based demand, which is a robust way than defining parameters manually. The quantile information is in the df_desc_demand data frame.

There are 2 types of models used here with different training speed and resolution: ‘one-node’ model and ‘multi-node’ model. 
‘one-node’ model uses the average demand of selected zones, and predicted a unified demand for all those zones; ‘multi-node’ model predicted demand for each selected zones in a model. ‘one-node’ model has faster training speed and lower accuracy, which is suitable for the many low-demand regions; ‘multi-node’ model on the other hand has lower training speed and higher accuracy, so it is suitable on the zones with higher demand.  Different group of geohash zones are directed to different models independently.

The future timestamp is defined by ‘horizon’, for example, use data up to timestamp T to predict demand at timestamp T+n would require horizon==n-1, hence, the requirement for this challenge is to use the model with horizon equals to 0,1,2,3,4 separately

Running all cells in the Jupyter notebook will give a prediction output for a sample time period (which is day 16 timestamp 15:00 to day 16 timestamp 16:00 in the training data) using data from day 3 to day 15 to train. To perform prediction for a different target time period, simply change the parameter ‘start’ and ‘num_time’ in the ‘horizon_node_lstm_predict’ function, and make sure ‘start+num_time’ equals to the start timestamp of the target period. 

The RMSE is printed out and stored in the training process. The overall RMSE for all zones for any time period is about 0.07 
