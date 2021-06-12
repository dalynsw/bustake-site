# Language Reference

## value types
In Bustake script, a value can be in 4 types, `string`, `number`, `boolean`, and `array`

### string
A string value is quoted by `'` or `"`
Valid string example: `'hello world'` or `"hello world"`

### number
A number value can be an integer or float. Exmples: `1, 2, 3` or `1.10, 2.20, -3.4`

### boolean
A boolean value is `true` or `false`.

### array
Array is a container type of value which contains other values. The value can be in different types. Examples: `[1,2,'hello',4]` or `[1.01, [1, 2], -3, 'hello',true, 4]`

### object
An object is a container of a value. You can think an object as a box which contains a value. A object can have value of `string`, `number`, `boolean` and `array`. Customer created objects can only be created by the `assignment` statment and the `fetch` statement.

```object
a = 'hello world'; //string object
b = 10; //number object
c = [1, 2, 'love', 4]; //array object
e = a; //string object the value is from object `a`
f = true; //boolean object
```

## expressions
Expression is a value. expression value is computed from other  values and objects connected by operators. Different operator is in different precedence.

the precedence of operators are:
    - `()`
    _ `[]`
    - `not !`
    - `* / %`
    - `+ -`
    - `|| && or and`
    - `> < >= <= == !=`

### literal expression
the literal expression is `string`, `number` and `array`. Examples: `1, 2, 0.9, 'abc', [1,23, 'abc']`

### logical expression
The logical expression is a boolean value which is caculated by `||` , `&&` or `and`. Examples: `ab && cd` `cc or abc`  `ee || df`

### comparison expression
The logical expression is a boolean value which is caculated by `>` `<` `>=` `<=` `==` `!=`.  Examples: `ab > 5` `cc <= abc`  `ee == df`

### parentheses
Parentheses is a group.  The group has high precedence. examples `(bc > 5 || isTrue)`


### string operators

String concat example: `abc + 'dfd' + adf`

### number operators
Number operators are `+ - * / %`

## body

A body is a collection of statements. A body starts with an indent and ends with a outdent.

```body
if good
    //a indent starts a body
    a = b;
    c = d;
    //a outdent ends a  body

switch condition
    //a indent starts a body
    e = ds;
    email subject='sdfsdf', content='sfsdf';
    //a outdent ends a  body

a = b;

```

## statements

The statement is an execute unit of a script.  A script is a body. A body includes a collection of statements or bodys.

### if

The `if` statement runs a body when a expression's value is true.

```if
if a > 5
    //a indent starts a body
    a = b;
    c = d;
    //a outdent ends a  body
```

### if else

The `if else` statement runs 2 bodys. When the exprssion's value is true, it runs the `if` body, otherwise the `else` body.

```if else
if a > 5
    //if body
    a = b;
    c = d;
else
    //else body
    ddd = 'asdfasf';
    dff = 223
```

### if elsif else

The `if else` statement runs multi bodys. When the exprssion's value is true, it runs the `if` body, otherwise it will check the other `elsif` bodys, otherwise the `else` body.

```if elsif else
if a > 5
    //if body
    a = b;
    c = d;
elsif a > 2
    //elsif  body
    ds = 'ddd';
elsif a > 1
    //elsif  body
    fsdf = 123;
else
    //else  body
    ddd = 'asdfasf';
    dff = 223;
```

### switch case

The `switch case` statement runs multi bodys based on the expression matching. When the exprssion of a `case statement` value is true, it runs the `body of the case`, otherwise it will check the other `cases`, otherwise the `default` body.

```switch case
switch age
    case 100
        a = b;
        c = d;
    case 90
        ds = 'ddd';
    case 80
        fsdf = 123;
    default
        ddd = 'asdfasf';
        dff = 223;
```

### assignment

An `assignment` statement puts a value into an object. It ends with `;`

```object
a = 'hello world';
b = 10;
c = [1, 2, 'love', 4];
e = a;
```

### audio
The `audio` statement creates an audio object. The object can be accessed by the name.

```audio
audio name='ivr_department_menu', voice='alice', file='departments_menu_file_name', tts='press 1 for sales department, press 2 for support department, press 3 for booking an appointment.';

```
- attributes
    - name: the name of the object. The object can be accessed by the name
    - file: the audio file name of saved under the `audio repository`
   



### ivr

create an ivr object. The object can be accessed by the name attribute.

```ivr

ivr name='department', audioRef='ivr_department_menu', numDigits=1;

```

- attributes
    - name: the name of the created object.
    - audioRef: the audio object used be play before gathering the dtmf inputs.
    - audioNoInputRef: the audio object used be play when no inputs detected.
    - numDigits: the number of dtmf input expected.


### play_audio

Play the audio object.

```play_audio

play_audio name='recording_alert', loop=2, voice='alice';

```

- attributes
    - name: the name of audio object to be played.
    - voice: the name of voice. please ref from [voice_name](voice_name)
    - loop: loop count. default is 1.


### play_ivr

Play the IVR message and wait to gather the dtmf inputs.

```play_ivr

play_ivr name='department';

```

- attributes
    - name: the name of ivr object to be played.

### dial

Dial an outbound leg and bridge the inbound leg when success.

```dial
dial values=['6123443xx'];
```

- attributes [twiML dial] (https://www.twilio.com/docs/voice/twiml/dial)
    - trunkType: pstn/sip default is pstn
    - answerOnBridge: 
    - callerId:
    - callReason:
    - hangupOnStar:
    - record
    - recordingTrack
    - ringTone
    - timeLimit
    - timeout
    - trim


### hangup

The`Hangup` statement ends a call. 

```hangup
hangup
```


### reject

The `reject` statement rejects an incoming call to your Twilio number without billing you. This is very useful for blocking unwanted calls.

```reject
reject
```

### fetch

The `fetch` statement will download a formated data by a http 'get' request.

```fetch
fetch url='https://www.test.ccc/abc', timeout=2;
```

The response of the fetch result must be in this format:

```fetch result
key1=value1
key2=value2
key3=value3
...
```

The fetched key will be saved in a string object with keys and values. These keys will be objects with the values. The app can accesses these string objects afterwards.

- attributes
    - url
    - timeout

### web_hook


The `web_hook` statement will send a http 'get' request and ignore the result.

```web_hook
web_hook url='https://www.test.ccc/abc', timeout=2;
```

The result of `web_hook` is ignored.

### sms

### email

The `email` statement  will send an email

```email
email subject='abc', content='cddd';
```

- attributes
    - subject
    - content

## built-in objects

### CALL_SID

The Twilio-provided Call SID that uniquely identifies the Call

### FROM

The phone number, SIP address, Client identifier or SIM SID that made this call. Phone numbers are in E.164 format (e.g., +16175551212). SIP addresses are formatted as name@company.com. Client identifiers are formatted client:name. SIM SIDs are formatted as sim:sid.


### TO
The phone number, SIP address, or client identifier to call.

### ACCOUNT_SID

Your account ID.

### CALL_STATUS

The status of this call. Can be: queued, ringing, in-progress, canceled, completed, failed, busy or no-answer. See Call Status Values below for more information.

### DIRECTION
A string describing the direction of the call:

    - inbound for inbound calls

    - outbound-api for calls initiated via the REST API

    - outbound-dial for calls initiated by a <Dial> verb.

### FORWARDED_FORM

This parameter is set only when Twilio receives a forwarded call, but its value depends on the caller's carrier including information when forwarding.

Not all carriers support passing this information.

### CALLER_NAME

This parameter is set when the IncomingPhoneNumber that received the call has had its VoiceCallerIdLookup value set to true ($0.01 per lookup).

### PARENT_CALL_SID

A unique identifier for the call that created this leg.

This parameter is not passed if this is the first leg of a call.

### FORWARDED_FORM

This parameter is set only when Twilio receives a forwarded call, but its value depends on the caller's carrier including information when forwarding.

### CALLER_NAME

This parameter is set when the IncomingPhoneNumber that received the call has had its VoiceCallerIdLookup value set to true.

### PARENT_CALL_SID

A unique identifier for the call that created this leg.

This parameter is not passed if this is the first leg of a call.


### FROM_CITY

The city of the caller

### FROM_STATE

The state or province of the caller

### FROM_ZIP

The postal code of the caller

### FROM_COUNTRY

The country of the caller

### TO_CITY

The city of the called party

### TO_STATE

The state or province of the called party

### TO_ZIP

The postal code of the called party

### TO_COUNTRY

The country of the called party

### DIAL_CALL_STATUS

The outcome of the `dial` attempt. See the DialCallStatus section below for details.

### DIAL_CALL_SID

The call sid of the new call leg. This parameter is not sent after dialing a conference.

### DIAL_CALL_DURATION

The duration in seconds of the dialed call. This parameter is not sent after dialing a conference, or if a Child call is redirected to a new TwiML URL before being disconnected.

### CALL_DURATION

The duration in seconds of the just-completed call.

### RECORDING_URL

The URL of the phone call's recorded audio. This parameter is included only if Record=true is set on the REST API request, and does not include recordings from `dial` or `record`

### RECORDING_SID

The unique id of the Recording from this call.

### RECORDING_DURATION

The duration of the recorded audio (in seconds).

### GATHER_DIGITS

The dtmf digits entered.

### GATHER_SPEECH_RESULT

contains the transcribed result of your caller's speech.

### GATHER_CONFIDENCE

contains a confidence score between 0.0 and 1.0. A higher confidence score means a better likelihood that the transcription is accurate.


## built-in functions

The build-in functions is tool to manipulate `string`, `number` value

### starts_with

return `true` or `false` a `string` starts with a prefix.

```starts_with
    isAustralia = starts_with value=FROM, prefix='+61';
```

### ends_with

return `true` or `false` a `string` ends with a suffix.

```starts_with
    isEndABC = ends_with value=TO, suffix='abc';
```

### template

return a string from a format string and values

```template
email_content = template format="I love {0}, I am {1}",  values=['Twilio', 'Jay'];
\\email_content is string object with value 'I love Twilio, I am Jay'
```

### split

return a an array of string separated by a separator

```split
values = split value="a&b&c&d", separator="&";
\\ the values is array object with ['a','b','c','d']
```

### match

return `true` or `false` when a `string` value match the regex 

```match
is_number = match value="str", pattern='[0..1]+';
```

### number

convert a `stirng` value to a `number` value

```number
    num = number value='123';
    \\num value is 123 in number object.
```

### string

convert a `number` value to a `string` value

```number
    str = string value=123;

    \\str value is '123' in string object.
```

### random

return a random number between the `min` and `max`

num = random min=0, max=100;

### substring

```substring
    sstr = substring value='abcde', strart=1, end=4;

    \\sstr value is 'bcd'
```