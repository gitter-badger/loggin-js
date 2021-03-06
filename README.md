# Loggin' JS
A logger similar to the [one used in python](https://docs.python.org/2/library/logging.html) for NodeJS.

> Based on standard RFC3164

[![npm version](https://badge.fury.io/js/loggin-js.svg)](https://badge.fury.io/js/loggin-js)
[![npm](https://img.shields.io/npm/dm/loggin-js.svg?colorB=blue)](https://www.npmjs.com/package/loggin-js)

[![CircleCI](https://img.shields.io/circleci/project/github/nombrekeff/loggin-js.svg)](https://www.npmjs.com/package/loggin-js)
[![David](https://img.shields.io/david/nombrekeff/loggin-js.svg)](https://david-dm.org/nombrekeff/loggin-js?view=tree)
[![Maintainability](https://api.codeclimate.com/v1/badges/bef29a84728f23c52c21/maintainability)](https://codeclimate.com/github/nombrekeff/loggin-js/maintainability)
[![Coverage Status](https://coveralls.io/repos/github/nombrekeff/loggin-js/badge.svg?branch=master)](https://coveralls.io/github/nombrekeff/loggin-js?branch=master)
### References
* [Get started](https://github.com/nombrekeff/logging-js/wiki/Get-Started)
* [Basic Usage](https://github.com/nombrekeff/logging-js/wiki/Basic-Usage)
* [Loggers](https://github.com/nombrekeff/logging-js/wiki/Logger)
* [Notifiers](https://github.com/nombrekeff/logging-js/wiki/Notifier)
* [Severity](https://github.com/nombrekeff/logging-js/wiki/Severity)
* [Examples](https://github.com/nombrekeff/logging-js/wiki/Examples)
* [Collaborating](#Collaborating)


### Get-Started
* Install with npm
```bash
npm install loggin-js --save
```

* Using in node
```js
// Require the logging library
const logging = require('loggin-js');
```

### Basic-Usage
##### Basic Example
In this example we create a new logger with a severity of DEBUG, and we set color to true.  
This means it will output any log to the console as DEBUG englobes all other severities

We create it making use of the `logging.getLogger(options?)` method that creates a logger based on the options.  
_There are other ways of creating a Logger as described in the examples and docs_

```js
// Require the logging library
const logging = require('loggin-js');

// Shortcuts
const { Severity } = logging;

// Get a logger with DEBUG severity. 
// Severity DEBUG will output any severity.
const logger = logging.getLogger({
  level: Severity.DEBUG,
  color: true
});

// Does the same as passing into settings, as done above
logger.setLevel(Severity.DEBUG);
logger.setColor(true);


// Available predefined log levels
logger.info('info', { user: 'pedro', id: 10 });
logger.error('error');
logger.warning('warning');
logger.alert('alert');
logger.emergency('emergency');
logger.critical('critical');
logger.debug('debug');
logger.notice(['notice', 'notice']);


// If enabled set to false logs will not be output
logger.setEnabled(false);
```


##### File Logging Example
Log to files instead of the console

We create it making use of the `FileLogger` class.  
```js
// Require the logging library
const logging = require('loggin-js');

// Shortcut for the severity constants
const { Severity, Loggers, Notifiers } = logging;

// Create a file logger
const logger = new Loggers.FileLogger({
  
  // Display line number at the begining of the log 
  lineNumbers: true,

  // You can pass a pipes array to the file logger or you can do after instancing (showed below)
  pipes: [
    // Here we create a pipe that will pipe level ERROR logs to the file 'logs/error-logs.log'
    new Notifiers.Pipe(Severity.ERROR, 'logs/error-logs.log'),
    // This one will pipe level INFO logs to the file 'logs/info-logs.log'
    new Notifiers.Pipe(Severity.INFO, 'logs/info-logs.log')
  ]
});

// You can also add pipes after creating the logger as follows
logger.pipe(Severity.ERROR, 'logs/error-logs.log');
logger.pipe(Severity.INFO, 'logs/info-logs.log');


// INFO message will log to 'logs/info-logs.log'
logger.info('Logging a info log');

// ERROR message will log to 'logs/error-logs.log'
logger.error('Logging a error log', new Error('An error'));

// Will not be logged as only the ERROR and INFO severities will be output to their respective files
logger.warning('Logging a warning log');
logger.notice('Logging a notice log');
logger.alert('Logging a error log');
```

##### Custom Formater Example
Custom formatter, customize the output of the log 
```js
const logging = require('loggin-js');
const logger = logging.getLogger({
  level: logging.Severity.DEBUG,
  color: true,

  /**
   * You can also use a custom formater if the default one does not satisfy your needs.
   * In the formater you can access all log properties and you can also set the 
   * color of some segments of the log by using <%L> where L is one of:
   *  - r red
   *  - g green
   *  - b blue
   *  - p pink
   *  - y yellow
   *  - c cyan
   *  - m magenta
   *  - (nnn) a number between 0-255 # not implemented yet
   *
   * If you use %b lets say it will color until the breakpoint: [-,_|]  
   */
  formater: '[{time.toLocaleString}] - <%m{user}> | {severityStr} | {message} - {JSON.stringify(data)}'
});

// Set user to root
logger.setUser('root');

// Set formatter
logger.setFormater('[{time.toLocaleString}] - <%m{user}> | {severityStr} | {message} - {JSON.stringify(message)}');

// Log something
logger.debug('debug');             // $ [2018-6-2 00:46:24] - root - DEBUG - debug
logger.info('info', {data: 'Hi'}); // $ [2018-6-2 00:46:24] - root - INFO - info - {"data":"Hi"}
```


### Collaborating
Hi there, if you like the project don't hesitate in collaborating (_if you like to_), submit a pull request, post an issue, ...   
It's just a little sideproject, nothing serius, so its just for fun!  
But any **help** or **ideas** are apreciated!
