# Codeforcer notes from fork  
This is a dirty hack over the original package to fix the fact that it is broken in React due to its `request` dependency which relies on `fs`.  

To fix this issue I've replaced `requests` with `axios`. However, to use it the output data for the functions need to be wrangled differently, ie the following code will print translations:  
```
translate(['Hello', 'Goodbye'], 'en','ar', (response) => console.log(response.error.data.data.translations));
```

At some point I might clean it up further, but my intention here is just to make the package work rather than maintain it. Currently none of the official Google packages for cloud translation work on React frontend without ejection (they all require `fs`).

To install this package simply install the package like you would originally then update the google-translate line in `package.json` to:
```
"google-translate": "CodeForcer/node-google-translate",
```

Then run:
```
yarn upgrade google-translate
```


Google Translate API for Node
=====================

A Node.js module for working with the [Google Cloud Translation API](https://cloud.google.com/translate/docs/). 

Automatically handles bulk translations that exceed the Google Translation API query limit.

Installation
----------

Install via [npm](http://npmjs.org/)

    npm install google-translate --save


Usage overview
----------

Require module and pass in your API Google project API key after enabling the API service. Note: This module currently doesn't support oauth authentication.
  
    var googleTranslate = require('google-translate')(apiKey);
    
String translation:

    googleTranslate.translate('My name is Brandon', 'es', function(err, translation) {
      console.log(translation.translatedText);
      // =>  Mi nombre es Brandon
    });

Language detection:

    googleTranslate.detectLanguage('Gracias', function(err, detection) {
      console.log(detection.language);
      // =>  es
    });

# API

**Callbacks**: All methods take a callback as their last parameter. Upon method completion, callbacks are passed an error if exists (otherwise null), followed by a response object or array: `callback(err, data)`.

**Bulk translations**:  Passing an array of strings greater than 5k characters will be result in multiple concurrent asynchronous calls. Once all calls are completed, the response will be parsed, merged, and  passed to the callback. The default maximum concurrent requests is 10. You can override this value by passing in a new limit when you pass in your API key: `require('google-translate')(apiKey, concurrentLimit)`

### Translate

Translate one or more strings.

    googleTranslate.translate(strings, source, target, callback)
    
* **strings**: Required. Can be a string or an array of strings
* **source**: Optional. Google will autodetect the source locale if not specified. [Available languages](https://developers.google.com/translate/v2/using_rest#target)
* **target**:  Required. Language to translate to. [Available languages](https://developers.google.com/translate/v2/using_rest#target)
* **callback**:  Required.

*Example*: Translate a string to German (de) and autodetect source language

    googleTranslate.translate('Hello', 'de', function(err, translation) {
      console.log(translation);
      // =>  { translatedText: 'Hallo', originalText: 'Hello', detectedSourceLanguage: 'en' }
    });

*Example*: Translate an array of English (en) strings to German (de)

    googleTranslate.translate(['Hello', 'Thank you'], 'en', 'de', function(err, translations) {
      console.log(translations);
      // =>  [{ translatedText: 'Hallo', originalText: 'Hello' }, ...]
    });
  
### Detect language

Detect language of string or each string in an array.

    googleTranslate.detectLanguage(strings, callback)
    
* **strings**: Required. Can be a string or an array of strings
* **callback**:  Required.
 
*Example*: Detect language from a string

    googleTranslate.detectLanguage('Hello', function(err, detection){
      console.log(detection);
      // =>  { language: "en", isReliable: false, confidence: 0.5714286, originalText: "Hello" }
    });

*Example*: Detect language from an array of strings

    googleTranslate.detectLanguage(['Hello', 'Danke'], function(err, detections) {
      console.log(detections);
      // =>  [{ language: "en", isReliable: false, confidence: 0.5714286, originalText: "Hello" }, ...]
    });


### Get supported languages

Retrieve all languages supported by the Google Translate API.


    googleTranslate.getSupportedLanguages(target, callback)
    
* **target**: Optional. If specified, response will include the name of the language translated to the specified target language
* **callback**:  Required.

*Example*: Get all supported language codes

    googleTranslate.getSupportedLanguages(function(err, languageCodes) {
      console.log(languageCodes);
      // => ['af', 'ar', 'be', 'bg', 'ca', 'cs', ...]
    });
    
*Example*: Get all supported language codes with language names in German

    googleTranslate.getSupportedLanguages('de', function(err, languageCodes) {
      console.log(languageCodes);
      // => [{ language: "en", name: "Englisch" }, ...]
    });

  
# Contribute

Forks and pull requests welcome!

# TODO
* Add authentication using new Google Cloud oauth token
* Add tests
* Better way of defining API keys to allow use of multiple Google Translate API keys
* Changelog

# Author

Maintained by [Localize](https://localizejs.com).
