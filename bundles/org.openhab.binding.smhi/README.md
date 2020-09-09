# Smhi Binding

This binding gets hourly and daily forecast from SMHI - the Swedish Meteorological and Hydrological Institute. 
It can get forecasts for the nordic countries (Sweden, Norway, Denmark and Finland).

## Supported Things

The binding support only one thing-type: forecast. 
The thing can be configured to get hourly forecasts for up to 24 hours, and daily forecasts for up to 10 days.


## Discovery

This binding does not support automatic discovery.

## Thing Configuration

The forecast thing needs to be configured with the latitude and longitude for the location of the forecast. 
You can also choose for which hours and which days you would like to get forecasts.

| Parameter | Description | Required |
|-----------|-------------|----------|
| Latitude  | Latitude of the forecast | Yes |
| Longitute | Longitude of the forecast | Yes |
| Hourly forecasts | The hourly forecasts to display | No |
| Daily forecasts | The daily forecasts to display | No |

## Channels

The channels are the same for all forecasts: 

#### Basic channels

| channel  | type   | description                  |
|----------|--------|------------------------------|
| Temperature  | Number:Temperature | Temperature in Celsius  |
| Wind direction  | Number:Angle | Wind direction in degrees  |
| Wind Speed  | Number:Speed | Wind speed in m/s  |
| Wind gust speed  | Number:Speed | Wind gust speed in m/s  |
| Minimum precipitation  | Number:Speed | Minimum precipitation intensity in mm/h  |
| Maximum precipitation  | Number:Speed | Maximum precipitation intensity in mm/h  |
| Precipitation category*  | Number | Type of precipitation  |
| Air pressure  | Number:Pressure | Air pressure in hPa  |
| Relative humidity  | Number:Dimensionless | Relative humidity in percent  |
| Total cloud cover  | Number:Dimensionless | Mean value of total cloud cover in percent  |
| Weather condition**  | Number | Short description of the weather conditions  |

#### Advanced channels

| channel  | type   | description                  |
|----------|--------|------------------------------|
| Visibility  | Number:Length | Horizontal visibility in km  |
| Thunder probability  | Number:Dimensionless | Probability of thunder in percent  |
| Frozen precipitation  | Number:Dimensionless | Percent of precipitation in frozen form (will be set to UNDEF if there's no precipitation)  |
| Low level cloud cover  | Number:Dimensionless | Mean value of low level cloud cover (0-2500 m) in percent  |
| Medium level cloud cover  | Number:Dimensionless | Mean value of medium level cloud cover (2500-6000 m) in percent  |
| High level cloud cover  | Number:Dimensionless | Mean value of high level cloud cover (> 6000 m) in percent  |
| Mean precipitation  | Number:Speed | Mean precipitation intensity in mm/h  |
| Median precipitation  | Number:Speed | Median precipitation intensity in mm/h  |

\* The precipitation category can have a value from 0-6, representing different types of precipitaion:

| Value | Meaning |
|-------|---------|
| 0 | No precipitation|
| 1 | Snow |
| 2 | Snow and rain |
| 3 | Rain |
| 4 | Drizzle |
| 5 | Freezing rain |
| 6 | Freezing drizzle |

\** The weather condition channel can take values from 1-27, each corresponding to a different weather condition:

| Value | Condition |
|-------|-----------|
| 1 | Clear sky |
| 2 | Nearly clear sky |
| 3 | Variable cloudiness |
| 4 | Halfclear sky |
| 5 | Cloudy sky |
| 6 | Overcast |
| 7 | Fog |
| 8 | Light rain showers |
| 9 | Moderate rain showers |
| 10 | Heavy rain showers |
| 11 | Thunderstorm |
| 12 | Light sleet showers |
| 13 | Moderate sleet showers |
| 14 | Heavy sleet showers |
| 15 | Light snow showers |
| 16 | Moderate snow showers |
| 17 | Heavy snow showers |
| 18 | Light rain |
| 19 | Moderate rain |
| 20 | Heavy rain |
| 21 | Thunder |
| 22 | Light sleet |
| 23 | Moderate sleet |
| 24 | Heavy sleet |
| 25 | Light snowfall |
| 26 | Moderate snowfall |
| 27 | Heavy snowfall |


## Full Example

demo.things

```
Thing smhi:forecast:demoforecast "Demo forecast" [ latitude=57.997072, longitude=15.990068, hourlyForecasts=0,1,2, dailyForecasts=0,1 ]
```

demo.items

```
Number:Temperature Smhi_Temperature_Now "Current temperature [%.1f °C]" {channel="smhi:forecast:demoforecast:hour_0#t"}
Number:Speed Smhi_Min_Precipitation_Now "Current precipitation (min) [%.1f mm/h]" {channel="smhi:forecast:demoforecast:hour_0#pmin"}

Number:Temperature Smhi_Temperature_1hour "Temperature next hour [%.1f °C]" {channel="smhi:forecast:demoforecast:hour_1#t"}
Number:Speed Smhi_Min_Precipitation_1hour "Precipitaion next hour (min) [%.1f mm/h]" {channel="smhi:forecast:demoforecast:hour_1#pmin"}

Number:Temperature Smhi_Temperature_Tomorrow "Temperature tomorrow [%.1f °C]" {channel="smhi:forecast:demoforecast:day_1#t"}
Number:Speed Smhi_Min_Precipitation_Tomorrow "Precipitaion tomorrow (min) [%.1f mm/h]" {channel="smhi:forecast:demoforecast:hour_1#pmin"}
```

demo.sitemap

```
sitemap demo label="Smhi" {
    Frame label="Current weather" {
        Text item=Smhi_Temperature_Now
        Text item=Smhi_Min_Precipitation_Now
    }
    Frame label="Weather next hour" {
        Text item=Smhi_Temperature_1hour
        Text item=Smhi_Min_Precipitation_1hour
    }
    Frame label="Weather tomorrow" {
        Text item=Smhi_Temperature_Tomorrow
        Text item=Smhi_Min_Precipitation_Tomorrow
    }
}
```

## To use the binding in Swedish
Då väderlek och nedbördstyp skrivits på engelska så kan nedanstående användas istället för att få det på svenska.
Det är alla 24h samt alla 9 dagar, och man måste såklart inte använda alla items.

smhi.items
```
Group SMHI "Väder från SMHI"

Number:Pressure SMHI_Pressure_h0 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_0#msl"}
Number:Temperature SMHI_Temperature_h0 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#t"}
Number:Length SMHI_Visibility_h0 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#vis"}
Number:Angle SMHI_WindDirection_h0 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#wd"}
Number:Speed SMHI_WindSpeed_h0 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#ws"}
Number:Dimensionless SMHI_Humidity_h0 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#r"}
Number:Dimensionless SMHI_ThunderProbability_h0 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#tstm"}
Number:Dimensionless SMHI_CloudCover_h0 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h0 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h0 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h0 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h0 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#gust"}
Number:Speed SMHI_PrecipitationMin_h0 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#pmin"}
Number:Speed SMHI_PrecipitationMax_h0 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#pmax"}
Number:Speed SMHI_PrecipitationMean_h0 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#pmean"}
Number:Speed SMHI_PrecipitationMedian_h0 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h0 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#spp"}
Number SMHI_PrecipitationCat_h0 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#pcat"}
Number SMHI_Condition_No_h0 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_0#wsymb2"}

Number:Pressure SMHI_Pressure_h1 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_1#msl"}
Number:Temperature SMHI_Temperature_h1 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#t"}
Number:Length SMHI_Visibility_h1 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#vis"}
Number:Angle SMHI_WindDirection_h1 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#wd"}
Number:Speed SMHI_WindSpeed_h1 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#ws"}
Number:Dimensionless SMHI_Humidity_h1 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#r"}
Number:Dimensionless SMHI_ThunderProbability_h1 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#tstm"}
Number:Dimensionless SMHI_CloudCover_h1 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h1 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h1 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h1 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h1 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#gust"}
Number:Speed SMHI_PrecipitationMin_h1 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#pmin"}
Number:Speed SMHI_PrecipitationMax_h1 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#pmax"}
Number:Speed SMHI_PrecipitationMean_h1 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#pmean"}
Number:Speed SMHI_PrecipitationMedian_h1 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h1 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#spp"}
Number SMHI_PrecipitationCat_h1 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#pcat"}
Number SMHI_Condition_No_h1 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_1#wsymb2"}

Number:Pressure SMHI_Pressure_h2 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_2#msl"}
Number:Temperature SMHI_Temperature_h2 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#t"}
Number:Length SMHI_Visibility_h2 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#vis"}
Number:Angle SMHI_WindDirection_h2 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#wd"}
Number:Speed SMHI_WindSpeed_h2 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#ws"}
Number:Dimensionless SMHI_Humidity_h2 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#r"}
Number:Dimensionless SMHI_ThunderProbability_h2 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#tstm"}
Number:Dimensionless SMHI_CloudCover_h2 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h2 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h2 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h2 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h2 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#gust"}
Number:Speed SMHI_PrecipitationMin_h2 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#pmin"}
Number:Speed SMHI_PrecipitationMax_h2 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#pmax"}
Number:Speed SMHI_PrecipitationMean_h2 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#pmean"}
Number:Speed SMHI_PrecipitationMedian_h2 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h2 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#spp"}
Number SMHI_PrecipitationCat_h2 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#pcat"}
Number SMHI_Condition_No_h2 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_2#wsymb2"}

Number:Pressure SMHI_Pressure_h3 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_3#msl"}
Number:Temperature SMHI_Temperature_h3 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#t"}
Number:Length SMHI_Visibility_h3 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#vis"}
Number:Angle SMHI_WindDirection_h3 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#wd"}
Number:Speed SMHI_WindSpeed_h3 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#ws"}
Number:Dimensionless SMHI_Humidity_h3 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#r"}
Number:Dimensionless SMHI_ThunderProbability_h3 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#tstm"}
Number:Dimensionless SMHI_CloudCover_h3 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h3 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h3 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h3 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h3 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#gust"}
Number:Speed SMHI_PrecipitationMin_h3 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#pmin"}
Number:Speed SMHI_PrecipitationMax_h3 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#pmax"}
Number:Speed SMHI_PrecipitationMean_h3 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#pmean"}
Number:Speed SMHI_PrecipitationMedian_h3 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h3 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#spp"}
Number SMHI_PrecipitationCat_h3 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#pcat"}
Number SMHI_Condition_No_h3 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_3#wsymb2"}

Number:Pressure SMHI_Pressure_h4 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_4#msl"}
Number:Temperature SMHI_Temperature_h4 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#t"}
Number:Length SMHI_Visibility_h4 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#vis"}
Number:Angle SMHI_WindDirection_h4 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#wd"}
Number:Speed SMHI_WindSpeed_h4 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#ws"}
Number:Dimensionless SMHI_Humidity_h4 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#r"}
Number:Dimensionless SMHI_ThunderProbability_h4 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#tstm"}
Number:Dimensionless SMHI_CloudCover_h4 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h4 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h4 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h4 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h4 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#gust"}
Number:Speed SMHI_PrecipitationMin_h4 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#pmin"}
Number:Speed SMHI_PrecipitationMax_h4 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#pmax"}
Number:Speed SMHI_PrecipitationMean_h4 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#pmean"}
Number:Speed SMHI_PrecipitationMedian_h4 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h4 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#spp"}
Number SMHI_PrecipitationCat_h4 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#pcat"}
Number SMHI_Condition_No_h4 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_4#wsymb2"}

Number:Pressure SMHI_Pressure_h5 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_5#msl"}
Number:Temperature SMHI_Temperature_h5 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#t"}
Number:Length SMHI_Visibility_h5 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#vis"}
Number:Angle SMHI_WindDirection_h5 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#wd"}
Number:Speed SMHI_WindSpeed_h5 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#ws"}
Number:Dimensionless SMHI_Humidity_h5 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#r"}
Number:Dimensionless SMHI_ThunderProbability_h5 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#tstm"}
Number:Dimensionless SMHI_CloudCover_h5 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h5 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h5 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h5 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h5 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#gust"}
Number:Speed SMHI_PrecipitationMin_h5 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#pmin"}
Number:Speed SMHI_PrecipitationMax_h5 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#pmax"}
Number:Speed SMHI_PrecipitationMean_h5 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#pmean"}
Number:Speed SMHI_PrecipitationMedian_h5 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h5 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#spp"}
Number SMHI_PrecipitationCat_h5 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#pcat"}
Number SMHI_Condition_No_h5 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_5#wsymb2"}

Number:Pressure SMHI_Pressure_h6 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_6#msl"}
Number:Temperature SMHI_Temperature_h6 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#t"}
Number:Length SMHI_Visibility_h6 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#vis"}
Number:Angle SMHI_WindDirection_h6 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#wd"}
Number:Speed SMHI_WindSpeed_h6 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#ws"}
Number:Dimensionless SMHI_Humidity_h6 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#r"}
Number:Dimensionless SMHI_ThunderProbability_h6 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#tstm"}
Number:Dimensionless SMHI_CloudCover_h6 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h6 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h6 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h6 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h6 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#gust"}
Number:Speed SMHI_PrecipitationMin_h6 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#pmin"}
Number:Speed SMHI_PrecipitationMax_h6 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#pmax"}
Number:Speed SMHI_PrecipitationMean_h6 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#pmean"}
Number:Speed SMHI_PrecipitationMedian_h6 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h6 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#spp"}
Number SMHI_PrecipitationCat_h6 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#pcat"}
Number SMHI_Condition_No_h6 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_6#wsymb2"}

Number:Pressure SMHI_Pressure_h7 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_7#msl"}
Number:Temperature SMHI_Temperature_h7 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#t"}
Number:Length SMHI_Visibility_h7 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#vis"}
Number:Angle SMHI_WindDirection_h7 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#wd"}
Number:Speed SMHI_WindSpeed_h7 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#ws"}
Number:Dimensionless SMHI_Humidity_h7 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#r"}
Number:Dimensionless SMHI_ThunderProbability_h7 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#tstm"}
Number:Dimensionless SMHI_CloudCover_h7 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h7 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h7 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h7 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h7 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#gust"}
Number:Speed SMHI_PrecipitationMin_h7 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#pmin"}
Number:Speed SMHI_PrecipitationMax_h7 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#pmax"}
Number:Speed SMHI_PrecipitationMean_h7 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#pmean"}
Number:Speed SMHI_PrecipitationMedian_h7 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h7 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#spp"}
Number SMHI_PrecipitationCat_h7 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#pcat"}
Number SMHI_Condition_No_h7 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_7#wsymb2"}

Number:Pressure SMHI_Pressure_h8 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_8#msl"}
Number:Temperature SMHI_Temperature_h8 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#t"}
Number:Length SMHI_Visibility_h8 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#vis"}
Number:Angle SMHI_WindDirection_h8 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#wd"}
Number:Speed SMHI_WindSpeed_h8 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#ws"}
Number:Dimensionless SMHI_Humidity_h8 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#r"}
Number:Dimensionless SMHI_ThunderProbability_h8 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#tstm"}
Number:Dimensionless SMHI_CloudCover_h8 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h8 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h8 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h8 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h8 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#gust"}
Number:Speed SMHI_PrecipitationMin_h8 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#pmin"}
Number:Speed SMHI_PrecipitationMax_h8 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#pmax"}
Number:Speed SMHI_PrecipitationMean_h8 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#pmean"}
Number:Speed SMHI_PrecipitationMedian_h8 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h8 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#spp"}
Number SMHI_PrecipitationCat_h8 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#pcat"}
Number SMHI_Condition_No_h8 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_8#wsymb2"}

Number:Pressure SMHI_Pressure_h9 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_9#msl"}
Number:Temperature SMHI_Temperature_h9 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#t"}
Number:Length SMHI_Visibility_h9 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#vis"}
Number:Angle SMHI_WindDirection_h9 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#wd"}
Number:Speed SMHI_WindSpeed_h9 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#ws"}
Number:Dimensionless SMHI_Humidity_h9 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#r"}
Number:Dimensionless SMHI_ThunderProbability_h9 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#tstm"}
Number:Dimensionless SMHI_CloudCover_h9 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h9 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h9 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h9 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h9 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#gust"}
Number:Speed SMHI_PrecipitationMin_h9 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#pmin"}
Number:Speed SMHI_PrecipitationMax_h9 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#pmax"}
Number:Speed SMHI_PrecipitationMean_h9 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#pmean"}
Number:Speed SMHI_PrecipitationMedian_h9 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h9 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#spp"}
Number SMHI_PrecipitationCat_h9 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#pcat"}
Number SMHI_Condition_No_h9 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_9#wsymb2"}

Number:Pressure SMHI_Pressure_h10 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_10#msl"}
Number:Temperature SMHI_Temperature_h10 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#t"}
Number:Length SMHI_Visibility_h10 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#vis"}
Number:Angle SMHI_WindDirection_h10 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#wd"}
Number:Speed SMHI_WindSpeed_h10 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#ws"}
Number:Dimensionless SMHI_Humidity_h10 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#r"}
Number:Dimensionless SMHI_ThunderProbability_h10 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#tstm"}
Number:Dimensionless SMHI_CloudCover_h10 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h10 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h10 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h10 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h10 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#gust"}
Number:Speed SMHI_PrecipitationMin_h10 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#pmin"}
Number:Speed SMHI_PrecipitationMax_h10 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#pmax"}
Number:Speed SMHI_PrecipitationMean_h10 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#pmean"}
Number:Speed SMHI_PrecipitationMedian_h10 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h10 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#spp"}
Number SMHI_PrecipitationCat_h10 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#pcat"}
Number SMHI_Condition_No_h10 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_10#wsymb2"}

Number:Pressure SMHI_Pressure_h11 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_11#msl"}
Number:Temperature SMHI_Temperature_h11 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#t"}
Number:Length SMHI_Visibility_h11 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#vis"}
Number:Angle SMHI_WindDirection_h11 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#wd"}
Number:Speed SMHI_WindSpeed_h11 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#ws"}
Number:Dimensionless SMHI_Humidity_h11 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#r"}
Number:Dimensionless SMHI_ThunderProbability_h11 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#tstm"}
Number:Dimensionless SMHI_CloudCover_h11 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h11 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h11 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h11 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h11 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#gust"}
Number:Speed SMHI_PrecipitationMin_h11 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#pmin"}
Number:Speed SMHI_PrecipitationMax_h11 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#pmax"}
Number:Speed SMHI_PrecipitationMean_h11 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#pmean"}
Number:Speed SMHI_PrecipitationMedian_h11 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h11 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#spp"}
Number SMHI_PrecipitationCat_h11 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#pcat"}
Number SMHI_Condition_No_h11 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_11#wsymb2"}

Number:Pressure SMHI_Pressure_h12 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_12#msl"}
Number:Temperature SMHI_Temperature_h12 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#t"}
Number:Length SMHI_Visibility_h12 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#vis"}
Number:Angle SMHI_WindDirection_h12 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#wd"}
Number:Speed SMHI_WindSpeed_h12 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#ws"}
Number:Dimensionless SMHI_Humidity_h12 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#r"}
Number:Dimensionless SMHI_ThunderProbability_h12 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#tstm"}
Number:Dimensionless SMHI_CloudCover_h12 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h12 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h12 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h12 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h12 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#gust"}
Number:Speed SMHI_PrecipitationMin_h12 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#pmin"}
Number:Speed SMHI_PrecipitationMax_h12 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#pmax"}
Number:Speed SMHI_PrecipitationMean_h12 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#pmean"}
Number:Speed SMHI_PrecipitationMedian_h12 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h12 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#spp"}
Number SMHI_PrecipitationCat_h12 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#pcat"}
Number SMHI_Condition_No_h12 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_12#wsymb2"}

Number:Pressure SMHI_Pressure_h13 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_13#msl"}
Number:Temperature SMHI_Temperature_h13 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#t"}
Number:Length SMHI_Visibility_h13 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#vis"}
Number:Angle SMHI_WindDirection_h13 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#wd"}
Number:Speed SMHI_WindSpeed_h13 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#ws"}
Number:Dimensionless SMHI_Humidity_h13 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#r"}
Number:Dimensionless SMHI_ThunderProbability_h13 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#tstm"}
Number:Dimensionless SMHI_CloudCover_h13 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h13 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h13 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h13 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h13 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#gust"}
Number:Speed SMHI_PrecipitationMin_h13 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#pmin"}
Number:Speed SMHI_PrecipitationMax_h13 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#pmax"}
Number:Speed SMHI_PrecipitationMean_h13 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#pmean"}
Number:Speed SMHI_PrecipitationMedian_h13 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h13 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#spp"}
Number SMHI_PrecipitationCat_h13 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#pcat"}
Number SMHI_Condition_No_h13 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_13#wsymb2"}

Number:Pressure SMHI_Pressure_h14 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_14#msl"}
Number:Temperature SMHI_Temperature_h14 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#t"}
Number:Length SMHI_Visibility_h14 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#vis"}
Number:Angle SMHI_WindDirection_h14 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#wd"}
Number:Speed SMHI_WindSpeed_h14 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#ws"}
Number:Dimensionless SMHI_Humidity_h14 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#r"}
Number:Dimensionless SMHI_ThunderProbability_h14 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#tstm"}
Number:Dimensionless SMHI_CloudCover_h14 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h14 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h14 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h14 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h14 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#gust"}
Number:Speed SMHI_PrecipitationMin_h14 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#pmin"}
Number:Speed SMHI_PrecipitationMax_h14 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#pmax"}
Number:Speed SMHI_PrecipitationMean_h14 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#pmean"}
Number:Speed SMHI_PrecipitationMedian_h14 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h14 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#spp"}
Number SMHI_PrecipitationCat_h14 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#pcat"}
Number SMHI_Condition_No_h14 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_14#wsymb2"}

Number:Pressure SMHI_Pressure_h15 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_15#msl"}
Number:Temperature SMHI_Temperature_h15 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#t"}
Number:Length SMHI_Visibility_h15 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#vis"}
Number:Angle SMHI_WindDirection_h15 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#wd"}
Number:Speed SMHI_WindSpeed_h15 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#ws"}
Number:Dimensionless SMHI_Humidity_h15 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#r"}
Number:Dimensionless SMHI_ThunderProbability_h15 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#tstm"}
Number:Dimensionless SMHI_CloudCover_h15 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h15 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h15 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h15 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h15 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#gust"}
Number:Speed SMHI_PrecipitationMin_h15 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#pmin"}
Number:Speed SMHI_PrecipitationMax_h15 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#pmax"}
Number:Speed SMHI_PrecipitationMean_h15 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#pmean"}
Number:Speed SMHI_PrecipitationMedian_h15 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h15 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#spp"}
Number SMHI_PrecipitationCat_h15 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#pcat"}
Number SMHI_Condition_No_h15 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_15#wsymb2"}

Number:Pressure SMHI_Pressure_h16 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_16#msl"}
Number:Temperature SMHI_Temperature_h16 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#t"}
Number:Length SMHI_Visibility_h16 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#vis"}
Number:Angle SMHI_WindDirection_h16 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#wd"}
Number:Speed SMHI_WindSpeed_h16 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#ws"}
Number:Dimensionless SMHI_Humidity_h16 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#r"}
Number:Dimensionless SMHI_ThunderProbability_h16 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#tstm"}
Number:Dimensionless SMHI_CloudCover_h16 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h16 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h16 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h16 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h16 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#gust"}
Number:Speed SMHI_PrecipitationMin_h16 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#pmin"}
Number:Speed SMHI_PrecipitationMax_h16 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#pmax"}
Number:Speed SMHI_PrecipitationMean_h16 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#pmean"}
Number:Speed SMHI_PrecipitationMedian_h16 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h16 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#spp"}
Number SMHI_PrecipitationCat_h16 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#pcat"}
Number SMHI_Condition_No_h16 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_16#wsymb2"}

Number:Pressure SMHI_Pressure_h17 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_17#msl"}
Number:Temperature SMHI_Temperature_h17 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#t"}
Number:Length SMHI_Visibility_h17 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#vis"}
Number:Angle SMHI_WindDirection_h17 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#wd"}
Number:Speed SMHI_WindSpeed_h17 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#ws"}
Number:Dimensionless SMHI_Humidity_h17 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#r"}
Number:Dimensionless SMHI_ThunderProbability_h17 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#tstm"}
Number:Dimensionless SMHI_CloudCover_h17 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h17 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h17 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h17 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h17 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#gust"}
Number:Speed SMHI_PrecipitationMin_h17 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#pmin"}
Number:Speed SMHI_PrecipitationMax_h17 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#pmax"}
Number:Speed SMHI_PrecipitationMean_h17 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#pmean"}
Number:Speed SMHI_PrecipitationMedian_h17 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h17 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#spp"}
Number SMHI_PrecipitationCat_h17 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#pcat"}
Number SMHI_Condition_No_h17 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_17#wsymb2"}

Number:Pressure SMHI_Pressure_h18 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_18#msl"}
Number:Temperature SMHI_Temperature_h18 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#t"}
Number:Length SMHI_Visibility_h18 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#vis"}
Number:Angle SMHI_WindDirection_h18 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#wd"}
Number:Speed SMHI_WindSpeed_h18 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#ws"}
Number:Dimensionless SMHI_Humidity_h18 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#r"}
Number:Dimensionless SMHI_ThunderProbability_h18 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#tstm"}
Number:Dimensionless SMHI_CloudCover_h18 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h18 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h18 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h18 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h18 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#gust"}
Number:Speed SMHI_PrecipitationMin_h18 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#pmin"}
Number:Speed SMHI_PrecipitationMax_h18 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#pmax"}
Number:Speed SMHI_PrecipitationMean_h18 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#pmean"}
Number:Speed SMHI_PrecipitationMedian_h18 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h18 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#spp"}
Number SMHI_PrecipitationCat_h18 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#pcat"}
Number SMHI_Condition_No_h18 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_18#wsymb2"}

Number:Pressure SMHI_Pressure_h19 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_19#msl"}
Number:Temperature SMHI_Temperature_h19 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#t"}
Number:Length SMHI_Visibility_h19 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#vis"}
Number:Angle SMHI_WindDirection_h19 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#wd"}
Number:Speed SMHI_WindSpeed_h19 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#ws"}
Number:Dimensionless SMHI_Humidity_h19 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#r"}
Number:Dimensionless SMHI_ThunderProbability_h19 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#tstm"}
Number:Dimensionless SMHI_CloudCover_h19 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h19 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h19 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h19 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h19 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#gust"}
Number:Speed SMHI_PrecipitationMin_h19 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#pmin"}
Number:Speed SMHI_PrecipitationMax_h19 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#pmax"}
Number:Speed SMHI_PrecipitationMean_h19 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#pmean"}
Number:Speed SMHI_PrecipitationMedian_h19 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h19 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#spp"}
Number SMHI_PrecipitationCat_h19 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#pcat"}
Number SMHI_Condition_No_h19 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_19#wsymb2"}

Number:Pressure SMHI_Pressure_h20 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_20#msl"}
Number:Temperature SMHI_Temperature_h20 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#t"}
Number:Length SMHI_Visibility_h20 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#vis"}
Number:Angle SMHI_WindDirection_h20 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#wd"}
Number:Speed SMHI_WindSpeed_h20 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#ws"}
Number:Dimensionless SMHI_Humidity_h20 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#r"}
Number:Dimensionless SMHI_ThunderProbability_h20 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#tstm"}
Number:Dimensionless SMHI_CloudCover_h20 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h20 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h20 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h20 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h20 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#gust"}
Number:Speed SMHI_PrecipitationMin_h20 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#pmin"}
Number:Speed SMHI_PrecipitationMax_h20 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#pmax"}
Number:Speed SMHI_PrecipitationMean_h20 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#pmean"}
Number:Speed SMHI_PrecipitationMedian_h20 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h20 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#spp"}
Number SMHI_PrecipitationCat_h20 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#pcat"}
Number SMHI_Condition_No_h20 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_20#wsymb2"}

Number:Pressure SMHI_Pressure_h21 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_21#msl"}
Number:Temperature SMHI_Temperature_h21 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#t"}
Number:Length SMHI_Visibility_h21 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#vis"}
Number:Angle SMHI_WindDirection_h21 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#wd"}
Number:Speed SMHI_WindSpeed_h21 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#ws"}
Number:Dimensionless SMHI_Humidity_h21 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#r"}
Number:Dimensionless SMHI_ThunderProbability_h21 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#tstm"}
Number:Dimensionless SMHI_CloudCover_h21 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h21 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h21 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h21 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h21 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#gust"}
Number:Speed SMHI_PrecipitationMin_h21 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#pmin"}
Number:Speed SMHI_PrecipitationMax_h21 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#pmax"}
Number:Speed SMHI_PrecipitationMean_h21 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#pmean"}
Number:Speed SMHI_PrecipitationMedian_h21 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h21 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#spp"}
Number SMHI_PrecipitationCat_h21 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#pcat"}
Number SMHI_Condition_No_h21 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_21#wsymb2"}

Number:Pressure SMHI_Pressure_h22 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_22#msl"}
Number:Temperature SMHI_Temperature_h22 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#t"}
Number:Length SMHI_Visibility_h22 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#vis"}
Number:Angle SMHI_WindDirection_h22 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#wd"}
Number:Speed SMHI_WindSpeed_h22 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#ws"}
Number:Dimensionless SMHI_Humidity_h22 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#r"}
Number:Dimensionless SMHI_ThunderProbability_h22 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#tstm"}
Number:Dimensionless SMHI_CloudCover_h22 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h22 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h22 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h22 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h22 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#gust"}
Number:Speed SMHI_PrecipitationMin_h22 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#pmin"}
Number:Speed SMHI_PrecipitationMax_h22 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#pmax"}
Number:Speed SMHI_PrecipitationMean_h22 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#pmean"}
Number:Speed SMHI_PrecipitationMedian_h22 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h22 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#spp"}
Number SMHI_PrecipitationCat_h22 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#pcat"}
Number SMHI_Condition_No_h22 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_22#wsymb2"}

Number:Pressure SMHI_Pressure_h23 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_23#msl"}
Number:Temperature SMHI_Temperature_h23 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#t"}
Number:Length SMHI_Visibility_h23 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#vis"}
Number:Angle SMHI_WindDirection_h23 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#wd"}
Number:Speed SMHI_WindSpeed_h23 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#ws"}
Number:Dimensionless SMHI_Humidity_h23 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#r"}
Number:Dimensionless SMHI_ThunderProbability_h23 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#tstm"}
Number:Dimensionless SMHI_CloudCover_h23 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h23 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h23 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h23 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h23 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#gust"}
Number:Speed SMHI_PrecipitationMin_h23 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#pmin"}
Number:Speed SMHI_PrecipitationMax_h23 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#pmax"}
Number:Speed SMHI_PrecipitationMean_h23 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#pmean"}
Number:Speed SMHI_PrecipitationMedian_h23 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h23 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#spp"}
Number SMHI_PrecipitationCat_h23 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#pcat"}
Number SMHI_Condition_No_h23 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_23#wsymb2"}

Number:Pressure SMHI_Pressure_h24 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:hour_24#msl"}
Number:Temperature SMHI_Temperature_h24 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#t"}
Number:Length SMHI_Visibility_h24 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#vis"}
Number:Angle SMHI_WindDirection_h24 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#wd"}
Number:Speed SMHI_WindSpeed_h24 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#ws"}
Number:Dimensionless SMHI_Humidity_h24 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#r"}
Number:Dimensionless SMHI_ThunderProbability_h24 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#tstm"}
Number:Dimensionless SMHI_CloudCover_h24 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_h24 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_h24 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_h24 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_h24 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#gust"}
Number:Speed SMHI_PrecipitationMin_h24 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#pmin"}
Number:Speed SMHI_PrecipitationMax_h24 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#pmax"}
Number:Speed SMHI_PrecipitationMean_h24 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#pmean"}
Number:Speed SMHI_PrecipitationMedian_h24 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_h24 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#spp"}
Number SMHI_PrecipitationCat_h24 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#pcat"}
Number SMHI_Condition_No_h24 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:hour_24#wsymb2"}



//###### Dagar ######

Number:Pressure SMHI_Pressure_d0 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:day_0#msl"}
Number:Temperature SMHI_Temperature_d0 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#t"}
Number:Length SMHI_Visibility_d0 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#vis"}
Number:Angle SMHI_WindDirection_d0 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#wd"}
Number:Speed SMHI_WindSpeed_d0 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#ws"}
Number:Dimensionless SMHI_Humidity_d0 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#r"}
Number:Dimensionless SMHI_ThunderProbability_d0 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#tstm"}
Number:Dimensionless SMHI_CloudCover_d0 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_d0 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_d0 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_d0 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_d0 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#gust"}
Number:Speed SMHI_PrecipitationMin_d0 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#pmin"}
Number:Speed SMHI_PrecipitationMax_d0 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#pmax"}
Number:Speed SMHI_PrecipitationMean_d0 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#pmean"}
Number:Speed SMHI_PrecipitationMedian_d0 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_d0 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#spp"}
Number SMHI_PrecipitationCat_d0 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#pcat"}
Number SMHI_Condition_No_d0 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_0#wsymb2"}

Number:Pressure SMHI_Pressure_d1 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:day_1#msl"}
Number:Temperature SMHI_Temperature_d1 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#t"}
Number:Length SMHI_Visibility_d1 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#vis"}
Number:Angle SMHI_WindDirection_d1 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#wd"}
Number:Speed SMHI_WindSpeed_d1 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#ws"}
Number:Dimensionless SMHI_Humidity_d1 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#r"}
Number:Dimensionless SMHI_ThunderProbability_d1 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#tstm"}
Number:Dimensionless SMHI_CloudCover_d1 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_d1 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_d1 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_d1 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_d1 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#gust"}
Number:Speed SMHI_PrecipitationMin_d1 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#pmin"}
Number:Speed SMHI_PrecipitationMax_d1 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#pmax"}
Number:Speed SMHI_PrecipitationMean_d1 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#pmean"}
Number:Speed SMHI_PrecipitationMedian_d1 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_d1 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#spp"}
Number SMHI_PrecipitationCat_d1 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#pcat"}
Number SMHI_Condition_No_d1 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_1#wsymb2"}

Number:Pressure SMHI_Pressure_d2 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:day_2#msl"}
Number:Temperature SMHI_Temperature_d2 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#t"}
Number:Length SMHI_Visibility_d2 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#vis"}
Number:Angle SMHI_WindDirection_d2 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#wd"}
Number:Speed SMHI_WindSpeed_d2 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#ws"}
Number:Dimensionless SMHI_Humidity_d2 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#r"}
Number:Dimensionless SMHI_ThunderProbability_d2 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#tstm"}
Number:Dimensionless SMHI_CloudCover_d2 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_d2 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_d2 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_d2 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_d2 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#gust"}
Number:Speed SMHI_PrecipitationMin_d2 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#pmin"}
Number:Speed SMHI_PrecipitationMax_d2 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#pmax"}
Number:Speed SMHI_PrecipitationMean_d2 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#pmean"}
Number:Speed SMHI_PrecipitationMedian_d2 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_d2 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#spp"}
Number SMHI_PrecipitationCat_d2 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#pcat"}
Number SMHI_Condition_No_d2 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_2#wsymb2"}

Number:Pressure SMHI_Pressure_d3 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:day_3#msl"}
Number:Temperature SMHI_Temperature_d3 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#t"}
Number:Length SMHI_Visibility_d3 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#vis"}
Number:Angle SMHI_WindDirection_d3 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#wd"}
Number:Speed SMHI_WindSpeed_d3 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#ws"}
Number:Dimensionless SMHI_Humidity_d3 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#r"}
Number:Dimensionless SMHI_ThunderProbability_d3 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#tstm"}
Number:Dimensionless SMHI_CloudCover_d3 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_d3 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_d3 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_d3 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_d3 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#gust"}
Number:Speed SMHI_PrecipitationMin_d3 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#pmin"}
Number:Speed SMHI_PrecipitationMax_d3 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#pmax"}
Number:Speed SMHI_PrecipitationMean_d3 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#pmean"}
Number:Speed SMHI_PrecipitationMedian_d3 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_d3 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#spp"}
Number SMHI_PrecipitationCat_d3 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#pcat"}
Number SMHI_Condition_No_d3 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_3#wsymb2"}

Number:Pressure SMHI_Pressure_d4 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:day_4#msl"}
Number:Temperature SMHI_Temperature_d4 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#t"}
Number:Length SMHI_Visibility_d4 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#vis"}
Number:Angle SMHI_WindDirection_d4 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#wd"}
Number:Speed SMHI_WindSpeed_d4 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#ws"}
Number:Dimensionless SMHI_Humidity_d4 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#r"}
Number:Dimensionless SMHI_ThunderProbability_d4 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#tstm"}
Number:Dimensionless SMHI_CloudCover_d4 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_d4 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_d4 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_d4 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_d4 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#gust"}
Number:Speed SMHI_PrecipitationMin_d4 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#pmin"}
Number:Speed SMHI_PrecipitationMax_d4 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#pmax"}
Number:Speed SMHI_PrecipitationMean_d4 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#pmean"}
Number:Speed SMHI_PrecipitationMedian_d4 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_d4 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#spp"}
Number SMHI_PrecipitationCat_d4 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#pcat"}
Number SMHI_Condition_No_d4 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_4#wsymb2"}

Number:Pressure SMHI_Pressure_d5 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:day_5#msl"}
Number:Temperature SMHI_Temperature_d5 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#t"}
Number:Length SMHI_Visibility_d5 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#vis"}
Number:Angle SMHI_WindDirection_d5 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#wd"}
Number:Speed SMHI_WindSpeed_d5 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#ws"}
Number:Dimensionless SMHI_Humidity_d5 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#r"}
Number:Dimensionless SMHI_ThunderProbability_d5 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#tstm"}
Number:Dimensionless SMHI_CloudCover_d5 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_d5 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_d5 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_d5 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_d5 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#gust"}
Number:Speed SMHI_PrecipitationMin_d5 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#pmin"}
Number:Speed SMHI_PrecipitationMax_d5 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#pmax"}
Number:Speed SMHI_PrecipitationMean_d5 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#pmean"}
Number:Speed SMHI_PrecipitationMedian_d5 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_d5 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#spp"}
Number SMHI_PrecipitationCat_d5 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#pcat"}
Number SMHI_Condition_No_d5 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_5#wsymb2"}

Number:Pressure SMHI_Pressure_d6 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:day_6#msl"}
Number:Temperature SMHI_Temperature_d6 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#t"}
Number:Length SMHI_Visibility_d6 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#vis"}
Number:Angle SMHI_WindDirection_d6 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#wd"}
Number:Speed SMHI_WindSpeed_d6 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#ws"}
Number:Dimensionless SMHI_Humidity_d6 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#r"}
Number:Dimensionless SMHI_ThunderProbability_d6 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#tstm"}
Number:Dimensionless SMHI_CloudCover_d6 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_d6 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_d6 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_d6 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_d6 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#gust"}
Number:Speed SMHI_PrecipitationMin_d6 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#pmin"}
Number:Speed SMHI_PrecipitationMax_d6 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#pmax"}
Number:Speed SMHI_PrecipitationMean_d6 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#pmean"}
Number:Speed SMHI_PrecipitationMedian_d6 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_d6 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#spp"}
Number SMHI_PrecipitationCat_d6 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#pcat"}
Number SMHI_Condition_No_d6 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_6#wsymb2"}

Number:Pressure SMHI_Pressure_d7 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:day_7#msl"}
Number:Temperature SMHI_Temperature_d7 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#t"}
Number:Length SMHI_Visibility_d7 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#vis"}
Number:Angle SMHI_WindDirection_d7 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#wd"}
Number:Speed SMHI_WindSpeed_d7 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#ws"}
Number:Dimensionless SMHI_Humidity_d7 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#r"}
Number:Dimensionless SMHI_ThunderProbability_d7 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#tstm"}
Number:Dimensionless SMHI_CloudCover_d7 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_d7 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_d7 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_d7 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_d7 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#gust"}
Number:Speed SMHI_PrecipitationMin_d7 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#pmin"}
Number:Speed SMHI_PrecipitationMax_d7 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#pmax"}
Number:Speed SMHI_PrecipitationMean_d7 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#pmean"}
Number:Speed SMHI_PrecipitationMedian_d7 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_d7 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#spp"}
Number SMHI_PrecipitationCat_d7 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#pcat"}
Number SMHI_Condition_No_d7 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_7#wsymb2"}

Number:Pressure SMHI_Pressure_d8 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:day_8#msl"}
Number:Temperature SMHI_Temperature_d8 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#t"}
Number:Length SMHI_Visibility_d8 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#vis"}
Number:Angle SMHI_WindDirection_d8 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#wd"}
Number:Speed SMHI_WindSpeed_d8 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#ws"}
Number:Dimensionless SMHI_Humidity_d8 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#r"}
Number:Dimensionless SMHI_ThunderProbability_d8 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#tstm"}
Number:Dimensionless SMHI_CloudCover_d8 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_d8 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_d8 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_d8 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_d8 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#gust"}
Number:Speed SMHI_PrecipitationMin_d8 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#pmin"}
Number:Speed SMHI_PrecipitationMax_d8 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#pmax"}
Number:Speed SMHI_PrecipitationMean_d8 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#pmean"}
Number:Speed SMHI_PrecipitationMedian_d8 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_d8 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#spp"}
Number SMHI_PrecipitationCat_d8 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#pcat"}
Number SMHI_Condition_No_d8 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_8#wsymb2"}

Number:Pressure SMHI_Pressure_d9 "Lufttryck [%.1f hPa]" <pressure> (SMHI)  {channel="smhi:forecast:c75bc4e1:day_9#msl"}
Number:Temperature SMHI_Temperature_d9 "Temperatur [%.1f °C]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#t"}
Number:Length SMHI_Visibility_d9 "Sikt [%.1f km]" <temperature> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#vis"}
Number:Angle SMHI_WindDirection_d9 "Vindriktning [%.0f °]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#wd"}
Number:Speed SMHI_WindSpeed_d9 "Vindhastighet [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#ws"}
Number:Dimensionless SMHI_Humidity_d9 "Luftfuktighet [%.1f %%]" <humidity> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#r"}
Number:Dimensionless SMHI_ThunderProbability_d9 "Åska sanolikhet [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#tstm"}
Number:Dimensionless SMHI_CloudCover_d9 "Molntäckning [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#tcc_mean"}
Number:Dimensionless SMHI_CloudCoverLow_d9 "Molntäckning Låg [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#lcc_mean"}
Number:Dimensionless SMHI_CloudCoverMedium_d9 "MolnTäckning Medel [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#mcc_mean"}
Number:Dimensionless SMHI_CloudCoverHigh_d9 "MolnTäckning Hög [%.0f %%]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#hcc_mean"}
Number:Speed SMHI_WindGustSpeed_d9 "Byvindar [%.1f m/s]" <wind> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#gust"}
Number:Speed SMHI_PrecipitationMin_d9 "Nederbörd Min [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#pmin"}
Number:Speed SMHI_PrecipitationMax_d9 "Nederbörd Max [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#pmax"}
Number:Speed SMHI_PrecipitationMean_d9 "Nederbörd Medel [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#pmean"}
Number:Speed SMHI_PrecipitationMedian_d9 "Nederbörd Median [%.1f mm/h]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#pmedian"}
Number:Dimensionless SMHI_PrecipitationFrozen_d9 "Frysen nederbörd [%.0f %%]" <climate> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#spp"}
Number SMHI_PrecipitationCat_d9 "[MAP(smhi_precip_cat.map):%s]" <rain> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#pcat"}
Number SMHI_Condition_No_d9 "[MAP(smhi_Wsymb2.map):%s]" <sun_clouds> (SMHI) {channel="smhi:forecast:c75bc4e1:day_9#wsymb2"}
```

smhi_Wsymb2.map
```
1=Klart
2=Lätt molnighet
3=Halvklart
4=Molnigt
5=Mycket moln
6=Mulet
7=Dimma
8=Lätta regnskurar
9=Regnskurar
10=Kraftiga regnskurar
11=Åskskurar
12=Lätt byar av regn och snö
13=Byar av regn och snö
14=Kraftiga byar av regn och snö
15=Lätta snöbyar
16=Snöbyar
17=Kraftiga snöbyar
18=Lätt regn
19=Regn
20=Kraftigt regn
21=Åska
22=Lätt snöblandat regn
23=Snöblandat regn
24=Kraftigt snöblandat regn
25=Lätt snöfall
26=Snöfall
27=Ymnigt snöfall
=Okänd väderlekstyp
```

smhi_precip_cat.map
```
0=Ingen nederbörd
1=Snö
2=Snöblandat regn
3=Regn
4=Duggregn
5=Underfruset regn
6=Underfruset duggregn
=Okänd nederbördstyp
```
