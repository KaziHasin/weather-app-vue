
# Weather App Vue

a cool weather app that displays current conditions in your location. In addition, it shows the forecast for the next three days. Also Your city and country names.
When your open this app on browser it asks for permission to get your location. If you deny then you cannot get the weather of your current location when your first time open the app.
I used  navigator.geolocation.getCurrentPosition of javascirpt and Geocoding API from https://opencagedata.com/ for fetching user's current city. 

See the app 
https://my-vue-app-69e3a.web.app/

 



## API Reference

#### Get all weatherapi from rapidapi

```http
  GET https://weatherapi-com.p.rapidapi.com/forecast.json
```

| Parameter | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `city` | `string` | **Required**. Your city key |
| `days` | `string` | **Required**. Your days key if want to get forecast |

`api_key` | `string` | **Required**. Your API key |

#### Get city by passing latitude and longitude

```http
  GET https://api.opencagedata.com/geocode/v1/json?q=${latitude} ${longitude}&key=${apiKey}`
```




## Use

clone or fetch

npm install

npm run dev