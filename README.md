# mail_phone_validator
Read your mails from Gmail recivieng a request from gcloud using  PUB/SUB for request to your Webhook and check if  a number and phone is validate at each mail that you recieve.

## Steps for achieve this
### First Part
1. Create a new project in your google cloud console 
   here: https://console.developers.google.com/
2. Enabled the GMail API for your project
   Here : https://developers.google.com/gmail/api/auth/about-auth
3. Create a new OAuth 2.0 Credential
   Here: https://console.cloud.google.com/apis/credentials
4. Edit your Auth consent screen

All the steps mentionend above can be done trough using the button **Enabled the Gmail API** and then **Create API Key**
here: https://developers.google.com/gmail/api/quickstart/js#troubleshooting
After of that wizard has created A Project with the gived name and the Auth 2.0 Credential. 
And then the allowed scopes are added programatically with the code provided en the example:https://github.com/googleworkspace/browser-samples/blob/master/gmail/quickstart/index.html


Just consider two things: 
- First replace The *Client ID* and the the *API KEY* in the prived Code with yours.
- Second. In your Auth 2.0 Credential configuration modify the Authorized Javascript origins the URIs to your own.

Take a time for read and understand how this code https://github.com/googleworkspace/browser-samples/blob/master/gmail/quickstart/index.html Works.





### Create a new project 