# Text to Speech

This is a very powerful feature of the bustake script. It allows you to compose a synthesis voice to make the phone call more like a human natrual speaking.


The tts content can be used in  the `audio` statement's `tts` attribute

## Paragraphs

[amazon docs - p](https://docs.aws.amazon.com/polly/latest/dg/supportedtags.html#p-tag)

A tts can include multi paragraphs. These paragraphs can be separated by the multi `carige return` keys.

The following example contains 2 paragraphs. The second paragraph starts from `Press 1 for (registration.)[emphasis level="strong"] [200ms]`



```paragraphs
main_menu_tts =`
[1s]
Welcome to bus take.
Please listen carefully the following menu.

Press 1 for (registration.)[emphasis level="strong"] [200ms]
Press 2 for (changing password.)[emphasis level="strong"] [200ms]
`;
```

## Sentences

[amazon docs - s](https://docs.aws.amazon.com/polly/latest/dg/supportedtags.html#s-tag)

Each line is a sentence. No matter there  is a `.` at the end or not. 

## Text

Text is the normal words you don't want to polish the pronauncation.

## break

The break can be in `seconds` or `milli seconds`.
`[2s]`   `[200ms]`

[amazon doc - break](https://docs.aws.amazon.com/polly/latest/dg/supportedtags.html#break-tag)


## Modifiers

Modifier is the  words you want to polish the pronauncation.

The format of Modifier is `(words words word)[modifer key1="value1", key2="value2", key3="value3"]`


### sayAs

[amazon doc - say_as](https://docs.aws.amazon.com/polly/latest/dg/supportedtags.html#say-as-tag)

### emphasis

[amazon doc - emphasis](https://docs.aws.amazon.com/polly/latest/dg/supportedtags.html#emphasis-tag)

level attribute values:

 - strong: Increases the volume and slows the speaking rate so that the speech is louder and slower.
 - moderate: Increases the volume and slows the speaking rate, but less than strong. Moderate is the default.
 - reduced: Decreases the volume and speeds up the speaking rate. Speech is softer and faster.

(changing password.)[emphasis level="strong"]

### prosody

[amazon docs - prosody](https://docs.aws.amazon.com/polly/latest/dg/supportedtags.html#prosody-tag)

  - volume
    default: Resets volume to the default level for the current voice.
    silent, x-soft, soft, medium, loud, x-loud: Sets the volume to a predefined value for the current voice.
    +ndB, -ndB: Changes volume relative to the current level. A value of +0dB means no change, +6dB means approximately twice the current volume, and -6dB means approximately half the current volume.

  - rate
    x-slow, slow, medium, fast,x-fast. Sets the pitch to a predefined value for the selected voice.
    n%: A non-negative percentage change in the speaking rate. For example, a value of 100% means no change in speaking rate, a value of 200% means a speaking rate twice the default rate, and a value of 50% means a speaking rate of half the default rate. This value has a range of 20-200%.
  - pitch
    default: Resets pitch to the default level for the current voice.
    x-low, low, medium, high, x-high: Sets the pitch to a predefined value for the current voice.
    +n% or -n%: Adjusts pitch by a relative percentage. For example, a value of +0% means no baseline pitch change, +5% gives a little higher baseline pitch, and -5% results in a little lower baseline pitch.


### w
[amazon docs - w](https://docs.aws.amazon.com/polly/latest/dg/supportedtags.html#w-tag)

### phoneme

[amaozn docs - phoneme](https://docs.aws.amazon.com/polly/latest/dg/supportedtags.html#phoneme-tag)

### sub

[amazon docs - sub](https://docs.aws.amazon.com/polly/latest/dg/supportedtags.html#sub-tag)