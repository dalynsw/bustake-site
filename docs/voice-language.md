# Voice Language Reference

### built-in Voice extended type value

### audio
The `audio` type represents a piece of speech. It can be a recorded audio file or a piece of text.

### ivr
The `ivr` type represents a period of voice interaction, which can be the result of the inputs of the dtmf keyboard or the recognition result of voice recognition.

## Voice extended statements


### audio
The `audio` statement is used to create an `audio` object. The audio object can be referenced through the `name attribute`. `;` is the end character of the statement.

- Attributes:
    - name: string value, used to refer to the created `audio object`.
    - url: string value, The url of the audio file
    - tts: string value, Text-to-speech content

```audio
audio name='ivr_department_menu', tts='press 1 for sales department, press 2 for support department, press 3 for booking an appointment.';
```

### ivr
`ivr` is used to interact with the caller by phone call. It can collect the caller's keyboard inputs or voice recognition string value. `;` is the end character of the statement.

- Attributes:
    - name: string value. Used to refer to the created `ivr object`.
    - audioRef: string value. The name of the `audio object`. This audio object will automatically play
    - audioNoInputRef: string value.   The name of the `audio object`. This audio object is played when no input is detected
    - audioNoMatchRef: string value.   The name of the `audio object`. This audio object is played when the inputs no matching is detected
    - numDigits: number value. Expected number of digits entered by the caller.
    - values: string array. Optional. This is the string values of the `dtmf` expecting or the `string values` of the transcribed result of your caller's speech expecting.


```ivr

ivr name='department', audioRef='ivr_department_menu', audioNoInputRef='no_match',  audioNoMatchRef='no_match', numDigits=1;

```

### play_audio
The `play_audio statement` is used to play `audio objects`.  `;` is the end character of the statement.

- Attributes:
    - ref: string value. Points to the name of the `audio object`
    - voice: string value. optinal. voice name.
    - loop: number value. Play cycles.


```play_audio
play_audio ref='recording_alert', loop=2, voice='alice';
```


### play_ivr
The `play_ivr statement` is used to play ivr and collect keyboard inputs or speech recognition result.

- Attributes:
     - ref: string value. Point to the name of the `ivr object`.
     - noInputCount: number value.  This the loop times of the `ivr` When no input occurs.
     - noMatchCount: number value.  This the loop times of the `ivr` When the input is not matching the options. 
     - finishOnKey: The dtmf key to terminate the inputs. The popular dtmf key is `#`
     - timeout: string value. The number of seconds to wait the inputs

```play_ivr
play_ivr ref='department', noInputCount=2, noMatchCount=2, timeout='5';
```

### dial
The `dial` statement is used to create an outbound call and bridge the incoming call. `;` is the end character of the statement.

- Attributes:
    - value: string value. terimination number.
    - values: Array of string objects,  containing multiple termination numbers. If the previous number is not connected, the next number will be dialed

```dial
dial value='+123443xx';
```


### redirect
The `redirect` statement redirects the phone call to the specified `Bus Script`. Any statement after the `redirect` statement will not be executed. `;` is the end character of the statement.

- Attributes:
    - path: Specified the path of the script


```redirect
redirect path='register.bus'
```

### hang_up
`hang_up` statement hangs up the current inbound call. `;` is the end character of the statement.

```hang_up
hang_up;
```



## Voice extension built-in objects

### CALL_SID
- Type: string
`CALL_SID` The ID of the current phone


### FROM
- Type: string
Caller's phone number


### TO
- Type: string
Phone number dialed


### ACCOUNT_SID
- Type: string
TWILIO account ID



### CALL_STATUS
- Type: string
Phone status. Values ​​can be: `queued, ringing, in-progress, canceled, completed, failed, busy or no-answer`.


### DIRECTION
- Type: string
The phone direction values ​​are as follows:
    -inbound for inbound calls
    -outbound-api for calls initiated via the REST API
    -outbound-dial for calls initiated by a <Dial> verb.

### GATHER_DIGITS
- Type: string
Keyboard input string collected by `play_ivr` statement

### GATHER_SPEECH_RESULT
- Type: string
Voice input value collected by `play_ivr`


### GATHER_CONFIDENCE
- Type: string
`play_ivr` voice input confidence


### FORWARDED_FORM
- Type: string


### CALLER_NAME
- Type: string


### PARENT_CALL_SID
- Type: string



### FORWARDED_FORM
- Type: string


### CALLER_NAME
- Type: string


### PARENT_CALL_SID
- Type: string


### FROM_CITY
- Type: string


### FROM_STATE
- Type: string


### FROM_ZIP
- Type: string

### FROM_COUNTRY
- Type: string


### TO_CITY
- Type: string


### TO_STATE
- Type: string


### TO_ZIP
- Type: string

### TO_COUNTRY
- Type: string

### DIAL_CALL_STATUS
- Type: string

### DIAL_CALL_SID
- Type: string


### DIAL_CALL_DURATION
- Type: string


### CALL_DURATION
- Type: string


### RECORDING_URL
Type: string


### RECORDING_SID
-Type: string


### RECORDING_DURATION
- Type: string
