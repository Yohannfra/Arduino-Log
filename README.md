# ArduinoLog - C++ Log library for Arduino devices

[![License](https://img.shields.io/badge/license-MIT%20License-blue.svg)](http://doge.mit-license.org)

_An minimalistic Logging framework for Arduino-compatible embedded systems._

ArduinoLog is a minimalistic framework to help the programmer output log statements to an output of choice, fashioned after extensive logging libraries such as log4cpp ,log4j and log4net. In case of problems with an application, it is helpful to enable logging so that the problem can be located. ArduinoLog is designed so that log statements can remain in the code with minimal performance cost. In order to facilitate this the loglevel can be adjusted, and (if your code is completely tested) all logging code can be compiled out.

## Features

- Different log levels (Error, Info, Warn, Debug, Verbose )
- Supports multiple variables
- Supports formatted strings
- Supports formatted strings from flash memory
- Fixed memory allocation (zero malloc)
- Optional ANSI colors
- MIT License

## Tested for

- All Arduino boards (Uno, Due, Mini, Micro, Yun...)
- ESP8266

## Downloading

This package has been published to the Arduino & PlatformIO package managers, but you can also download it from GitHub.

- By directly loading fetching the Archive from GitHub:

1.  Go to [https://github.com/Yohannfra/Arduino-Log](https://github.com/Yohannfra/Arduino-Log)
2.  Click the DOWNLOAD ZIP button in the panel on the
3.  Rename the uncompressed folder **Arduino-Log-master** to **Arduino-Log**.
4.  You may need to create the libraries subfolder if its your first library.
5.  Place the **Arduino-Log** library folder in your **<arduinosketchfolder>/libraries/** folder.
6.  Restart the IDE.

## Quick start

```c++
    Serial.begin(9600);

    // Initialize with log level and log output.
    Log.begin   (LOG_LEVEL_VERBOSE, &Serial);

    // Start logging text and formatted values
    Log.error (  "Log as Error   with binary values             : %b, %B"CR    , 23  , 345808);
    Log.warning (F("Log as Warning with integer values from Flash : %d, %d"CR) , 34  , 799870);
```

[See examples/Log/Log.ino](examples/Log/Log.ino)

## Usage

### Initialisation

The log library needs to be initialized with the log level of messages to show and the log output. The latter will often be the Serial interface.
Optionally, you can indicate whether to show the log type (error, debug, etc) for each line.

```
begin(int level, Print* logOutput, bool showLevel)
begin(int level, Print* logOutput)
```

The loglevels available are

```
* 0 - LOG_LEVEL_SILENT     no output
* 1 - LOG_LEVEL_FATAL      fatal errors
* 2 - LOG_LEVEL_ERROR      all errors
* 3 - LOG_LEVEL_WARNING    errors, and warnings
* 4 - LOG_LEVEL_NOTICE     errors, warnings and notices
* 5 - LOG_LEVEL_TRACE      errors, warnings, notices & traces
* 6 - LOG_LEVEL_VERBOSE    all
```

example

```
    Log.begin(LOG_LEVEL_ERROR, &Serial, true);
```

if you want to fully remove all logging code, uncomment `#define DISABLE_LOGGING` in `ArduinoLog.h`, this may significantly reduce your sketch/library size.

### Log events

The library allows you to log on different levels by the following functions

```c++
void fatal   (const char *format, va_list logVariables);
void error   (const char *format, va_list logVariables);
void warning (const char *format, va_list logVariables);
void notice  (const char *format, va_list logVariables);
void trace   (const char *format, va_list logVariables);
void verbose (const char *format, va_list logVariables);
```

where the format string can be used to format the log variables

```
* %s	display as string (char*)
* %S    display as string from flash memory (__FlashStringHelper* or char[] PROGMEM)
* %c	display as single character
* %d	display as integer value
* %l	display as long value
* %u	display as unsigned long value
* %x	display as hexadecimal value
* %X	display as hexadecimal value prefixed by `0x`
* %b	display as  binary number
* %B	display as  binary number, prefixed by `0b'
* %t	display as boolean value "t" or "f"
* %T	display as boolean value "true" or "false"
* %D,%F display as double value
```

Newlines can be added using the CR or CRLF keywords.

### Enabling colors

To enable colors define **ARDUINO_LOGS_ENABLE_COLORS**
```c
#define ARDUINO_LOGS_ENABLE_COLORS
```

Once the colors are enabled the following color will be used automatically for each event:
- fatal -> bright red
- error -> red
- warning -> yellow
- notice -> green
- trace, verbose -> white

Example

```cpp
Log.fatal("Fatal log");
Log.error("Error log");
Log.warning("Warning log");
Log.notice("Notice log");
Log.trace("Trace log");
Log.verbose("Verbose log");
```



![screenshot](.github/with_colors.png)




### Storing messages in Flash memory

Flash strings log variables can be stored and reused at several places to reduce final hex size.

```c++
    const __FlashStringHelper * logAs = F("Log as");
    Log.fatal   (F("%S Fatal   with string value from Flash   : %s"CR    ) , logAs, "value"     );
    Log.error   (  "%S Error   with binary values             : %b, %B"CR  , logAs, 23  , 345808);
```

If you want to declare that string globally (outside of a function), you will need to use the PROGMEM macro instead.

```c++
const char LOG_AS[] PROGMEM = "Log as ";

void logError() {
    Log.error   (  "%S Error   with binary values             : %b, %B"CR  , PSTRPTR(LOG_AS), 23  , 345808);
}
```

### Examples

```c++
    Log.fatal     (F("Log as Fatal   with string value from Flash   : %s"CR    ) , "value"     );
    Log.error   (  "Log as Error   with binary values             : %b, %B"CR    , 23  , 345808);
    Log.warning   (F("Log as Warning with integer values from Flash : %d, %d"CR) , 34  , 799870);
    Log.notice    (  "Log as Notice  with hexadecimal values        : %x, %X"CR  , 21  , 348972);
    Log.trace     (  "Log as Trace   with Flash string              : %S"CR      , F("value")  );
    Log.verbose (F("Log as Verbose with bool value from Flash     : %t, %T"CR  ) , true, false );
```

### Disable library

(if your code is completely tested) all logging code can be compiled out. Do this by uncommenting

```c++
#define DISABLE_LOGGING
```

in `Logging.h`. This may significantly reduce your project size.

## Credit

Based on [this library](https://github.com/thijse/Arduino-Log) by [Thijs Elenbaas](https://github.com/thijse)

I forked his project since it seemed to be unmaintained and I needed to add features and to use this library for a project.

## Copyright

ArduinoLog is provided under MIT License.
