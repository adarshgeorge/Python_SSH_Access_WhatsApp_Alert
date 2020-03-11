## Overview
In this article, we'll take a look at how Python can be used by System Administrators to get message in WhatsApp when there is new SSH root session is established. 

### Pre Request 
-Pyhton 3.x should installed on your server.

-You need root access



### Create an account in Twilio:  https://www.twilio.com/
Note the Account SDI and Token 

To setup the account visit the URL:https://www.twilio.com/docs/sms/whatsapp/quickstart/python



### Login to your server and execute the below commands to install the twilio library.

```
 pip3 install twilio
```


### Now visit the below URL and copy the code associated with Python

URL: https://www.twilio.com/docs/sms/whatsapp/api 



### Next we need to parse ssh log from the server.  Execute the below command will get the latest log entry.

```
# sudo tail -100 /var/log/secure |grep Accepted| tail -1
```


### Code
```
import os

from twilio.rest import Client
f = os.popen('sudo tail -100 /var/log/secure |grep Accepted| tail -1')
login = f.read()
host=login.split()[10]
user=login.split()[8]

account_sid = 'XXXXAdd_account_SDI_XXXX'
auth_token = 'XXX_add_your_tokenid_XXX'
client = Client(account_sid, auth_token)

message = client.messages.create(
                              from_='whatsapp:+1_Their_whatappNum',
                              body='Hi Jarvis! \nNew server access is established!! \nUser: {} \nUser IP address: {}'.format(user,host),
                              to='whatsapp:+91_yournumber_'
                          )

#print(message.sid)
```

### Then we need to add this into user's .bashrc  (root user account)

```
python3 /home/ec2-user/whataApp.py
```

### Now access your server!!
