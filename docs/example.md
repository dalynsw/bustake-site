# Examples

When you are calling this number `+1720-709-23851`. The following script will guid the call flow.

## portal.bus

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
audio name='wrong_option', tts='[1s]wrong option entered, bye bye.';
ivr name='main_menu', audioRef='main_menu', audioNoInputRef='no_input', numDigits='1';

play_ivr ref='main_menu', maxLoop='2', timeout='10';

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


 - line 1, the first line must be a comment line with `version`, `timezone` and `default voice`. The timezone will be used to caculate your local time, the voice is used to text to speech when it is not specified.
 - line 3 - 10, this is an `assignment statement` which create a `string object`. The value of the string is the words of speaking in format of SSML markdown. the `[1s]` means `silicen` in 1 second, `[200ms]` means silence ins 200 millis-seconds. For details pelase reference in the [text to speech](text-to-speech.md) section.
 - line 12,  this is an `audio` statement which creates an `audio` object. This object can be referenced by it `name`. This audio object's `tts` attribute links to an `string` object which is the above `main_menu_tts` `text-to-speech` string object.
 - line 13, this line create another `audio` object. The `tts` attribute is an literal `string` object.
 - line 14, same as line 13 creates another `audio` oject.
 - line 15 creates an `ivr` object.
    -  this `name` attribute is the name used to be referenced. 
    - The `audioRef` attribute is the `name` of the `audio` object when the `ivr` object been played. 
    - The `audioNoInputRef` attribute is the `name` of the `audio` object when no any input founded when playing the `ivr` object.
    - The `numDigits` attribute specify how many `dtmf` key inputs the `ivr` object exepecting.

 - line 17, the is an `play_ivr` statement, which playing the `ivr` object.
    - The `ref` attribute specify the `name` of the `ivr` object being played.
    - The `maxLoop` attribute specify the maximum rounds of the playing.
    - The `timeout` attriutee specify the timeout in seconds of waiting the `dtmf` key inputs. If no inputs detected, One round is completed. If the inputs are dectected. The `play_ivr` statment return with the value of key inputs into an buit-in string object, the name is `GATHER_DIGITS`. 
- line 19 - line 34, this section is a `switch` statement,
    - line 19, check the built-in `GATHER_DIGITS` string object value. This value is from the above `play_ivr` statement's return.
    - line 20, this is a `case` statement which check the `switch` statement's value which  is the `GATHER_DIGITS` string object against it's own value which is a string literal object `'1'`, if both values matched, this means when the built-in `GATHER_DIGITS` value is `'1'`,  the case statement will run it's own block of statments.
        - line 21, this is the first line of statement of the `case` statement's which is a `redirect` statement. It redirects the current phone call into the new `bus script` to handle. The `path` attribute specify the new script path.
    - line 23 same as line 20, it will run when the value is `'2'`
    - line 26 is when the value is empty, which means `no input` is detected.
        - line 27 is a `play_audio` statement.  The `ref` attribute value is `no_input` which will looking for `audio` object with the name of `no_input`
        - line 28 is a `hang_up` statement. It will hangup the current call.
    - line 30 is the default case which is running when the input value is not matching with `'1'` `'2'` empty string, such as when the `GATHER_DIGITS` value is `'3'`.
        - line 31 is a `play_audio` statement will will play the `audio` object with then name of `'wrong_option'`
        - line 32 will hangup the current call.


## register.bus

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


audio name='no_input', tts='[1s]no input detected.';

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
ivr name='return_main_menu', audioRef='return_main_menu', audioNoInputRef='no_input', numDigits='1';
play_ivr ref='return_main_menu', maxLoop=2, timeout='10';                                   

if GATHER_DIGITS == '1' {
    redirect path='portal.bus';
} else {
    audio name='bye_bye', tts='[1s] bye bye.';
    play_audio ref='bye_bye';
    hang_up;
}

```

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
play_ivr ref='return_main_menu', maxLoop=2, timeout='10';

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