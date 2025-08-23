# Mobile

1. [iOS](ios.md)
2. [Android](android.md)

## Firebase

{% code overflow="wrap" %}
```sh
GoogleService-Info.plist
* API KEY AIzaSyAs1[...]
* APP ID 1:612345678909:ios:c212345678909876

curl -v -X POST "https://firebaseremoteconfig.googleapis.com/v1/projects/612345678909/namespaces/firebase:fetch?key=AIzaSyAs1[...]" -H "Content-Type: application/json" --data '{"appId": "1:612345678909:ios:c212345678909876", "appInstanceId": "PROD"}'

# REF: https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/buckets/firebase-database
```
{% endcode %}

## Google Map API

{% code overflow="wrap" %}
```bash
https://maps.googleapis.com/maps/api/geocode/json?latlng=40,30&key=[GOOGLE_MAP_API]
```
{% endcode %}
