# OpenWhisk Failed Activation Event Notification

## Introduction

This solution has been developed as a workaround for monitoring the health of OpenWhisk. This solution is in support of the IBM BlueCompute application. Today an IBM solution is not available for the Bluemix Service.

This solution is a simple OpenWhisk action that polls the OpenWhisk API, looks for failed activations and triggers a notification for failed activations. Should a failure be detected, an event is sent to the on premises IBM Netcool Operations Insight for processing and first responder notification. This solution can also send the event to Slack and LogMet.

Activations in every Bluemix organisation and space that the user has access to in the current region will be scanned 

## Deploying the solution

The system has two steps
    
    1. getFailedActivations
       - This action is triggered periodically and scans all activations since last run
       - If a failed activation is found a failure message is generated  
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
