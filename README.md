# mistral
Easy time handling in the context of weather forecasting

# Concepts

ForecastTime integrates the concept of two central notions of time in a weather forecasting scenario.
anatime is the time when a numerical weather prediction model is initialized.
validtime is the time when the weather forecast is to be validated towards observations.
Between these times is a time interval which ForecastTime keeps track of in seconds, but
since hours is a more common time unit the interface of this time interval is given in hours
fchours denotes that time interval.
The time stamps are time zone naive.

# Usage

## Initializing

~~~~python
from mistral import ForecastTime
ft=ForecastTime()
ft.set_anatime('2018122800')
ft.set_fchours(24)
~~~~

