# Quick start

The Twilio SDK includes languages such as NodeJS, Java, C#, PHP, Python, Ruby, Go, but now we have another new option which is designed purposely for the 'Voice app' - `Bustake Script`.

## hello world
<!-- tabs:start -->

#### **Bustake**
```bustake script

audio name='hello_world', tts='Hello world!';
play_audio name='hello_world';

```
#### **Node JS**

```Node JS

const http = require('http');
const VoiceResponse = require('twilio').twiml.VoiceResponse;

http
  .createServer((req, res) => {
    // Create TwiML response
    const twiml = new VoiceResponse();

    twiml.say('Hello World!');

    res.writeHead(200, { 'Content-Type': 'text/xml' });
    res.end(twiml.toString());
  })
  .listen(1337, '127.0.0.1');

console.log('TwiML server running at http://127.0.0.1:1337/');

```

#### **Python**

```Python
from flask import Flask
from twilio.twiml.voice_response import VoiceResponse

app = Flask(__name__)


@app.route("/answer", methods=['GET', 'POST'])
def answer_call():
    """Respond to incoming phone calls with a brief message."""
    # Start our TwiML response
    resp = VoiceResponse()

    # Read a message aloud to the caller
    resp.say("Hello World!")

    return str(resp)

if __name__ == "__main__":
    app.run(debug=True)

```

#### **PHP**
```PHP

<?php
require __DIR__ . '/vendor/autoload.php';
use Twilio\TwiML\VoiceResponse;

// Start our TwiML response
$response = new VoiceResponse;

// Read a message aloud to the caller
$response->say(
    "Hello World!"
);

echo $response;

```

#### **Ruby**

```Ruby
require 'bundler'
Bundler.require()

def self.get_or_post(url,&block)
  get(url,&block)
  post(url,&block)
end

get_or_post '/answer' do

	Twilio::TwiML::VoiceResponse.new do |r|
		r.say(message: "Hello World!")
	end.to_s

end

```

#### **Java**

```Java
import com.twilio.twiml.VoiceResponse;
import com.twilio.twiml.voice.Say;

import static spark.Spark.*;

public class VoiceApp {
    public static void main(String[] args) {

        get("/hello", (req, res) -> "Hello Web");

        post("/", (request, response) -> {
            Say say  = new Say.Builder(
                    "Hello World")
                    .build();
            VoiceResponse voiceResponse = new VoiceResponse.Builder()
                    .say(say)
                    .build();
            return voiceResponse.toXml();
        });
    }
}

```

<!-- tabs:end -->


## Two levels IVR

Let's build a 2 levels IVR for a car dealer's hotline. The first level is departments and the second level of the 'sales' department is 'booking a test drive' and 'order enquiry'.

```Call Flow Graph
.
└── Car Dealer hotline
    ├── 1. Customer service department
    ├── 2. Car Service department
    └── 3. Sales department
        ├── 1. Booking a test drive
        ├── 2. Order enquiry
```

<!-- tabs:start -->

#### **Bustake**

```bustake


audio name='department_menu', tts='press 1 for customer service, press 2 for car service, press 3 for sales';
ivr name='departments', audioRef='department_menu', numDigits=1;

play_ivr name='departments', loop=2;

numberTo = '+61023433xx';

switch GATHER_DIGITS {
    case '1' {
        numberTo = '+610234433xx';
    }
    case '2' {
        numberTo = '+613334332dd';
    }
    case '3' {
        audio name='sales_menu', tts='press 1 for booking a test drive, press 2    for order enquiry';
        ivr name='sales', audioRef='sales_menu', numDigits=1;

        play_ivr name='sales', loop=2;
        if GATHER_DIGITS == '1'{
            numberTo = '+6143322xx';
        } elsif GATHER_DIGITS == '2' {
            numberTo = '+619888cc';
        }
    }
}

dial value=numberTo;

```

#### **Node JS**

```node js

app.get('/hotline/dial', (req, res) => {
	dial(req, res);
});

app.get('/hotline/email', (req, res) => {
	email(req, res);
});

app.get('/hotline/level1', (req, res) => {
	level1(req, res);
});

app.get('/hotline/level2', (req, res) => {
	level2(req, res);
});


function level1(req, res){
    let loopCount = 2;
    let menu = 'press 1 for customer service, press 2 for car service, press 3 for sales';

    const response = new VoiceResponse();
    for (let i = 0; i < loopCount; i++) {
        let gather = response.gather(attrs);
        gather.action('/hotline/level2');
        gather.say(menu);
    }

    response.redirect({method: 'GET'}, '/hotline/dial');
    res.send(response.toString());
}

function dial(to){
    //....100 lines..skip
}

function email(to){
    //....another 100 lines..skip
}

function level2(req, res){
    let choice = req.query['Digits'];
    if (choice === '1'){
        //customer service
        dial('+610234433xx', req, res);
        return;
    }

    if (choice === '2'){
        // car service departement
        dial('+613334332dd', req, res);
        return;
    }

    let loopCount = 2;
    let menu = 'press 1 for booking a test drive, press 2 for order enquiry';

    const response = new VoiceResponse();
    for (let i = 0; i < loopCount; i++) {
        let gather = response.gather(attrs);
        gather.action('/hotline/dial');
        gather.say(menu);
    }

    response.redirect({method: 'GET'}, '/hotline/dial');
    res.send(response.toString());
}

```

#### **Python**

```python
    //too difficult I am still learning how to write...
```

#### **PHP**

```php
    //too difficult I am still learning how to write...
```

#### **Ruby**

```ruby
    //too difficult I am still learning how to write...
```

#### **Java**

```java
    //too difficult I am still learning how to write...
```

<!-- tabs:end -->


Next >> [language reference](language.md).
