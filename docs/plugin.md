# plugin command
plugin command is a statment which connects the voice app with the third parties' API, such as sendgrid email, twilio sms, Google Calendar. There will be more and more plugin commands availabe.

- Attributes
    - name: string type. the plugin name.
    - command:string object. The command to run of this plugin.

## built-in variables

### PLUGIN_STATUS
This is a string  object. The value  could be `OK` or `FAILED`. 
This string object record the status of the plugin command executing result. The program must check this string object everytime after invoking  a plugin command. If the value is `FAILED`. it means there is an error when executing the command. Please see the call log to  see  the error description.


## email plugin

### send
The `send` command is connecting your app with sendgrid email API. Your app can send email by this command.
You can setup an free sendgrid account in their website and using their infrastructure to send free email.

- Attributes
    - name: string object. The value is `email`
    - command:string object. The valuee is `send`
    - subject: the email subject.
    - text: the sms content. The content is a template string object. You can  embed any built-in objects and user defined objects inside this template string object. The system will auto populate values.
    - to: the string object. The email address to send. multi address can be seprated by `,`

You can setup your sendgrid API configuration in` console` `settings`. There are 2 parameters. 
Please read the sendgrid website document to get the `apiKey`
for example:

```
{
    "apiKey": "SG.lE1O7kSSRzCRmvps1sJq_g.KzeBtxgETSUEEvWWNAQHCbgHg9pOry8XvCnOho",
    "from":"no-reply@em.bustake.com"
}
```


```
email_template=`
You received a call from {FROM}.  The status of call is {CALL_STATUS}. The duration of call is {CALL_DURATION}
`;
plugin name='email', command='send', subject='received call', text=email_template, to='aabc@abc.com';
```


## sms plugin
### send
The `send` command is connecting your app with twilio sms API. Your app can send sms by this command.


- Attributes
    - name: string object. The value is `sms`
    - command:string object. The valuee is `send`
    - text: the sms content. The content is a template string object. You can  embed any built-in objects and user defined objects inside this template string object. The system will auto populate values.
    - to: the string object. The sms phone numbere to send. multi address can be seprated by `,`

You can setup your twilio API configuration in `console` `settings`. There are 3 parameters. 
Please read the twilio website document to get the `authKey`
for example:

```
{
    "authKey": "7f74b918dddd40e808a28813fdd1c03d6",
    "accountSid": "AC19d4ed94f9f2c9b9f29877a38fa8",
    "from": "+17207385"
}
```

```
sms_template=`
You received a call from {FROM}.  The status of call is {CALL_STATUS}. The duration of call is {CALL_DURATION}
`;
plugin name='sms', command='send', text=sms_template, to='12333444';
```


## booking plugin
booking plugin connects your voice app with Google Calendar. There are several commands availabe.

### enquire
enquire the availabe booking vacancies.

- return
    - The hidden `BOOKING_VACANCIES`array object will be created which include all the availabe vacancies.

- Attributes
    - name: string object. The value is `booking`
    - command:string object. The value is `enquire`
    - maxResults: return max number of booking vacancies
    - date: optinal string object. The date you would like to make a booking. The format is `YYYYMMDD`. If absent, the command will return the most recent available vacancies.

```
plugin name='booking', command='enquire',maxResults=8;
```

### reserve
Reserve a booking vacancy for caller.

- Attributes
    - name: string object. The value is `booking`
    - command:string object. The value is `reserve`
    - calendarId: The Google calendar id.
    - startTime: booking event start time.
    - endTime: booking event end time.
    - summary: event subject.
    - description: event description
    

```
 plugin name='booking', command='reserve', calendarId=calendarId, startTime=startTime, endTime=endTime, summary=summary, description=description;
```


### details
get the caller's current booking.

- return
    - if there is a booking for the caller. The following variables are created.

`BOOKING_EVENT_ID`,
`BOOKING_CALENDAR_ID`
`BOOKING_SUMMARY`
`BOOKING_DESCRIPTION`
`BOOKING_START_TIME`
`BOOKING_END_TIME`

```
 plugin name='booking', command='details';
```


### cancel
cancel caller's existing booking.

```
 plugin name='booking', command='cancel';
```

# The project `tele_vaccine` is a good example to show the plugins. The github [link](https://github.com/Benbus/tele_vaccine)