# Jedi Validate

[![NPM](https://nodei.co/npm/jedi-validate.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/jedi-validate/)

* [Introduction](#intro)
* [Installation and Usage](#install)
* [Options](#options)
  * [Default Options](#default)
  * [Ajax Parameters](#ajax)
  * [Data Encoding Options](#encoding)
* [Validation Rules](#rules)
* [Error Message](#error)
* [Changelog](#changelog)

![logo](http://oi68.tinypic.com/t7o1n9.jpg)
<h1>
    <a name="intro"></a>
    Introduction
</h1>
Jedi Validate is a lightweight form validation component.

## How Can I Use It?

This is a JS class, and you can create a new instance by passing in a DOM element and an options object.

```javascript
    new JediValidate(formWrapper, options);
```

By default, the form will be sent via ajax with the parameters which were set in HTML.

## Why Should I Use It?

Because it provides a strict json format for interaction. You can send a form in many different ways:

* serialized
* as a JSON object
* as FormData.

But server answer always has one structure. It is easier to implement.

<h2>
    <a name="install"></a>
    Build and Test
</h2>
If you would like to build the source code, run tests, or contribute, then first fork or clone this repo onto your local machine. Ensure NodeJS is installed and in turn npm. Check in a terminal with `node -v` and `npm -v`.

To install project dependencies first run,

```
npm install
```
### Build
To build the source and watch for changes in the terminal run,

```
npm run build
```

### Build and Serve
To bundle the source and serve it to `localhost:8080` run,

```
npm run dev
```
This will open a webpack local server where you can navigate to the desired directory or resource. The test page is located in **`example/bootstrap.html`**

### Running Tests
The tests are not yet complete, and runtime errors will occur when attempting to run the tests in the console or through the test browser.

<h1>
    <a name="options"></a>
    Options
</h1>

There are three types of options:

* Default component options
* Form attributes such as action or method
* Initialization options

<h2>
   <a name="default"></a>
   Default Options
</h2>


```javascript
    {
        ajax: {
            url: null,
            enctype: 'application/x-www-form-urlencoded',
            sendType: 'serialize', // 'formData', 'json'
            method: 'GET'
        },
        rules: {},
        messages: {},
        containers: {
            parent: 'form-group',
            message: 'help-block',
            baseMessage: 'base-error'
        },
        states: {
            error: 'error',
            valid: 'valid',
            pristine: 'pristine',
            dirty: 'dirty'
        },
        callbacks: {
            success: function () {
            },
            error: function () {
            }
        },
        clean: true,
        redirect: true
    }
```

<h2>
  <a name="ajax"></a>
  ajax
</h2>

Under the ajax option we define how to send the form.
It can be ```null``` if we do not want the form to be sent,
or it can be an object with the following options:

#### url
default: ```null```
Can be overridden by the `action` form attribute or init options.

#### enctype
default: ```'application/x-www-form-urlencoded'```
Can be overridden by the `enctype` form attribute, init options, or `sendType`.

#### method
default: ```'GET'```
Can be overridden by the `method` form attribute or init options.

<a name="encoding"></a>
#### sendType
default: ```'serialize'```

You can encode and send the data in three different ways. Valid options are:

* ```'formData'``` - send form as FormData. ```'Content-type'``` to ```'multipart/form-data'```
* ```'json'``` - send form as a JSON object. Set ```'Content-type'``` to ```'application/json; charset=utf-8'```
* ```'serialize'``` - send form as a regular request. Set ```'Content-type'``` to ```'application/x-www-form-urlencoded'```

Files can only be sent using 'formData' encoding.

#### serialize

```
    name=111&phone=222222222&email=wow%40wow.com
```

#### formData

```
-----------------------------678106150613000712676411464
Content-Disposition: form-data; name="name"

111
-----------------------------678106150613000712676411464
Content-Disposition: form-data; name="phone"

222222222
-----------------------------678106150613000712676411464
Content-Disposition: form-data; name="email"
...
```

#### json

```json
    {"name":"111","phone":"222222222","email":"wow@wow.com","file":"index.html"}
```

<h1>
  <a name="rules"></a>
  Validation Rules
</h1>

Rules used to validate input. Each form element will be matched by the 'name' attribute with a corresponding rule, if one exists. If no rule exists, then no validation will occur.

#### Basic Rules:

Rules are not defined by default, but they can be set via attributes or classes in HTML, or in the init options.

> - required :  boolean
> - regexp : RegExp
> - email :  boolean
> - tel :  boolean
> - url :  boolean
> - filesize: number
> - extension: string

These attributes can be used
> - type - email, tel or url (regexp will be used for each type)
> - pattern - regexp with attribute value
> - required - check input for empty value

Example:

```html
    <input id="name" type="text" name="name" required class="required">
    <input id="email" type="email" name="email" class="required">
```

* type="email" or class="email" to validate as email
* required or class="required" to validate as a required field

#### Custom Validation Rules

You can set your own rules using the ```addMethod``` function:

```
JediValidate.addMethod('methodName', function (value, element, options) {
    return // true if valid
}, 'Error message');
```

### Initialization Options Example

Add rules as part of your options object when initializing:

```javascript
    new JediValidate(formWrapper, {
        rules: {
            name: {
                required: true
            },
            email: {
                email: true
            },
            phone: {
                regexp: /^([\+]+)*[0-9\x20\x28\x29\-]{5,20}$/
            },
            file: {
                filesize: 10000,
                extension: "html|css|txt"
            }
        }
    });
```

<h1>
  <a name="error"></a>
  Error Messages
</h1>

You can define your own error messages in case validation fails. In case a form element fails validation, then the message corresponding to the element's 'name' attribute will apply.

```javascript
    messages: {
        phone: {
            regexp: "Invalid phone number"
        },
        file: {
            filesize: "File is too big"
        }
    },
```

<h1>
  <a name="changelog"></a>
  Changelog
</h1>
- 1.0.6 fix bug with sending
- 1.0.5 add enhanced language support
- 1.0.4 add simple language support
