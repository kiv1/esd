To ensure this development work

To run the project:

docker-compose up -d

Prerequisite:

//AUTH0
Ensure you have ngrok installed on your computer.
This is to make your url public for webhook purpose.
Follow the guide here to install https://ngrok.com/download 

To target a specific port using ngrok: 

ngrok http 80

//STRIPE
Ensure you have localtunnel installed on your computer.
This is to make your url public for webhook purpose.
Follow the guide here to install http://localtunnel.github.io/www/ 

To target a specific port using ngrok: 

lt --port 8000

Everything has been linked up and data has been seeded for the ease of development

Things that need to be done in order for the project to work correctly

Auth0 Webhook and JWT
---------------------
1) Go to https://auth0.com/ and sign up for an account

2) Once at the dashboard hover over "Applications" on your left and click on "Applications"

3) Click on "Default App" and copy the "Domain" and "Client ID" over to your docker-compose.yml under frontend
"REACT_APP_AUTH0_DOMAIN", "REACT_APP_AUTH0_CLIENT_ID"

4) Scroll down and ensure "Application Type" is set to "Single Page Application"

5) Scroll down and ensure "Allowed Callback URLs", "Allowed Logout URLs" and 
"Allowed Web Origins" contians "http://localhost:7335/"

*IMPORTANT: PLEASE SCROLL ALL THE WAY DOWN TO SAVE CHANGES

6) Under the "Connections" tab located in between of "Addons" and "Organizartions"
Disable "Username-Password-Authentication"  


6) Go under API and Create one (This shall be used for our API-Gateway JWT token)
- Under "Name" you can put anything
- Under "Identifier" for simplicity sake you may put "gateway" 
(REACT_APP_AUTH0_AUDIENCE under frontend and AUDIENCE_NAME under gateway)

7) You will see a Quick Start page and click C#. 
Copy the Authority and Audience string into the docker-compose.yml
(REACT_APP_AUTH0_AUDIENCE under frontend and AUDIENCE_NAME under gateway,
For Authority AUTHORITY_URL found under gateway)

8) To setup a webhook back to our machine go yo the left panel and click "Actions" --> "Flows"

9) Click "Login" and exit the tour

10) Click the "+" icon on the right hand side --> "Build Custom" to create an action
- "Name" can be anything
- Click "Create"

11) Click on the left, the 3rd icon of the Auth0 IDE that looks like a box. 
This is to add Dependency.

12) Click on "Add Dependency"
- "Name" put axios
- Click "Create"

13) Create a new line after line 6 and paste
const axios = require("axios");

14) Between line 8 ("exports.onExecutePostLogin = async (event, api) => {") 
and 9 ("};") and paste the following

await axios.post("https://{ngrok_provided_url}/auth/webhook", { params: { id: event.user.user_id, name: event.user.name, email: event.user.email}});

IMPORTANT PLEASE REPLACE {ngrok_provided_url} WITH THE LINK PROVIDED BY NGROK 
AFTER RUNNING "ngrok http 7321"

15) Click "Deploy"

16) Go back to the Login Flow page anc click the custom tab

17 Now you will see your newly created webhook. 
Drag it to the flow. It should be in between start and complete.

18) Click apply

Your Auth0 is now ready

At this point you may login using the frontend code 

Stripe Webhook and Credentials
------------------------------

1) Go to https://dashboard.stripe.com/register and sign up for an account

2) Click under "Developers" located at the top right area
- At this point ensure the "Test mode is enabled"

3) Click on "API Keys" located on the navigation panel on the left

4) Copy the "Publishable key" into the docker-compose.yml under frontend

5) Copy the "Secret key" into the docker-compose.yml under stripe "SECRET_KEY"

6) Go to "Webhooks" on the left and click on it 

7) Click on "Add an endpoint"

8) Under endpoint add your stripe endpoint "https://{local_tunnel}.loca.lt/Checkout"

IMPORTANT: BE SURE TO OPEN A NEW TERMINAL TO RUN THE LOCALTUNNEL COMMAND
PLEASE REPLACE {local_tunnel} WITH THE URL PROVIDED BY LOCALTUNNEL
"lt --port 7329"
BE SURE TO CLICK THE NEDPOINT IN YOUR TERMINAL SO THAT THE LINK IS ACTIVE

9) Click  "Select events" and search for "payment_intent.succeeded"

10) Click on the box next to "payment_intent.succeeded" to ensure upon this event your webhook is called

11) Click on "Add events"

12) Click on "Add endpoint"

13) Under the "Signing secret" click "Reveal"

14) Copy the string to your docker-compose.yml and replace "STRIPE_SECRET" under chekcout

Your Stripe is now ready
