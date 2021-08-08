
# SMS echo counter

```bus
// version=1, timezone='America/Los_Angeles', expiry='30D'

if  !counter {
    counter = 1;
} else {
    counter = counter + 1;
}

message = BODY + ',counter:' + counter;

sms text=message;
```
- line 1 must be a comment. The `version` parameter indicates that the bus script `version` is `1`, `timezone` is `Los Angeles`, and `expirey` represents the `lifetime` of the `variables` in the sms session is 30 days.
- lines  3 - 15 are `if statement`
- Line 3 checks whether the `counter` variable exists.
- line 4 set the value of the `counter` variable to `1` when the `counter` variable does not exist.
- Line 6 adds 1 to the value of the `counter` variable when the `counter` variable exists.
- Line 9 is an `assignment statement`, and the content is a string composed of the values of the variable `BODY` and the `counter` variable. The value of the `BODY` variable is the content of the short message received by the system.
- Line 11 is a `sms statement`, and the value of the string variable above is sent to the text message input party.

When the system receives the SMS from the same SMS sender within 30 days, the program will return the sent content and return the increasing `counter` value

```
-> Hello
<- Hello 1
->Hello
<- Hello 2
-> Hello
<- Hello 3
...
...
```