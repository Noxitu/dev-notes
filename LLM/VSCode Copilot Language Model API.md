
# Extension setup

Extension needs to report it provides a langugage model provider in its `package.json`:

```json
"contributes": {
    "languageModelChatProviders": [
        {
            "vendor": "my-provider-name",
            "displayName": "My Provider Display Name"
        }
    ]
}
```

Upon activation extension needs to register such provider:

```js
const provider = {
    provideLanguageModelChatInformation: (options, token) => {
        return [];
    },

    provideTokenCount: async (model, text, cancellationToken) => {
        return ...;
    },

    provideLanguageModelChatResponse: async (model, messages, options, progress, cancellationToken) => {
    },
};

context.subscriptions.push(
    vscode.lm.registerLanguageModelChatProvider("my-provider-name", provider)
);
```

Example model information looks as follows:

```js
{
    id: "ai/smollm2,
    name: "[Local] ai/smollm2,
    vendor: "my-provider-name",
    version: "local",
    family: family || "unknown",
    tooltip: tooltip || "Local model.",
    tokens: 100000,
    capabilities: {
        toolCalling: true,
        vision: false
    }
}
```

# Messages

Message contains `role` field that stores one of: `vscode.LanguageModelChatMessageRole.User` (1), `vscode.LanguageModelChatMessageRole.Assistant` (2) or `vscode.LanguageModelChatMessageRole.System` (3).

Message contains `content` field which contains array of parts.

## LanguageModelTextPart

Contains `value` with text. Constructor arguments: `(value)`.

## LanguageModelThinkingPart

Contains `value` with text. Constructor arguments: `(value)`.

## LanguageModelToolCallPart

Contains `callId` with unique token of a call, `name` of a function to call and `input` with object containing function arguments.

Constructor arguments: `(callId, name, input)`.

## LanguageModelToolResultPart

Contains `callId` with a token matching tool call, and `content` with the result. Content is array of parts, and in example runs consisted of two parts: first `LanguageModelTextPart` with full result, and cache control part.

## Cache control part

Doesn't seem to have a type. Can be recognized by field `mimeType` equal to `"cache_control"`.
