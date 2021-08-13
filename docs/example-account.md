# Account Managment
This is an example of user registration and password modification by making a call.


## portal.bus
This is an `IVR` program that registers a new user by letting the user enter `1` on the keyboard, and enter `2` to change the password.

```bus
// version=1, timezone='America/Los_Angeles', voice='Polly.Emma'

main_menu_tts =`
[1s]
Welcome to bus take.
Please listen carefully the following menu.

Press 1 for (registration.)[emphasis level="strong"] [200ms]
Press 2 for (changing password.)[emphasis level="strong"] [200ms]
`;

audio name='main_menu', tts=main_menu_tts;
audio name='no_input', tts='[1s]no input detected.';
audio name='no_match', tts='[1s]wrong option entered.';
audio name='wrong_option', tts='[1s]wrong option entered, bye bye.';
ivr name='main_menu', audioRef='main_menu', audioNoInputRef='no_input', audioNoMatchRef='no_match',numDigits=1;
play_ivr ref='main_menu', noInputCount=2, noMatchCount=2, timeout='10';

switch GATHER_DIGITS {
    case '1' {
        redirect path='register.bus';
    }
    case '2' {
        redirect path='change_password.bus';
    }
    case '' {
        play_audio ref='no_input';
        hang_up;
    }
    default {
        play_audio ref='wrong_option';
        hang_up;
    }
}


```


 - Line 1 must be a comment, the comment contains version information is `1`, the time zone is `Los Angeles`, and the default voice is `Polly.Emma`.
 - Line 3-10, this is an assignment statement, which creates a string object. The value of this string is a text-to-speech.
 - line 4 is silent for 1 second
 - line 8 registration in emphasis
 - line 9 changing password in emphasis
 - Line 12, is an `audio` statement, which creates an audio object. The name is `main_menu`, and the content of tts comes from the string object value above.
 - Line 13, is an `audio` statement, which creates an audio object. The name is `no_input`, and the content of tts is a string object value.
 - Line 14, is an `audio` statement, which creates an audio object. The name is `no_match`, and the content of tts is a string object value.
 - Line 15, is an `audio` statement, which creates an audio object. The name is `wrong_option`, and the content of tts is a string object value.
 - Line 16, is an `ivr` statement, which creates an `ivr` object with the name `main_menu`. The `audioRef` of the `ivr` will be played first. The `audioNoInputRef` of the `ivr` will be played when no inputs. The `audioNoMatchRef` of the `ivr` will be played when inputs not matching.  The`numDigits` is the number of `dtmf` entered expecting.
 - line 17, is a `play_ivr` statement that plays the `ivr` object whose name is `main_menu`.  The `noInputCount` is the number of loop of the `ivr` when no inputs occurs. The `noMatchCount` is the number of loop of the `ivr` when no matching occurs. The `timeout` is the Waitting time which is 10 seconds for waiting the inputs.
 - line 19-34 is a `switch case` statement
 - The line 19 `switch statement` will detect the value of a built-in string object `GATHER_DIGITS`, which comes from the result of the above `play_ivr` statement.
 - Line 20-22, check whether the value of `GATHER DIGITS` is equal to the string `'1'`, if so, run this `case` statement. Line 21 is `redirect` to the `regsiter.bus` script
 - Line 23-25, check whether the value of `GATHER DIGITS` is equal to the string 2, if it is, run this `case` statement. Line 24 is `redirect` to the `change_password.bus` script
 - Line 26-29, Check whether the value of `GATHER DIGITS` is equal to an empty string. That is, there is no input, if it is, run this `case` statement. Line 27 plays `no_input` audio. Line 28 `hangs up`
 - Line 30-33 are `default case statements`. When the input is neither `'1'` nor `'2'`, the `default statement` block will be executed. Line 31 plays the `wrong_option` audio. Line 32 `hangs up` the call.


## register.bus
Register an account. First play the audio and let the user wait. Then send an http request to the background to check whether the user is already registered, if not, create an account. The background returns the newly created user name, password, and ID. Then send a text message containing the user name and password , ID to the user. If it has been created, it will play a voice to tell the user that it has been registered. After that, play the IVR to allow the user to jump to the main menu or hang up.
```bus
// version=1, timezone='America/Los_Angeles', voice='Polly.Ivy'

register_checking_tts=`
[1s]
Please wait, we are checking your status now...
`;

audio name='register_checking', tts=register_checking_tts;
play_audio ref='register_checking';

userURL = template format='http://localhost:8020/account/user/%s', values=[FROM];
fetch url=userURL;


if userid {

    audio name='already_registerd_tts', tts='[1s] You are registered already. [200ms]';
    play_audio ref='already_registerd_tts';

} else {

    regURL = template format='http://localhost:8020/account/register/%s', values=[FROM];
    fetch url=regURL;
    sms_format = `
Congratulations. You have registered successfully.
Please be ware this is your userid %s this is your user name %s  this is your password %s
`;
    sms_content = template format=sms_format, values=[userid, phone, password];
    sms to=phone, text=sms_content;

    audio name='sms_reg_tts', tts='[1s]Congratulations. We have sent a SMS of the login information.';
    play_audio ref='sms_reg_tts';

}

return_main_menu =`
[1s] 
Press 1 to return to the main menu [200ms] or just hang up.
`;

audio name='return_main_menu', tts=return_main_menu;
audio name='no_input', tts='[1s]no input detected.';
ivr name='return_main_menu', audioRef='return_main_menu', audioNoInputRef='no_input', numDigits='1';
play_ivr ref='return_main_menu', noInputCount=2, timeout='10';                                   

if GATHER_DIGITS == '1' {
    redirect path='portal.bus';
} else {
    audio name='bye_bye', tts='[1s] bye bye.';
    play_audio ref='bye_bye';
    hang_up;
}

```
- Line 1 must be a comment, the comment contains version information is 1, the time zone is Los Angeles, and the default sound is `Polly.Ivy`.
- Lines 3-6 is an `assignment statement`, which is an assignment statement that creates a string object. The value of this string is a text code synthesized from speech input.
- Line 4 Mute for 1 second when playing tts
- Line 8 is an `audio` statement that creates an audio object. The name is `main_menu`, and the content of tts comes from the string object value above.
- line 9 plays the above audio object.
- line 11 is an `assignment statement`. The `url` string object is created through the `template` function.
- line 12 is a `fetch statement`. Call the url pointed to by the string object created above.
- lines 15-34 is an `if else` statement
- Line 15-19 is the `if statement`. Check whether the `userid` object is true. If it is, execute the `statement block`. This `userid` object is created by the return value of the `fetch statement` above.
The http return of `fetch` should be `userid=xxxx`. In this way, the value of the `userid` string object is an empty string. It means that the account already exists.

- Line 17 is an `audio statement`, creating an `audio object`, and the content of tts is that the user has registered.
- Line 18 is the `play_audio statement`, which plays the `audio` object above. 
- Line 20-34 is the `else statement block`, which runs when `userid` is an empty string, which means that the user does not exist.
- line 22 is an `assignment statement`. The url string object is created through the `template function`. This url is the backend url registered by the user
- Line 23 is a `fetch statement`. Call the url pointed to by the string object created above. That is, create a new user. The return value of fetch will contain multiple lines, including `user ID, password, and username`. This will create multiple String objects.
userid=xxxx
password=xxxx
username=xxxx
- line 24 -27 is an `assignment statement`. The content is a `string object` of the `SMS template`.
- line 28 is an `assignment statement`. The `string object` of the SMS content is created through the `template function`.
- line 28 is an `sms statement`, send the content of the message to the caller
- line 31 is an `audio object`, and the content is the registered tts
- line 32 play the above `audio object`
- Line 41-52 are `IVR` guide users to return to the main menu or `hang up` the phone


## change_password.bus
```bus
// version=1, timezone='America/Los_Angeles', voice='Polly.Kimberly'

status_checking_tts=`
[1s]
Please wait, we are checking your status now...
`;

audio name='status_checking', tts=status_checking_tts;
play_audio ref='status_checking';

userURL = template format='http://localhost:8020/account/password/change/%s', values=[FROM];
fetch url=userURL;


audio name='no_input', tts='[1s]no input detected.';

if password {

    sms_format = `
Congratulations. Your password is changed successfully.
Please be ware this is your userid %s this is your user name %s this is your password %s
`;
    sms_content = template format=sms_format, values=[userid, phone, password];
    sms to=phone, text=sms_content;

    audio name='sms_reg', tts='[1s]Congratulations. We have sent a SMS of the login information.';
    play_audio ref='sms_reg';

} else {

    audio name='not_registerd', tts='[1s] Your account is not founded, please register firstly. [200ms]';
    play_audio ref='not_registerd';

}

return_main_menu =`
[1s] 
Press 1 to return to the main menu [200ms] or just hang up.
`;

audio name='return_main_menu', tts=return_main_menu;
ivr name='return_main_menu', audioRef='return_main_menu', audioNoInputRef='no_input', numDigits='1';
play_ivr ref='return_main_menu', noInputCount=2, timeout='10';

if GATHER_DIGITS == '1' {
    redirect path='portal.bus';
} else {
    audio name='bye_bye', tts='[1s] bye bye.';
    play_audio ref='bye_bye';
    hang_up;
}

```


## VoicePortal.js
```js
const {asyncMiddleware} = require('middleware-async');

const service = require('./service');
const { strformat } = require('./utils');
module.exports = function(app){


    app.get('/account/register/:user', asyncMiddleware(async(req, res) => {
        let user = req.params.user;
            
        let accountDetails = await service.getAccountDetails(user);
        let account = null;
        let password = '';
        if (!accountDetails) {
            account = await service.insertAccount(user);
            accountDetails = await service.getAccountDetails(user);
            let passwordMeta = account.metas.find(m => m.section === 'security' && m.key ==='password');
            password = passwordMeta.value;
        }
        
                
        let ret = strformat('password=%s\nphone=%s\nbalance=%f\npaid=%f\ncharge=%f\nuserid=%s\n', [password, accountDetails.phone, accountDetails.balance, accountDetails.paid, accountDetails.charge, accountDetails.id]);
        res.status(200).send(ret);
    }));

    app.get('/account/password/change/:user', asyncMiddleware(async(req, res) => {
        let user = req.params.user;
            
        let uobj = await service.changePassword(user);
        if (!uobj) {
            res.status(200).send('password=\n');
            return;
        }
    
        let ret = strformat('password=%s\nuserid=%s\nphone=%s', [uobj.password, uobj.userid, uobj.phone]);
        res.status(200).send(ret);
    }));

    app.get('/account/user/:user', asyncMiddleware( async(req, res) => {
        
        let user = req.params.user;
        let account = await service.getAccountByPhone(user);
        let id = '';
        if (account) {
            id = account.userid;
        }
        let ret = strformat('userid=%s\n', [id]);
        res.status(200).send(ret);
    }));

    app.post('/account/auth', asyncMiddleware( async(req, res) => {
        let form = req.body;
        let username = form.username;
        let ret = await service.getAccountByPhone(username);
        if (!ret) {
            ret = {"username":""};
        }
        res.json(ret);
    }));
    
}
```