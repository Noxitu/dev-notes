# Transport

MCP is a JSON RPC server. It is transport independent, though few transport methods are commonly used:

1. Standard io - in this setup MCP is spawned as child process, reading requests from stdin and writing responses to stdout. In this mode requests and responses are delimited by new lines.
2. Dual-endpoint Server Sent Events - in this setup client first opens a SSE connection for reading answers, which provides it with endpoint to send any RPC requests.
3. Streamable HTTP, which might respond directly using HTTP response; it might decide to upgrade to stream for longer waits.

SSE is the default behavior to make sure no proxies along the way will mess up the communication - for example by introducing timeouts for longer calls.

# JSON RPC

JSON RPC works by interchanging objects. Client requests look as follows:

```json
{
  "jsonrpc": "2.0",
  "method": "my_function",
  "params": [42, 23],
  "id": 1
}
```

`params` is optional; it can be an object rather than an array of arguments.

`id` can be either string or integer. `id` can be also ommited, turning a request into "notification". Server won't send responses to notifications.

Client request can also be an array of requests.

An example server response for such request might look as follows:

```json
{
  "jsonrpc": "2.0",
  "result": 65,
  "id": 1
}
```

or if error occured:

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Some error message"
  },
  "id": 1
}
```

While not part of JSON RPC, MCP servers can be the ones sending requests. Most of them will be notifications, but if they are full requests (like ping) client must send a response.

# Methods

|---|---|
| Method | Description |
| `initialize` | Handshake initiated by client. |
| `notifications/initialized` | Acknowledges server response to handshake. |
| `ping` | |
| `notifications/cancelled` | Can be send by both client and server. |
| `tools/list` | |
| `tools/call` | |
| `notifications/tools/list_changed` | |
| `resources/list` | |
| `resources/read` | |
| `resources/templates/list` | |
| `resources/subscribe` | |
| `resources/unsubscribe` | |
| `notifications/resources/updated` | |
| `prompts/list` | |
| `prompts/get` | |
| notifications/prompts/list_changed` | |
| `sampling/createMessage` | MCP server asking for LLM response to a message. |
| `elicitation/create` | Request user of the client for confirmation of an action. |
| `roots/list` | |
| `logging/setLevel` | |
| `notifications/message` | Log message. |
| `progress` | |
