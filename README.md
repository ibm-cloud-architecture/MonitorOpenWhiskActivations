# OpenWhisk Failed Activation Notifyer

Simple OpenWhisk action that polls OpenWhisk API, looks for failed activations and triggers a notification for for failed activations
Activations in every organisation and space that the user has access to in the current region will be scanned

The system has two steps
    
    1. getFailedActivations
       - This action is triggered periodically and scans all activations since last run, looking for failed activations
       - If a failed activation is found it triggers the notification trigger
    2. Notification
       - One or more notification actions are invoked when the notification trigger is fired


## Supported notification channels

- Slack
- IBM Netcool/OMNIbus
- IBM Bluemix LogMet

# Setup

## Create the notification trigger
```
wsk trigger create notificationTrigger
```
This trigger will be fired by the main activation poller action when a failed activation is found.


## Create the actions for the desired notification channels

### To create notification in Slack
```
wsk action create posttoslack posttoslack.py -p slackwebhook <webhookURL> -p slack_username <username> -p slack_channel <ChannelName?
wsk rule create postfailuretoslack notificationTrigger posttoslack
```
For more information on how to set up an incomming webhook in slack here: [https://api.slack.com/incoming-webhooks](https://api.slack.com/incoming-webhooks)

### To create an entry in NetCool/OMNIbus
```
wsk action create posttonoi posttonoi.py -p omnibus_url <omnibus_url>
wsk rule create postfailuretonoi notificationTrigger posttonoi
```
