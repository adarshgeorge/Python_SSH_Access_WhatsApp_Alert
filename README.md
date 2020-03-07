#Overview
Here we are going to setup a python script to get message in WhatsAppw when there is new SSH root access is established. 

#Pre Request 
-Pyhton 3.x should installed on your server.
-You need root access

#Create an account in Twilio:  https://www.twilio.com/
-Note the Account SDI and Token
-To setup the account visit the URL:https://www.twilio.com/docs/sms/whatsapp/quickstart/python

#Login to your server and execute the below commands to install the twilio library.

```
 pip3 install twilio
```

#Now visit the below and copy the code associated with Python

URL: https://www.twilio.com/docs/sms/whatsapp/api 

##Now we need to take ssh log from the server.  Execute the below command will get the latest log entry.

```
# sudo tail -100 /var/log/secure |grep Accepted| tail -1
```
##Code
```
import os
import time
from twilio.rest import Client
f = os.popen('sudo tail -100 /var/log/secure |grep Accepted| tail -1')
login = f.read()
host=login.split()[10]
user=login.split()[8]

account_sid = 'AC4118112108ae2ec9adc492034815e979'
auth_token = '6369c96caf54810d23eac7fa3cc64fd1'
client = Client(account_sid, auth_token)

message = client.messages.create(
                              from_='whatsapp:+14155238886',
                              body='Hi Jarvis! \nNew server access is established!! \nUser: {} \nUser IP address: {}'.format(user,host),
                              to='whatsapp:+919633131216'
                          )

#print(message.sid)
```

#Then we need to add this into user's .bashrc  (root user account)

```
python3 /home/ec2-user/whataApp.py
```

##Now access your server!!
