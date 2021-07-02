# Examples

## portal.bus

```bus
// version=1, timezone='Australia/Sydney', voice='Polly.Emma'

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
        reject;
    }
    default {
        play_audio ref='wrong_option';
        reject;
    }
}


```


## register.bus

```bus
// version=1, timezone='Australia/Sydney', voice='Polly.Ivy'

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
// version=1, timezone='Australia/Sydney', voice='Polly.Kimberly'

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