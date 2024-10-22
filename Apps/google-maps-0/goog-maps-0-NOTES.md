
# [GOOGLE: Learn map-making essentials](https://console.cloud.google.com/google/maps-apis/home?project=heidless-portfolio-4)
```
# enable APIS
--
Maps Javascript API
Places API

--

```


# [Google map api with Rails](https://medium.com/@umeriqbal256/google-map-api-with-rails-60c5a6291e15)
## google
```
gcloud init

ACCOUNT: heidlessemail-8@gmail.co.uk
PROJECT: heidless-portfolio-4

```

## enable APIs
```
ConfigureConsentScreen->OAuth consent screen
--
External

'Create'

APP NAME: google-maps-0
EMAIL: heidlessemail08@gmail.com

--

```


Credentials->CreateCredentials->APIKey
--
AIzaSyBPYuL0DYnwtcMR51vQjii2WYbuHL1d_TM
--



rails _7.2.1_ new google-maps-0 -d postgresql

rails g controller Pages index

rails generate stimulus google-map



```



