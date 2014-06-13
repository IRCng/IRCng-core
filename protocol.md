IRCng protocol version 1 specification
====

IRCng is a highly complicated protocol.
Its line format is JSON, which means we do not need to provide formats,
only a specification for objects/arrays.

JSON objects must be printable on one, 19,998 character line.
This means not using pretty print and using newline escape.

The "source": subobject is ALWAYS a description of the source of a message, or from clients it doesnt exist.

Source subobject
====

"source": {
	"nick":"&lt;nickname&gt;",<br />
	"ident":"&lt;identname&gt;",<br />
	"ident-status":&lt;number describing how identd responded&gt;,<br />
	"hostname":&lt;visible hostname of the user&gt;,<br />
	etc<br />
}<br />

The source field describes the source of a message.
"realhostname" may be used if the client receiving the message is
allowed to see this information.

PROTOCTL message
====
Old protocol: PROTOCTL &lt;version&gt; &lt;extensions&gt; &lt;message&gt;<br />
New protocol: <br />
{<br />
	"cmd":"PROTOCTL",<br />
	"protover":&lt;protocol version as number&gt;,<br />
	"extensions":{<br />
		"extname":"parameter", &lt;etc&gt;<br />
	}<br />
}<br />

PROTOCTL is used to upgrade the protocol from IRCv2.8-compatibility to IRCng.
PROTOCTL is NOT used to downgrade the protocol; instead, downgrades are done automatically if an old protocol message is sent.

PRIVMSG/NOTICE messages
====
Old protocol: PRIVMSG &lt;target name&gt; :&lt;message&gt;<br />
              NOTICE &lt;target name&gt; :&lt;message&gt;<br />
New protocol:<br />
{<br />
	"cmd":"PRIVMSG",<br />
	"dest":{<br />
		"nick":"&lt;target name&gt;"<br />
	},<br />
	"message": "multi line message goes here, with all the escaping goodehs",<br />
	"data": freeform object/array/string<br />
}<br />
or s/PRIVMSG/NOTICE/

PRIVMSG and NOTICE can contain freeform data, like CTCPs, and/or they can contain messages.
