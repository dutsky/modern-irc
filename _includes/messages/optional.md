These messages are not required for a server implementation to work, but SHOULD be implemented. If a command is not implemented, it MUST return the {% numeric ERR_UNKNOWNCOMMAND %} numeric.

### AWAY message

         Command: AWAY
      Parameters: [ <text> ]

The `AWAY` command lets clients indicate that their user is away.
If this command is sent with a parameter (the 'away message') then the user is set to be away. If this command is sent with no parameters, the user is no longer away.

The server acknowledges the change in away status by returning the {% numeric RPL_NOWAWAY %} and {% numeric RPL_UNAWAY %} numerics.
If the [IRCv3 `away-notify` capability](https://ircv3.net/specs/extensions/away-notify.html) has been requested by a client, the server MAY also send that client `AWAY` messages to tell them how the away status of other users has changed.

Servers SHOULD notify clients when a user they're interacting with is away when relevant, including sending these numerics:

1. {% numeric RPL_AWAY %}, with the away message, when a {% command PRIVMSG %} command is directed at the away user (not to a channel they are on).
2. {% numeric RPL_AWAY %}, with the away message, in replies to {% command WHOIS %} messages.
3. In the {% numeric RPL_USERHOST %} numeric, as the `+` or `-` character.

Numeric Replies:

* {% numeric RPL_UNAWAY %}
* {% numeric RPL_NOWAWAY %}

### USERHOST message

         Command: USERHOST
      Parameters: <nickname>{ <nickname>}

The `USERHOST` command is used to return information about users with the given nicknames. The `USERHOST` command takes up to five nicknames, each a separate parameters. The nicknames are returned in {% numeric RPL_USERHOST %} numerics.

Numeric Replies:

* {% numeric ERR_NEEDMOREPARAMS %}
* {% numeric RPL_USERHOST %}

Command Examples:

      USERHOST Wiz Michael Marty p    ;USERHOST request for information on
                                      nicks "Wiz", "Michael", "Marty" and "p"

Reply Examples:

      :ircd.stealth.net 302 yournick :syrk=+syrk@millennium.stealth.net
                                      ; Reply for user syrk

### WALLOPS message

         Command: WALLOPS
      Parameters: <text>

The WALLOPS command is used to send a message to all currently connected users who have set the 'w' user mode for themselves.
The `<text>` SHOULD be non-empty.

Servers MAY echo WALLOPS messages to their sender even if they don't have the 'w' user mode.

Servers MAY send WALLOPS only to operators.

Servers may generate it themselves, and MAY allow operators to send them.

Numeric replies:

* {% numeric ERR_NEEDMOREPARAMS %}
* {% numeric ERR_NOPRIVILEGES %}
* {% numeric ERR_NOPRIVS %}

Examples:

     :csd.bu.edu WALLOPS :Connect '*.uiuc.edu 6667' from Joshua
                                     ;WALLOPS message from csd.bu.edu announcing
                                     a CONNECT message it received and acted
                                     upon from Joshua.

