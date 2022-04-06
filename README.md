## Description

What is mollie? Read about it here: https://www.mollie.com

## How it works:

We create a Customer via an API call in Mollie, we can then create payments for this user.

Once the userflow has started and starts a payment he/she will be redirected to Mollie where he/she does the actual payment. Once completed Mollie will call the webhook url which will result in a REST call back to mendix where we can check the Mollie payment status and update the database. Once mendix returns a response back to Mollie it will redirect the user back to a mendix page (the MOLLIE_REDIRECTURL page set by the user) where we can apply logic based on the payment status.

## Dependencies

- Community Commons Function Library

## Installation

- Download the community commons module from the appstore if you havn't already
- Set the admin and user security in the project settings
- Get an API-key from Mollie (https://docs.mollie.com/guides/authentication) if you don’t have one yet.
Enable the desired payment methods in the website profile in the Mollie dashboard, should be under settings -> websiteprofiles (for testing with the TEST api key you just need to enable them, no need to complete the full signup).
- Create an admin page and add the Payments_Overview snippet to this page.
- Create a user “Thank You” page (this is the page the user will be redirected to after doing a payment in Mollie). Add a dataview with the payment entity as page parameter and make sure to set a page url for this page (for example: /mollie/payment/status/{Id}). You can also use the example page.
- Set all the Constants in the USE_ME folder

## Configuration

Use the example microflows or create your own logic to start a payment for a user. 

The MOLLIE_KEY constant expects the full API-KEY
The MOLLIE_REDIRECTURL expects the page url from the user “Thank You” page including the /p/ and without the object ID (So for example /p/mollie/payment/status/)

If you want to add custom logic after recieving a payment like sending a confirmation email you should add this to the webhook microflow (ACT_Mollie_Webhook)

The Mollie API documentation can be found here:  https://docs.mollie.com/reference/v2/payments-api/create-payment

## Important notes

Mollie requires a redirectURL that works, this means that if you run this integration locally it will not work because localhost is not a valid url. It needs to be ran in the cloud where Mollie can reach the URL.