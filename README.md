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

### Second Part Enabled PUB/SUB API for our Project 
1. Enable PUB/SUB API Here: https://console.cloud.google.com/flows/enableapi

Select the created project. From the side menu select API Manager, press the ENABLE API status, search for Pub / Sub and enable them.

2. Craete a Topic 
   Here: https://console.cloud.google.com/cloudpubsub/topicList

   The topic will receive the nor tifications of new messages from the Gmail API . It is recommended to use only one topic for all Gmail APIs. ( see Pub / Sub limitations )

3. Create a Subscription
   https://cloud.google.com/pubsub/docs/subscriber

   It is possible to create two types of subscriptions. One passive (Pull) and one active (Push). Pull subscriptions provide the ability to receive messages upon client request, while push subscriptions automatically send notifications to the client whenever there is a new message.
   In this basica project we will use PUSH subscription Bassically a POST to our API.

    
   **DON'T FORGET add the url to your endpoint in push url option text box and disable Verification Checkbox** We are in a testing environment.

4. Give to PUB/SUB service  permission for be a publisher at    GMail API
   
   Pub / Sub needs permissions to send push messages. It is therefore necessary to guarantee the privilege publish to the service account: gmail-api-push@system.gserviceaccount.com
   1. So go to the Topic Dashboard, select your topic recently created and then in more actions select the option **View Permissions**
   2. Add Memember 
      In the text box add *New Memeber* add gmail-api-push@system.gserviceaccount.com with the role PUB/SUB publisher.

    We have completed the PUB/SUB basic configuration for our project.


### Third Part Generate Bearer Token 

With the fiven Code at /templates/custom1_index.html

We can generate the permissions to our gmail account, once you have logged and authorized the permissions to our application we can generate the Bearer Token with: 
```javascript
gapi.auth2.getAuthInstance().currentUser.get().getAuthResponse().access_token
```

### Make  the API call for put to Watch to our Topic
We need need put to our topic send the PUSH notification from GMAIL so we need to use GMAIL client API to INVOKE the method WATCH 

here: 
https://developers.google.com/gmail/api/v1/reference/users/watch


- request POST 
- url https://gmail.googleapis.com/gmail/v1/users/me/watch \
- Headers:
  - 'Content-Type': "application/json",
  - 'Authorization': "Bearer "TOKEN""
  - data 
    ```json
    {
	"topicName":"projects/test-gapi-1607289061594/topics/your_topic",
	"labelIds": ["INBOX"] 
    }
    ```
The response of this api call must to be something like: 
```json
{
  "historyId": "1166609",
  "expiration": "1607903297492"
}
```

Now each time that we recieve a new email our API recieve a a POST Request with a a default payload.


### Processing the incoming mails







### Create a new project 