# Voice

## Voice App
Voice App is a set of `Bus Script` script files and configuration files used to control the phone call to complete a series of voice interactions.

### Voice App Files Structure
The file system of a voice App is as follows::
```
App
└── /
    ├── etc
    ├── logs
    ├── static
    ├── templates
    └── scripts
```

    - `etc` is used to save configuration files
    - `logs` are used to store phone call logs. Each phone call will generate a log file. The files are managed by the year, month, and day folders.
    - `static` is used to store static files. These files can be accessed by URL.
    - `templates` are used to save `handlebars` template files, which can be used to assist phone call control.
    - `scripts` is the folder used to save `bus scripts`. Each script can be accessed by URL. One `inbound url`, one `outbound url`

### inbound url
Each `bus script` has an `inbound url` that can be used to handle `inbound calls`. The `URL format` is as follows:
`https://twlo.bustake.com/script/voice/${appID}/${path}`

    - ${appID} is the ID of an app
    - ${path} is the `path` of the script file in the `scripts` folder. For example, `scripts/a/b/c/abc.bus` ${path} is `a/b/c/abc.bus`

The `inbound url` can be pasted in the `numbers` purchased by twilio as follows:
![tiwlio number](https://raw.githubusercontent.com/jaynsw/bustake-site/main/docs/images/twlo-number.png)

### outbound url
Each `bus script` has an `outbound url`. Run this URL in the browser or send a `GET http request` to this URL can create a phone call, which will be controlled by this script. The `outbound URL` format is as follows:
`https://twlo.bustake.com/script/outbound/${appID}/${path}`

This URL must contain two parameters:
    - token: This token is a string value, this string value comes from the configuration file `etc/ouboundcalltokens.txt` Each line of this file is a `token`. If you delete the token, the URL corresponding to the token will not work.
    - to: phone number, the created phone call will come to this number.

```
https://twlo.bustake.com/script/outbound/sdfasdfddsd233dd/ab/abc/abc.bus?token=dwe2333dsssdfds&to=1023334
```

outboundcalltokens.txt
![outboundcalltokens](https://raw.githubusercontent.com/jaynsw/bustake-site/main/docs/images/outboundcalltokens.png)

If you want to successfully create a `outbound call`, you must save the file `etc/twilio.json`. The values of `authKey`, `accountSid` are all from the `twilio console`
`from` is the number displayed on outgoing calls, this number must be authenticated by `twilio`
![twilio-json](https://raw.githubusercontent.com/jaynsw/bustake-site/main/docs/images/twilio-json.png)


etc
![etc](https://raw.githubusercontent.com/jaynsw/bustake-site/main/docs/images/etc.png)