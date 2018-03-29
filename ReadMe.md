## Test RegEx
The lnav format defintion for parsing gstreamer logs. The parser regex tested with regex.com.


Note: [regex.com](http://www.regexe.com/) expression will use only single \

http://www.regexe.com/

Sample gst log line to parse
0:00:00.000644666   990   0x210400 INFO                GST_INIT gst.c:511:init_pre: Initializing GStreamer Core Library version 1.8

RegEx
(?<timestamp>\d{1,2}:\d{2}:\d{2}.\d{9})\s+(?<val1>\d{3})\s+(?<val2>\w+)\s+(?<alertlevel>\w+)\s+(?<logsection>\w+)\s+(?<fileinfo>\w+.\w:\d+:\w+:)\s+(?<body>.*)$


_Parsed Output_

Result details:
1. 	0:00:00.000644666 990 0x210400 INFO GST_INIT gst.c:511:init_pre: Initializing GStreamer Core Library version 1.8


	 	 
 	1.1. Group: 0:00:00.000644666
 	1.2. Group: 990
 	1.3. Group: 0x210400
 	1.4. Group: INFO
 	1.5. Group: GST_INIT
 	1.6. Group: gst.c:511:init_pre:
 	1.7. Group: Initializing GStreamer Core Library version 1.8


----
Also ensure to check with Notepad++ if the logs are copied from console to remove the unwanted ESC charcaters.

What should be removed is not simply the ASCII escape character, but the entire escape sequence, which terminates the the "m".

In Notepad++, you can substitute regular expressions.  this expression will search for escape sequences for colors:

\x1b\[[0-9;]*m
Tick off Search Mode = Regular expression, and click Find Next.

Further reading:
This expression will search for non-ascii values:

[^\x00-\x7F]+
Tick off 'Search Mode = Regular expression', and click Find Next.

Source: Regex any ascii character
Notepad++, How to remove all non ascii characters with regex?


_Update in ~/.lnav/formats/gstlogformat_
-- Note : lnav formats use \\
```

{
 	"gst" : {
 		"title"			: "gstreamer log format",
 		"description"	: "gstreamer log parser",
 		"url"			: "",
 		"regex" : {
 			"gst-1"	: {
 				"pattern" :"^(?<timestamp>\\d{1,2}:\\d{2}:\\d{2}.\\d{9})\\s+(?<val1>\\d{3})\\s+(?<val2>\\w+)\\s+(?<alert_level>\\w+)\\s+(?<logsection>\\w+)\\s+(?<fileinfo>\\w+.\\w:\\d+:\\w+:)\\s*(?<body>.*)$"
			}
 		},
 		
		"timestamp-format" : [
			"%k:%M:%S.%N" 
			 
		],
		
		"level-field" : "alert_level",
 		"level" : {
 			"info"	: "STATUS",
 			"error" : "ERROR",
 			"warning" : "WARN",
 			"debug" : "DEBUG",
 			"info" : "INFO"
 		},
 		"value" : {
						
			"alert_level"	: { "kind" : "string", "identifier" : true },
 			"val1"		: { "kind" : "string", "identifier" : false },
 			"val2"	        : { "kind" : "string", "identifier" : false },
 			"logsection"	: { "kind" : "string", "identifier" : true },
			"fileinfo"	: { "kind" : "string", "identifier" : false },
			"body"          : {"kind" : "string"}
 			
 		},
 		"sample" : [
 			{
 				"line" : "0:00:00.000644666   990   0x210400 INFO                GST_INIT gst.c:511:init_pre: Initializing GStreamer Core Library version 1.8.1"
 			},
 			{
 				"line"  :"0:00:00.004471666   990   0x210400 INFO                GST_INIT gstmessage.c:119:_priv_gst_message_initialize: init messages"
 			},
			{
 				"line"  :"0:00:00.012362333   990   0x210400 DEBUG               GST_POLL gstpoll.c:726:gst_poll_add_fd_unlocked: 0x224c20: fd (fd:4, idx:0) "
 			}
 		]
 	}
 }
```
-------
Some help from Tim .....

https://github.com/tstack/lnav/issues/500

A collection of format defintions : One included for strace aswell : https://github.com/PaulWay/lnav-formats

