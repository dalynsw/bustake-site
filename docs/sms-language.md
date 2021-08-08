# SMS Language Reference
SMS scripts are used to process SMS messages sent by different users. The script can mark the lifetime of variables. SMS messages from the same sender share the values of these variables during the lifetime.
For example, when the system receives a short message from `user 1` for the first time, the counter value is set to `1`. After each short message is received, the counter value is increased by 1. Then the value of this counter will be increasing until the labeled lifetime expires.

## SMS extended statements

### sms
`sms` send text messages to the specified phone number. `;` End of character

- Attributes:
    - text:  sting value. text content of sms
    - to: string value. Receiver's phone number.


### redirect
The `redirect` statement redirects the phone to the specified Bus Script. Any statement after the redirect statement will not be executed. `;` End of character

- Attributes:
    - path: Specify the path of the script


```redirect
redirect path='register.bus'
```

## SMS extension built-in objects

### SMS_SID
SMS session ID


### FROM
Incoming SMS Number

### TO
SMS number sent

### ACCOUNT_SID
TWILIO account ID

### MESSAGE_STATUS
SMS status. Value can be: `queued, sending, sent, or failed`. 

### FROM_CITY

### FROM_STATE

### FROM_ZIP


### FROM_COUNTRY

### TO_CITY

### TO_STATE


### TO_ZIP


### TO_COUNTRY

