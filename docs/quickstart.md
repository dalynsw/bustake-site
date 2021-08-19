# Quick start

The Twilio SDK contains different programming languages, such as NodeJS, Java, C#, PHP, Python, Ruby, Go, but now, we provide a new programming language, which is a programming language specifically designed for voice and SMS - `Bus Script`.

## hello world

This App will play `Hello World` voice to inbound calls.

```bustake script
// version=1, timezone='America/Los_Angeles', voice='Polly.Emma'

audio name='hello_world', tts='Hello world!';
play_audio name='hello_world';

```


## Two levels IVR

This is a two-tier IVR. The first layer has 3 options, of which option 3 contains two options. Different options point to different phone numbers. According to the options, the IVR will forward the call to the specified number.


```Call Flow Graph
.
└── Car Dealer hotline
    ├── 1. Customer service department
    ├── 2. Car Service department
    └── 3. Sales department
        ├── 1. Booking a test drive
        ├── 2. Order enquiry
```



```bustake
// version=1, timezone='America/Los_Angeles', voice='Polly.Emma'

audio name='department_menu', tts='press 1 for customer service, press 2 for car service, press 3 for sales';
ivr name='departments', audioRef='department_menu', numDigits=1;

play_ivr name='departments', loop=2;

numberTo = '+1023433xx';

switch GATHER_DIGITS {
    case '1' {
        numberTo = '+10234433xx';
    }
    case '2' {
        numberTo = '+13334332dd';
    }
    case '3' {
        audio name='sales_menu', tts='press 1 for booking a test drive, press 2    for order enquiry';
        ivr name='sales', audioRef='sales_menu', numDigits=1;

        play_ivr name='sales', loop=2;
        if GATHER_DIGITS == '1'{
            numberTo = '+143322xx';
        } elsif GATHER_DIGITS == '2' {
            numberTo = '+19888cc';
        }
    }
}

dial value=numberTo;

```


## New User Registration

New users can go to the console to create an account. After successful registration, a email is sent. The email content contains the username and password.

## Development console

Console login [link](https://console.bustake.com)

There are two kinds of apps that can be created, Voice and SMS. Voice App can handle inbound and outbound calls. SMS App can handle inbound and outbound SMS.

![console apps](https://raw.githubusercontent.com/jaynsw/bustake-site/main/docs/images/apps.png)

Once the App is created, the system will create an App folder. The folder structure is as follows:

```
App
└── /
    ├── etc
    ├── logs
    ├── static
    ├── templates
    └── scripts
```

- `scripts` is the root directory of the `Bus Script` script. Scripts or folders can be created in this folder. Each script has a corresponding URL. These URLs can be pasted and set to Twilio's number to handle calls or text messages.
- `etc` is the root directory of configuration files. This folder can be used to store Twilio API configuration files and other third-party API configuration files.
- `logs` is the log folder. Each call will generate a log file. It contains the process of the call and various parameter values, which can be used to track and analyze the call.
- The `templates` folder is used to store `handlebars` scripts. These scripts can be used to assist phone call control.
- The `static` folder is used to store static files.

![app folder](https://raw.githubusercontent.com/jaynsw/bustake-site/main/docs/images/app-folder.png)

![scripts folder](https://raw.githubusercontent.com/jaynsw/bustake-site/main/docs/images/scripts-folder.png)

![script](https://raw.githubusercontent.com/jaynsw/bustake-site/main/docs/images/script.png)

![twilio.json](https://raw.githubusercontent.com/jaynsw/bustake-site/main/docs/images/twilio-json.png)



- The url of the script is
```
    └── /
    ├── etc
    ├── logs
    └── scripts
           └── test.bus    //https://twlo.bustake.com/script/voice/{appid}/test.bus

```