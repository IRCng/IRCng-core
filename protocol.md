IRCng protocol version 1 specification
====

IRCng is a highly complicated protocol.
Its line format is JSON, which means we do not need to provide formats,
only a specification for objects/arrays.

JSON objects must be printable on one, 19,998 character line.
This means not using pretty print and using newline escape.

Let's go.

PROTOCTL message
====
Old protocol: PROTOCTL <version> <extensions> <message>
New protocol: {
	"cmd":"PROTOCTL",
	"protover":<protocol version as number>,
	"extensions":{
		"extname":"parameter", <etc>
	}
}

PROTOCTL is used to upgrade the protocol from IRCv2.8-compatibility to IRCng.
PROTOCTL is NOT used to downgrade the protocol; instead, downgrades are done automatically if an old protocol message is sent.
