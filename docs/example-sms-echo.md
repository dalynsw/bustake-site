
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
```Jay Liu, [11.08.21 16:39]
// version=1, timezone='Australia/Sydney',voice='Polly.Russell'




if HOUR < 9 || HOUR > 22 {

    after_hours =`
[1s]
Sorry I am Jay.[200ms]
I am not available now.
Please call me between (9 am to 5 pm )[emphasis level="strong"]
Bye Bye.
`;
    audio name='after_hours', tts=after_hours;
    play_audio ref='after_hours';
    hang_up;
}

main_menu_tts =`
[500ms]
Hello, my name is Jay, and I am looking for a (Node JS Full Stack job.)[emphasis level="strong"]
Press 1 to know about my skills.
Press 2 to know about my work experience.
Press 3 to know that I expect to be paid.
Press 4 to find out the locations of office I am looking to work.
Press 5 to know my background.


Press 9 to talk to myself.
Press 0 to hear the options again.
`;

skill_tts=`
I have 20 years of experience in software development.
8 years of experience in Node JS.
4 years of experience in React. 
4 years of experience in Docker. 
20 years of experience in Java. 
10 years of experience in phone call control development. 
15 years of experience in web development.
`;

work_exp_tts=`
From October 2010 till now, I am a software engineer in Delacon. 

My work is as follows:
Develop telephone control modules.
Develop Call recording module.
Develop Transcription module to transcribe call recordings with the third-party SDK, including Google API, IBM API.
Develop Audio processing module to import audio files from outside and export to Cloud storage.
Review the customer technical demands and providing the tech solutions.


The technologies used include Java Tomcat, Spring Framekwork, MySQL, NODE JS, expreess JS, Docker, Docker-compose, Google SDK, IBM SDK.


From March 2009 to March 2010, I was a software engineer at NSW UNI. 

My work is as follows:

Develop personal health record software modules. 

The developed modules are as follows:
Blood  Test, Autisum therapy.

The technologies used include Java Tomcat, Spring Framekwork, MySQL.

From April 2000 to October 2008, I was a software engineer and team leader of Sina.com.

From 2006 to 2008, I was Video player development team leader. 

My works include:

Work with team to develop P2P video player.
Develop the live stream website.
Develop the DRM solution to  encrypt the live stream.

From 2003 to 2006, I worked at the new technology department. 
My jobs are development and maintenance of the company's common programming libraries.

From  2000 to 2003 I worked as engineer at Sina Mall Department.


the technologies used include Perl, C, C++, RPC, Java, PHP and MySQL.
`;

salary_tts=`
The salary I expect is 120,000 per year excluding super.
`;

office_tts=`
I hope to be able to work from home. Or near the following locations:
Belrose, Chatswood, North Syndey, Sydney CBD.
`;

background_tts=`
My education is a bachelor degree in engineering.
I am a 46 years old male.
I am an Australian citizen.
`;

audio name='main_menu', tts=main_menu_tts;
audio name='no_input', tts='[1s]no input detected.';
audio name='wrong_option', tts='[1s]wrong option entered, bye bye.';
ivr name='main_menu', audioRef='main_menu', audioNoInputRef='no_input', numDigits='1';

play_ivr ref='main_menu', maxLoop='2', timeout='10';

audio name='calling_wait', tts='[500ms] please wait, we are calling Jay.';

Jay Liu, [11.08.21 16:39]
switch GATHER_DIGITS {
    case '0' {
        redirect path='find_job.bus';
    }
    case '1' {
        audio name='skill', tts=skill_tts;
        play_audio ref='skill';
        redirect path='return_main_menu.bus';
    }
    case '2' {
        audio name='work', tts=work_exp_tts;
        play_audio ref='work';
        redirect path='return_main_menu.bus';
    }
    case '3' {
        audio name='salary', tts=salary_tts;
        play_audio ref='salary';
        redirect path='return_main_menu.bus';
    }
    case '4' {
        audio name='office', tts=office_tts;
        play_audio ref='office';
        redirect path='return_main_menu.bus';
    }
    case '5' {
        audio name='background', tts=background_tts;
        play_audio ref='background';
        redirect path='return_main_menu.bus';
    }
    case '9' {
        play_audio ref='calling_wait';
        dial value='+61403155614';
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