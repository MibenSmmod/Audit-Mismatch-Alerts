# XIQ Audit Mismatch Alerts
## Purpose
The objective of this script is to search for all Online Devices, Real, with an Audit Mismatch and write to a CSV file in the current directory.  Next it will gather all Offline Devices and write to the same CSV for reference.  An email will be sent to a list of addresses you specify with an attached CSV file.  This skips all Sim, Plan, and non-managed devices including any that are in a "New" state which haven't checked in yet.

## SMTP Relay
This script uses an SMTP relay to email alerts.  A free 100 messages per day cloud service called Sendgrid was used for the example but does not function without you creating an account and updating your API key, To address, From address variables.
https://app.sendgrid.com/

## Actions & Requirements
You must update the fields with your SMTP relay server information within the XIQ-Audit-Mismatch-Alerts.py.  Install the required modules and generate an API Token to run script without user prompts.  If you need assistance setting up your computing environment, see this guide to aid in your setup: https://github.com/ExtremeNetworksSA/API_Getting_Started

### Install Modules
There are additional modules that need to be installed in order for this script to function.  They're listed in the requirements.txt file and can be installed with the command 'pip install -r requirements.txt' if using pip.

### API Token
There are multiple authentication methods built-in, but the default setup will use Tokens so the code simply executes without user prompts.   Other options:  You can use hard code credentials to generate a token (not as secure).  Prompt the user to enter credentials every time you run it which encrypts the password before sending to XIQ to generate a token.

In order to have this script run without user prompts, you must generate a token using our api.extremecloudiq.com.
Follow this article to generate an API key with the minimum requirements below:  https://extreme-networks.my.site.com/ExtrArticleDetail?an=000102173
These permissions allow you to access the account APIs, device APIs in read-only, and allows you to logout.
Brief instructions of the process:
  1) Navigate to api.extremecloudiq.com
  2) Use the Authentication: /login API (Press: Try it out) to authenticate using a local administrator account in XIQ
  
    {
    "username": "username@company.com",
    "password": "ChangeMe"
    }
  3) Press Execute button
  4) Scroll down and copy clip the contents of the access_token; do not copy the "" characters

    {
    "access_token": "---CopyAllTheseCharacters---",
    "token_type": "Bearer",
    "expires_in": 86400    <--- Expires in 24 hours>
    }
  5) Scroll to the top and press the Authorize button
  6) Paste contents in the Value field then press the Authorize button.  You can now execute any API's listed on the page.  ***WARNING*** You have the power to run all POST/GET/PUT/DELETE/UPDATE APIs and affect your live production VIQ environment.
  7) Scroll down to Authorization section > /auth/apitoken API (Press: Try it out)
  8) You need to convert a desired Token expiration date and time to EPOCH time:  Online time EPOCH converter:  https://www.epochconverter.com/
     EPOCH time 1717200000 corresponds to June 1, 2024, 00:00:00 UTC
  9) Update the expire_time as you see fit from #8 above.  Update the permissions as shown for minimal privileges to run the script.

    {
    "description": "Token for API Script",   <--- Update the description
    "expire_time": 1717200000,    <--- Expires based on your expiration date converted to EPOCH time
    "permissions": [
    "auth:r","device:r","logout"   <--- Copy these permissions
    ]
    }
  10) Press Execute button
  11) Scroll down and copy clip the contents of the access_token

    "access_token": "---ThisIsYourScriptToken---",
  ^^^ Use this Token in your script ^^^
    
    Locate in your Python script and paste your token:
    XIQ_token = "---ThisIsYourScriptToken---"
