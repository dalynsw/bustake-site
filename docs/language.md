# Language Reference

## Common type value
`Bus Script` has four common data value types, `string`, `number`, `boolean`, and `array`

### string
string values, For example: 
```
'hello world'
"hello world"
`hello world`
```

### number
Numeric values ​​can be integers or floating point numbers. For example: `1, 2, 3` or `1.10, 2.20, -3.4`

### boolean
Boolean value can only be `true` or `false`.

### array
An array is a container type that can hold other types of values. The values ​​in the array can be different types of values. For example: `[1,2,'hello',4]` or `[1.01, [1, 2],- 3,'hello',true, 4]`


## Object
Objects are containers of various types of values. Objects can be used to hold string values, numeric values, boolean values, array values, and the values ​​of built-in objects. Once an object is created, it can be referenced anywhere in the script afterwards.

```object
a = 'hello world'; //`a` is a string object, the value is `hello world`
b = 10; //`b` is a number object, the value is 10
c = [1, 2, 'love', 4]; //`c` is an array type. c[2] is the string value `love`.
e = a; //The value of the `e` object is the same as the `a` object, which is a string value `hello world`
f = true; //`f` is a boolean object, the value is `true`
```

## expressions
The result of an expression is a value. This value comes from the calculation of the value of other objects. These values ​​are connected by operators. Different operators have different precedence.
The value of the expression can be a string value, a boolean value, a number value, an array value.

 The following is the precedence
 -  operators:
    - `()`
    - `[]`
    - `not !`
    - `* / %`
    - `+ -`
    - `|| && or and`
    - `> < >= <= == !=`

### Literal expression
Literal expressions are literal values. For example, these are literal expressions, and they are also literal values: `1, 2, 0.9,'abc', [1,23,'abc']`

### Logical expression
The value of the logical expression is a boolean. The logical expression is connected by the following operators `not` `!` `||`, `&&`, `or`, `and`. For example: `ab && cd` `cc or abc` ` ee || df`

### Comparison expression
The value of the comparison expression is a boolean value. The operators of the comparison expression are as follows: `>` `<` `>=` `<=` `==` `!=`. For example: `ab > 5` `cc <= abc` `ee == df`

### Bracket expression
Bracket expression is a group. This group will have high priority. For example `(bc> 5 || isTrue)`

### string operator
The result value of the string operator is a string.
For example: `'abc' +'dfd' +'adf'`  `abcdfdadf`

### numeric operator
The result value of a numeric operator is a number.
The numeric operators are as follows `+ - * / %`



## Statements
A statement is the most basic executable unit of a script. A script is a series of executable statements

### assignment
The `assignment` statement is used to assign a value to an object. `;` is the end character of the statement.

```object
a = 'hello world';
b = 10;
c = [1, 2, 'love', 4];
e = a;
```

## block statement
A statement block is a collection of multiple statements executed in sequence. `{` indicates the beginning of a statement block. `}` indicates the end of a statement block.

```block
if good { //a starts of a block
    
    a = b;
    c = d;
    
} //an ends of a block

switch condition { // a starts of a block
    case 'abc { // a starts of another block
        e = ds;
        email subject='sdfsdf', content='sfsdf';
    } // an ends of the above block
    case 'cdde' { // a starts of another block
        noop;
    } // an ends of the above block
} // an ends of a block

```

### switch case
The `switch case` statement contains a switch expression, multiple `case` blocks and a `default` block. Each `case` block contains one `expression` and one `block`, if the `case expression` is equal to `switch expression`, the `block` of the `case statement` is run. If there is no `case expression` equal to the `witch expression`, the `default block` is run.

```switch case
switch age {
    case 100 {
        a = b;
        c = d;
    }
    case 90 {
        ds = 'ddd';
    }
    case 80 {
        fsdf = 123;
    }
    default {
        ddd = 'asdfasf';
        dff = 223;
    }
}
```


### if
The `if statement` consists of two parts, one `Boolean expression` and one `block`. This `block` is run when the Boolean value is `true`.

```if
if a > 5 {
    //if block
    a = b;
    c = d;
}
```

### if else
The `if else statement` contains three parts, one `Boolean expression` and tow `block`. When the Boolean value is `true`, the `if block` is executed, and when the value is `false`, the `else block` is executed.


```if else
if a > 5 {
    //if block
    a = b;
    c = d;
} else {
    //else block
    ddd = 'asdfasf';
    dff = 223
}
```

### if elsif else
The `if else statement` contains one `if`, multiple `elsif` and one `else`. When the `if` boolean value is `true`, the `if` block is executed, otherwise the value of `elsif` is calculated When `true`, the corresponding `elseif block` is executed, and when all the `previous blocks` are not satisfied, the `else block` is executed.

```if elsif else
if a > 5 {
    //if block
    a = b;
    c = d;
} elsif a > 2 {
    //elsif  block
    ds = 'ddd';
} elsif a > 1  {
    //elsif  block
    fsdf = 123;
} else {
    //else  block
    ddd = 'asdfasf';
    dff = 223;
}
```


### fetch
The `fetch statement` issues an http get request, this request returns multiple rows of keys and values. ​​which are used to create a series of `string objects`. `;` is the end character of the statement.

- Attributes
    - url: string type. http get url.
    - timeout: Number type. Timeout, in seconds.

```fetch
fetch url='https://www.test.ccc/abc', timeout=2;
```

The return must be multiple lines, each line contains a `key=value` as follows:

```fetch result
key1=value1
key2=value2
key3=value3
```

- line 1 will create an string object `key1` with value `value1`
- line 2 will create an string object `key2` with value `value2`
- line 3 will create an string object `key3` with value `value3`

### email
The `email statement` is used to send email to the specified address, and `;` is the end character of the statement.

- Attributes
    - to: string type. Email receiver address.
    - subject: string type. Email subject
    - text: string type. Email text content

```email
email to='abc@abc.com', subject='abc', content='cddd';
```


### log
The `logs statement` can be used to help debugging. The value of the concerned object can be composed into a string and printed to the log file through the `log statement` to help analyze errors and call flow. `;` is the end character of the statement.

- Attributes
    - message: string value. This string will be printed to the log.

```log
    counter = 100;
    value = 'hello';
    message = template format='counter is %d, value is %s', values=[100, hello];
    log message=message;
```

## Built-in functions
Built-in functions can be used to process string values, numeric values.


### starts_with
Determine whether the string value contains the specified string value prefix.

- Return Value: 
    -  Boolean type. `true` means match and `false` means no match.

- Attributes:
    - value: String object
    - prefix: Prefix string value

```starts_with
    isUSA = starts_with value=FROM, prefix='+1';
```

Determine whether the `FROM` string object starts with prefix `+1`. `isUSA`  is assigned the boolean object.


### ends_with
Determine whether the string value contains the specified string suffix.

- Return Value: 
    - Boolean type. `true` means match and `false` means no match.

- Attributes:
    - value: String object
    - suffix: Suffix string value

```starts_with
    isEndABC = ends_with value=TO, suffix='abc';
```

The result of judging whether the `TO` string object ends with the `abc suffix`.  the result is ​​assigned to the `isEndABC` boolean object.

### template
`template` functions are used to generate formatted string objects

- Return Value:
    - String object

- Attributes:
    - format: Formated template string
    - values: The parameter array in the template.

```template
email_content = template format="I love %s, I am %s",  values=['Twilio', 'Ben'];
```

The `email_content` string object is created by the string value of `I love Twilio, I am Ben`

### split
`split` string values ​​into array of string objects.

- Return Value:
    - Array of string objects.

- Attributes:
    - value: String Object
    - separator: Delimiter string.

```split
values = split value="a&b&c&d", separator="&";
```
Split the string `a&b&c&d` according to the `& separator`, the result is `['a','b','c','d']`


### match
Use regular expressions to match strings

- Return Value:
    - boolean value.

- Attributes:
    - value: string object
    - pattern:  string object of the regular express

```match
is_number = match value="1234", pattern='[0..9]+';
```

 `is_number` is a boolean object with value `true` assigned from the `match function`  when value is  `1234` and `pattern` is the  regex `[0..9]+`


### number
Convert string value to numeric value

- Return Value:
    - number value.

- Attributes:
    - value: string object

```number
    num = number value='123';
```

Convert the string `'123'` into a numeric value `123`

### string
Convert a numeric value to a string value

- Return Value:
    - string value

- Attributes:
    - value: number value

```number
    str = string value=123;
```

Convert the numeric value `123` to the string `'123'` 

### random
Generate random number value

- Return Value:
    - number value.

- Attributes:
    - min: number value. Minimum
    - max: number value. Maximum

```random
num = random min=0, max=100;
```


### substring
Take substring

- Return Value:
    - Substring value.

- Attributes:
    - value: strong object.
    - start: number value. Substring start point
    - end: number value. End of substring


```substring
    sstr = substring value='abcde', strart=1, end=4;
```

cut the substring`'bcd'` and assign it to the string object `sstr`


### url_encode
Encode url

- Return Value:
    - string object.

- Attributes:
    - value: string object.

```url_encode
    url = template formt='a=%s&b=%s', values=[a, b];
    url = url_encode value=url;
    fetch url=url;
```


### template_url_encode
Generate the url of the `handlebars` template

- Return Value:
    - url string object.

- Attributes:
    - path: string object, templte file path.


```template_url_encode
    tellingURL = templte_url_encode path='telling.hb';
```

### static_url_encode
Generate static file url
- Return Value:
    - url string object.

- Attributes:
    - path: String, template file path.

```static_url_encode
    tellingURL = static_url_encode path='abc/cde/telling.xml';
```



## Built-in objects

APP_ID
- Type: string

SCRIPT_NAME
-  Type: string

URL
- Type: string

TIME_ZONE
- Type: string
such as America/Los_Angeles


START_TIME
- Type: Number
the number of milliseconds elapsed since January 1, 1970 00:00:00 UTC.


YEAR
- Type: Number
such as 2020


MONTH
- Type: Number. 1-12

DATE
- Type: Number 1-31

HOUR
- Type: Number 0-23

MINUTE
- Type: Number 0-59

SECOND
- Type: Number 0-59

WEEK_DAY
- Type: Number 1-7

MILLI_SECOND
- Type: Number 0 - 1000
