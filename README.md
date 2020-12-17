# sxo-santa-tracker

This SecureX Orchestration workflow will track Santa's route and notify you via Webex Teams when he gets close to your home.

Here is the video overview: https://www.youtube.com/embed/FOnFrV35S6o

## APIs Used
- [Google Maps Geolocation API](https://developers.google.com/maps/documentation/geolocation/overview)
- [Google Maps Distance Matrix API, Google Places API](https://developers.google.com/maps/documentation/distance-matrix/overview)
- [Undocumented API with Santa's Route JSON file](https://storage.googleapis.com/santa/route-v1/santa_en.json)
- [Webex Teams API](https://developer.webex.com/)

## Target Group

Default Target Group

## Targets Used

GOOGLE MAPS API, Webex Teams

## Workflow Steps

- Fetch coordinates and place_id for the input address
- Fetch Santa's Route
- Loop through each stop and get destination between Santa's stop and input location
- If the distance has been calculated and it is less than 100 km, post a card to Webex Teams with Santa's location details.

## Prerequisites

- Set up GCP account and review the Google Maps API [authentication requirements](https://developers.google.com/maps/documentation/geocoding/get-api-key) (you need an API key) and the API [usage and billing information](https://developers.google.com/maps/documentation/geocoding/usage-and-billing) (you need to enable billing on your project).
- [Set up](https://www.webex.com/team-collaboration.html) Webex Team Account.
- [Create Webex Teams Space](https://help.webex.com/en-us/nha7emp/Webex-Create-a-Team-Space#:~:text=Go%20to%20Teams%20and%20then,to%20add%20a%20space%20to.&text=Select%20Create%20a%20space%2C%20name,then%20press%20Enter%20or%20click%20.&text=1-,Go%20to%20Teams%20and%20then%20choose%20the%20team,to%20add%20a%20space%20to.&text=Select%20New%20space%2C%20name%20the,then%20press%20Enter%20or%20click%20.).

## Setup instructions

### Step 1. Set up Webex Teams Target

In order to send notifications via Webex Teams, the target needs to be configured to reach it's API. In SecureX Orchestration, go to Targets in the left hand side menu, select New Target, and apply configuration according to the screenshot below.

> **Make sure the display name of the target matches exactly (Webex Teams).**

![](/assets/webex_teams_target.png)

### Step 2. Create Webex Teams Room for notifications

> It is assumed that Webex Teams Room for notifications will be created in advance (see prerequisites section).

Create Teams Room and add all necessary people. SXO will need to know the Room ID in order to send notifications. The easiest way to find it out - add `roomid@webex.bot` to your room and it will reply to you with the private message containing the room id.

RoomID bot is developed by Cisco to assist Developers with obtaining Room ID information.

1. Add Bot to your room:

![](/assets/add_roomid_bot.png)

2. The bot will send you the Room ID in a private message:

![](/assets/room_id.png)

In SecureX orchestration, choose in the menu on the left Variables -> Global Variables -> New Variable.

![](/assets/variables.png)

Create new variable of string type with a unique display name (e.g. "Santa Tracking Space"), paste Room ID as a value and click Submit.

![](/assets/new_variable.png)

### Step 3. Obtain Webex Teams API Token

In order to send the Webex Teams messages, you have two options:
  - **Option 1 (For tests only):** Use your own Webex Teams API token that will need to be updated manually every 12 hours.
  - **Option 2 (Recommended for production):** Use Webes Teams Bot to send messages.
      - Create Webex Teams Bot following these instructions: [Create a Bot](https://developer.webex.com/docs/bots)
      - Record its API Token
      - Add Bot to the Webex Teams Room so that it can send notifications.
      
### Step 4. Import Atomic Workflows

SXO workflow is represented by the file in JSON format, that contains definitions and description of all the activities, targets, variables and atomic workflows that are in use.
In SecureX orchestration left hand-side menu, go to Workflows -> Atomic Actions -> Import -> Browse and import the atomic workflows.

![](/assets/atomics.png)

Import atomic workflows downloaded from this repository.

![](/assets/import_wf.png)

> You can either clone repository to your workstation using `git clone` command or simply copy and paste the content of it into the workflow import dialog box.

### Step 5. Import the main workflow (Santa Tracker)

In SecureX orchestration left hand-side menu, go to Workflows -> My Workflows -> Import -> Browse and import the workflow called __sxo-santa-tracker-workflow__.

You will be presented with the following warning:

![](/assets/import_warning.png)

Don't get scared and click "Update" :)

4. This is where you will provide your Webex Token:

![](/assets/token_request.png)

Copy your personal Webex API Token or your Bots' API Token into the VALUE field. This is Secure String variable and it will be stored securely in the SXO.

5. You should see the new workflow being added to the list. Click on the workflow when import is complete.

6. If import was successful, you should see zero warnings at the top of the workflow canvas.

![](/assets/inside_workflow.png)

## Step 5. Run the workflow

1. To execute the workflow, click RUN at the right top corner in the action pane.

![](/assets/action_pane.png)

2. You will be prompted to provide input information.

![](/assets/input_variables.png)

3. Provide necessary information and click `RUN`.

4.  Observe workflow execution.

As the workflow progresses, you should see activities turning green. Don't be alarmed if some activities turn red, it is expected behavior.

5. Once execution is complete, examine the Webex Teams Room and Email Inbox for notifications and check if Threat Response Casebook has been created/updated in SecureX Ribbon.

> You can return to previous runs information by clicking `VIEW RUNS` inside the workflow or going to __Runs__ in the left hand-side menu.
