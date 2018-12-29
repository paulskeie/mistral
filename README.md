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

The user can choose to initialize the class with no parameters and set time attributes with setters like this:

~~~~python
from mistral import ForecastTime
ft=ForecastTime()
ft.set_anatime('2018122800')
ft.set_fchours(24)
~~~~

The same can be achieved with

~~~~python
from mistral import ForecastTime
ft=ForecastTime(anatime='2018122800',fchours=24)
~~~~

Or if you want to provide validtime instead of fchours:

~~~~python
from mistral import ForecastTime
ft=ForecastTime(anatime='2018122800',validtime='2018122900')
~~~~

## Optional datetime formatting guesses

You may choose to initialize datetimes with strings or with python datetime objects.

When you set datetimes with strings ForecastTime needs to figure out the string formatting. This is achieved by trying format strings from a list. The first formatting that produces a valid datetime object without trowing an error is chosen.  This may seem a bit sloppy, but the list of string formattings to try may be be provided by the user and it may only contain one formatting string.

This is a the list that will be used if the user chooses to go with the defaults.


~~~~python
somedatetimeformats=[
    '%Y%m%d%H',
    '%Y%m%d%H',
    '%Y%m%d %H',
    '%Y%m%d%H%M',
    '%Y%m%d%H%M%S',
    '%Y-%m-%d %H',
    '%Y-%m-%d',
    '%Y-%m-%d %H:%M:%S',
    '%Y-%m-%d %H:%M:%SZ',
    '%Y-%m-%dT%H:%M:%SZ',
    '%Y-%m-%d %H%M%SZ',
    '%m-%d-%Y %H:%M:%S'
]
~~~~

What actually happens is that, if datetimes are provided as strings, the function guess_forecast_datetime_from_string is called.
This function can be 

~~~~python
from mistral import guess_forecast_datetime_from_string,somedatetimeformats

guess_forecast_datetime_from_string('2010-01-01 12',datetimeformats=somedatetimeformats,verbose=False)

# Returns
[(5, '%Y-%m-%d %H', datetime.datetime(2010, 1, 1, 12, 0))]
~~~~

If you want to download some real world weather forecasts, they are provided for free or for a charge and they often come in files where anatime, validtime or fchours is encoded in the filename.

ForecastTime to the resque:

~~~~python
gfs_url_template='http://www.ftp.ncep.noaa.gov/data/nccf/com/gfs/prod/gfs.{anatime:%Y%m%d%H}/gfs.t12z.pgrb2.0p25.f{fchours:03d}'
anatime=datetime.now().strftime('%Y%m%d00')
ft = ForecastTime(anatime=anatime)
for h in range(0,24,3):
  ft.set_fchours(h)
  print(ft)
  print( gfs_url_template.format(**ft.get_dict()) )
~~~~
ForecastTime,anatime:2018-12-28 00:00:00,validtime:2018-12-28 00:00:00,fchours:0
http://www.ftp.ncep.noaa.gov/data/nccf/com/gfs/prod/gfs.2018122800/gfs.t12z.pgrb2.0p25.f000

ForecastTime,anatime:2018-12-28 00:00:00,validtime:2018-12-28 03:00:00,fchours:3
http://www.ftp.ncep.noaa.gov/data/nccf/com/gfs/prod/gfs.2018122800/gfs.t12z.pgrb2.0p25.f003

ForecastTime,anatime:2018-12-28 00:00:00,validtime:2018-12-28 06:00:00,fchours:6
http://www.ftp.ncep.noaa.gov/data/nccf/com/gfs/prod/gfs.2018122800/gfs.t12z.pgrb2.0p25.f006

ForecastTime,anatime:2018-12-28 00:00:00,validtime:2018-12-28 09:00:00,fchours:9
http://www.ftp.ncep.noaa.gov/data/nccf/com/gfs/prod/gfs.2018122800/gfs.t12z.pgrb2.0p25.f009

ForecastTime,anatime:2018-12-28 00:00:00,validtime:2018-12-28 12:00:00,fchours:12
http://www.ftp.ncep.noaa.gov/data/nccf/com/gfs/prod/gfs.2018122800/gfs.t12z.pgrb2.0p25.f012

ForecastTime,anatime:2018-12-28 00:00:00,validtime:2018-12-28 15:00:00,fchours:15
http://www.ftp.ncep.noaa.gov/data/nccf/com/gfs/prod/gfs.2018122800/gfs.t12z.pgrb2.0p25.f015

ForecastTime,anatime:2018-12-28 00:00:00,validtime:2018-12-28 18:00:00,fchours:18
http://www.ftp.ncep.noaa.gov/data/nccf/com/gfs/prod/gfs.2018122800/gfs.t12z.pgrb2.0p25.f018

ForecastTime,anatime:2018-12-28 00:00:00,validtime:2018-12-28 21:00:00,fchours:21
http://www.ftp.ncep.noaa.gov/data/nccf/com/gfs/prod/gfs.2018122800/gfs.t12z.pgrb2.0p25.f021
