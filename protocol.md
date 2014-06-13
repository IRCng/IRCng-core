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
	"nick":"<nickname>",
	"ident":"<identname>",
	"ident-status":<number describing how identd responded>,
	"hostname":<visible hostname of the user>,
	etc
}

The source field describes the source of a message.
"realhostname" may be used if the client receiving the message is
allowed to see this information.

PROTOCTL message
====
Old protocol: PROTOCTL <version> <extensions> <message>
New protocol: 
{
	"cmd":"PROTOCTL",
	"protover":<protocol version as number>,
	"extensions":{
		"extname":"parameter", <etc>
	}
}

PROTOCTL is used to upgrade the protocol from IRCv2.8-compatibility to IRCng.
PROTOCTL is NOT used to downgrade the protocol; instead, downgrades are done automatically if an old protocol message is sent.

PRIVMSG/NOTICE messages
====
Old protocol: PRIVMSG <target name> :<message>
              NOTICE <target name> :<message>
New protocol:
{
	"cmd":"PRIVMSG",
	"dest":{
		"nick":"<target name>"
	},
	"message": "multi line message goes here, with all the escaping goodehs",
	"data": freeform object/array/string
}
or s/PRIVMSG/NOTICE/

PRIVMSG and NOTICE can contain freeform data, like CTCPs, and/or they can contain messages.
