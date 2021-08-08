# Quick start

The Twilio SDK 包含了不同的编程语言, 例如 NodeJS, Java, C#, PHP, Python, Ruby, Go,但现在,我们提供了一个新的编程语言,这是一个专门为语音和短信设计的编程语言
 but now we have another new option which is designed purposely for the  - `Bus Script`.

## hello world

This App will play `Hello World` voice to incoming calls.

```bustake script

audio name='hello_world', tts='Hello world!';
play_audio name='hello_world';

```


## Two levels IVR

这是一个两层的IVR 第一层有3个选项,其中选项3又包含两个选项的.不同的选项指向不同的电话号码. 根据选项IVR会转接电话到指定的号码.


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


audio name='department_menu', tts='press 1 for customer service, press 2 for car service, press 3 for sales';
ivr name='departments', audioRef='department_menu', numDigits=1;

play_ivr name='departments', loop=2;

numberTo = '1023433xx';

switch GATHER_DIGITS {
    case '1' {
        numberTo = '610234433xx';
    }
    case '2' {
        numberTo = '613334332dd';
    }
    case '3' {
        audio name='sales_menu', tts='press 1 for booking a test drive, press 2    for order enquiry';
        ivr name='sales', audioRef='sales_menu', numDigits=1;

        play_ivr name='sales', loop=2;
        if GATHER_DIGITS == '1'{
            numberTo = '6143322xx';
        } elsif GATHER_DIGITS == '2' {
            numberTo = '619888cc';
        }
    }
}

dial value=numberTo;

```


## 新用户注册

我们提供了一个免费的美国本地座机电话号码用于新用户注册. 您只需要拨打这个号码 `+1720-709-2385`, 根据提示,自动完成注册. 之后您会收到一条短信,内容包含您的账户的用户名和密码. 之后您可以用这个账户创建 APP 享受语音和短信编程的乐趣.


## 开发控制台

控制台登录链接  `https://console.bustake.com`

有两种App可以创建, Voice 和 SMS. Voice App可以用户处理inbound 和 outbound电话. SMS App 可以处理 inbound 和 outbound SMS.

![console apps](https://raw.githubusercontent.com/jaynsw/bustake-site/main/docs/images/apps.png)

一旦创建App后, 系统会创建App文件夹. 这个文件夹结构如下:



```
App
└── /
    ├── etc
    ├── logs
    ├── static
    ├── templates
    └── scripts
```

- `scripts` 是  `Bus Script` 脚本的根目录. 可以在这个文件夹下创建脚本或者文件夹. 每个脚本有对应的 URL. 这些 URL 可以粘贴和设置到 Twilio 的号码,用来处理电话或者短信.
- `etc` 是配置文件的根目录. 这个文件夹可以用来存储 Twilio API 配置文件 和 其它 第三方 API 的配置文件.
- `logs` 是日志文件夹. 每个电话都会生成一个日志文件. 包含了电话的流程和各种参数值, 可以用来跟踪和分析电话.
- `templates` 文件夹用来保存 `handlebars` 脚本. 这些脚本可以用来辅助电话控制.
- `static` 文件夹用来存储静态文件.

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



Next >> [Voice language reference](voice-language.md).