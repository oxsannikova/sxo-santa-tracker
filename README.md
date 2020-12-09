# sxo-santa-tracker

This SecureX Orchestration workflow will track Santa's route and notify you via Webex Teams when he gets close to your home.

Target Group: Default Target Group

Targets Used: GOOGLE MAPS API, Webex Teams

Steps:
[] Fetch coordinates and place_id for the input address
[] Fetch Santa's Route
[] Loop through each stop and get destination between Santa's stop and input location
[] If the distance has been calculated and it is less than 100 km, post a card to Webex Teams with Santa's location details.
