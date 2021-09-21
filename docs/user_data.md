# User Data
User data we defined here is the data that is from the user and can be used to identify the user.

# What User Data we access
The users data we collect include the following:

## Phone Call Data
The Phone Call data, including Caller ID and geographic location information of the phone call. These data come from third-party phone platforms such as Twilio.

## Google Calendar Events
The data of Google Calendar Events which include event summary, descripition, start time, end time, etc, from Google customer's calendars.


# Why we need to access these Data

## Phone Call Data
Bustake is a phone call control runtime environment. When controlling a phone call, it will naturally get the information of the controlled phone call. A third party could also pass in auxiliary information about the geographic location of the phone.

Bustake itself is only an runtime environment, it will not save these phone call information. But the log of system could contain the information of phone call. These logs will be purged regularly.
In short, the bustake runtime does not purposly collect user phone call information.

## Google Calendar Events
The Bustake runtime environment includes a Google Calendar Plugin. This Plugin connects the phone call with the Google Calendar. Programmers can use this plug-in to create voice applications, such as booking a Covid vaccination appointment. In order to successfully connect the phone call and Google Calendar, the Calendar owner's consent must first be obtained. Then the plug-in can query and create Google Calendar events to establish appointments. The user's appointment data is managed and saved through Google Calendar itself. The plug-in itself only acts as a bridge and provides a voice interface that allows users to select dates and create appointments. .
In short, the plug-in itself only provides a bridge function and a voice interface. It does not manage the user's calendar data.

[![How the Booking Plugin works](http://img.youtube.com/vi/lVTGaIgC6ew/0.jpg)](http://www.youtube.com/watch?v=lVTGaIgC6ew)
