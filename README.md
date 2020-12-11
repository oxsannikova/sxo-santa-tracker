# sxo-santa-tracker

This SecureX Orchestration workflow will track Santa's route and notify you via Webex Teams when he gets close to your home.

APIs Used: Google Maps Geolocation API, Google Maps Distance Matrix API, Google Places API, Webex Teams

Target Group: Default Target Group

Targets Used: GOOGLE MAPS API, Webex Teams

Steps:

- Fetch coordinates and place_id for the input address
- Fetch Santa's Route
- Loop through each stop and get destination between Santa's stop and input location
- If the distance has been calculated and it is less than 100 km, post a card to Webex Teams with Santa's location details.

Review the Google Maps API [authentication requirements](https://developers.google.com/maps/documentation/geocoding/get-api-key) (you need an API key) and the API [usage and billing information](https://developers.google.com/maps/documentation/geocoding/usage-and-billing) (you need to enable billing on your project).
