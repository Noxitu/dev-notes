## Hosts Aliases

Add to `.ssh/config`:

    Host example_alias
        HostName server.example.com
        User example_user

Use `ssh example_alias` instead `ssh example_user@server.example.com`.

## Escape Sequences

    Supported escape sequences:
    ~.   - terminate connection (and any multiplexed sessions)
    ~B   - send a BREAK to the remote system
    ~C   - open a command line
    ~R   - request rekey
    ~V/v - decrease/increase verbosity (LogLevel)
    ~^Z  - suspend ssh
    ~#   - list forwarded connections
    ~&   - background ssh (when waiting for connections to terminate)
    ~?   - this message
    ~~   - send the escape character by typing it twice
    (Note that escapes are only recognized immediately after newline.)

## Alive Check

Add to config:

    ServerAliveInterval 5
    ServerAliveCountMax 1

SSH will send a message every `ServerAliveInterval` seconds after not receiving data. After `ServerAliveCountMax` messages are sent without a message from the server connection will be terminated.