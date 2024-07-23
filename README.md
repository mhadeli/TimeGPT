# TimeGPT for Time-Series Forecasting

TimeGPT is a production-ready generative pretrained transformer for time series. It’s capable of accurately predicting various domains such as retail, electricity, finance, and IoT with just a few lines of code.

It is user-friendly and low-code. Users can simply upload their time series data and generate forecasts or detect anomalies with just a single line of code.

TimeGPT is the only out of-the-box foundation model for time series that can be used through our public APIs, through Azure Studio (coming soon) or on your own infrastructure.

## Features and capabilities

- **Zero-shot Inference**: TimeGPT can generate forecasts and detect anomalies straight out of the box, requiring no prior training data. This allows for immediate deployment and quick insights from any time series data.
- **Fine-tuning**: Enhance TimeGPT’s capabilities by fine-tuning the model on your specific datasets, enabling the model to adapt to the nuances of your unique time series data and improving performance on tailored tasks.
- **API Access**: Integrate TimeGPT seamlessly into your applications via our robust API. Upcoming support for Azure Studio will provide even more flexible integration options. Alternatively, deploy TimeGPT on your own infrastructure to maintain full control over your data and workflows.
- **Add Exogenous Variables**: Incorporate additional variables that might influence your predictions to enhance forecast accuracy. (E.g. Special Dates, events or prices)
- **Multiple Series Forecasting**: Simultaneously forecast multiple time series data, optimizing workflows and resources.
- **Custom Loss Function**: Tailor the fine-tuning process with a custom loss function to meet specific performance metrics.
- **Cross-validation**: Implement out of the box cross-validation techniques to ensure model robustness and generalizability.
- **Prediction Intervals**: Provide intervals in your predictions to quantify uncertainty effectively.
- **Irregular Timestamps**: Handle data with irregular timestamps, accommodating non-uniform interval series without preprocessing.
- **Anomaly Detection**: Automatically detect anomalies in time series, and use exogenous features for enhanced performance.

Source: [Nixtla Documentation](https://docs.nixtla.io/docs/getting-started-about_timegpt)

## Getting Started

### Step 1: Create a TimeGPT account and generate your API key

- Go to [dashboard.nixtla.io](https://dashboard.nixtla.io)
- Sign in with Google, GitHub or your email
- Create your API key by going to ‘API Keys’ in the menu and clicking on ‘Create New API Key’
- Your new key will appear. Copy the API key using the button on the right.

### Step 2: Install Nixtla

```bash
pip install nixtla
```

### Step 3: Import the Nixtla TimeGPT client

```python
from nixtla import NixtlaClient

nixtla_client = NixtlaClient(
    api_key = 'your_api_key_here'
)

# Check your API key status with the validate_api_key method.
nixtla_client.validate_api_key()
```

### Step 4: Start making forecasts!

Sample data from Kaggle: [Electric Production](https://www.kaggle.com/datasets/kandij/electric-production)

```python
import pandas as pd

df = pd.read_csv('Electric_Production.csv')
df.head()
df.tail()

nixtla_client.plot(df, time_col='DATE', target_col='Value')
```

### Forecast a longer horizon into the future

Next, forecast the next 12 months using the SDK `forecast` method. Set the following parameters:

- `df`: A pandas DataFrame containing the time series data.
- `h`: Horizons is the number of steps ahead to forecast.
- `freq`: The frequency of the time series in Pandas format. See pandas’ available frequencies. (If you don’t provide any frequency, the SDK will try to infer it)
- `time_col`: The column that identifies the datestamp.
- `target_col`: The variable to forecast.

```python
timegpt_fcst_df = nixtla_client.forecast(df=df, h=12, freq='MS', time_col='DATE', target_col='Value')
timegpt_fcst_df.head()
nixtla_client.plot(df, timegpt_fcst_df, time_col='DATE', target_col='Value')
```

You can also produce longer forecasts by increasing the horizon parameter and selecting the `timegpt-1-long-horizon` model. Use this model if you want to predict more than one seasonal period of your data.

For example, let’s forecast the next 36 months:

```python
timegpt_fcst_df = nixtla_client.forecast(df=df, h=36, time_col='DATE', target_col='Value', freq='MS', model='timegpt-1-long-horizon')
timegpt_fcst_df.head()
```

### Produce a shorter forecast

```python
timegpt_fcst_df = nixtla_client.forecast(df=df, h=6, time_col='DATE', target_col='Value', freq='MS')
nixtla_client.plot(df, timegpt_fcst_df, time_col='DATE', target_col='Value')
```
