Great! Let's **package your custom Language Server Protocol (LSP)** into a **publishable Visual Studio Code extension**. Here's a full guide from project setup to packaging, so you can eventually publish it to the [VS Code Marketplace](https://marketplace.visualstudio.com/).

---

# 📦 Package a Custom LSP as a VS Code Extension

---

## 🛠️ 1. Project Structure

Let’s assume you already have this:

```
my-lsp-extension/
├── server.js                # Your custom language server
├── package.json             # Server package.json
└── client/                  # VS Code extension project
    ├── extension.js
    ├── package.json         # Extension manifest
    └── ... (language files, icon, etc.)
```

---

## 📁 2. Clean Up and Organize

You want your publishable VS Code extension to be **self-contained**.

### Move files:

* Place the language server code in a subfolder: `server/`
* Update `client/` to include everything for the VS Code extension

New structure:

```
my-lsp-extension/
├── server/
│   ├── server.js
│   └── package.json
├── client/
│   ├── extension.js
│   ├── package.json   ← VS Code extension manifest
│   └── ... (language/syntax files)
```

---

## ✏️ 3. Update `client/package.json`

Add/modify this in your extension manifest:

```jsonc
{
  "name": "my-lsp-extension",
  "displayName": "My LSP",
  "description": "A minimal Language Server Protocol extension",
  "version": "0.0.1",
  "publisher": "your-publisher-name",   // Required for publishing
  "engines": {
    "vscode": "^1.80.0"
  },
  "activationEvents": [
    "onLanguage:plaintext"
  ],
  "main": "./extension.js",
  "contributes": {
    "languages": [
      {
        "id": "plaintext",
        "aliases": ["Plain Text"],
        "extensions": [".txt"]
      }
    ]
  },
  "scripts": {
    "postinstall": "cd ../server && npm install"
  },
  "dependencies": {
    "vscode-languageclient": "^9.0.0"
  }
}
```

---

## 🔗 4. Update the Client Code to Reference the Server

In `client/extension.js`, use a **relative path** to launch the server:

```js
const path = require('path');
const { LanguageClient, TransportKind } = require('vscode-languageclient/node');

let client;

function activate(context) {
  const serverModule = path.join(__dirname, '..', 'server', 'server.js');

  const serverOptions = {
    run: { module: serverModule, transport: TransportKind.stdio },
    debug: { module: serverModule, transport: TransportKind.stdio }
  };

  const clientOptions = {
    documentSelector: [{ scheme: 'file', language: 'plaintext' }]
  };

  client = new LanguageClient('my-lsp', 'My LSP', serverOptions, clientOptions);
  client.start();
}

function deactivate() {
  return client?.stop();
}

module.exports = { activate, deactivate };
```

---

## 📦 5. Install Dependencies

From the root:

```bash
cd server
npm install vscode-languageserver
cd ../client
npm install
```

---

## 🧪 6. Test the Extension

1. Run `code .` in the `client/` folder.
2. Press `F5` to launch the extension in a new window.
3. Create a `.txt` file and check for `"HelloWorld"` autocomplete.

---

## 📤 7. Package the Extension

### Step 1: Install `vsce`

```bash
npm install -g vsce
```

### Step 2: From `client/`, package the extension:

```bash
vsce package
```

This will produce something like:

```
my-lsp-extension-0.0.1.vsix
```

---

## 🚀 8. (Optional) Publish to Marketplace

### Requirements:

* Sign up on [https://aka.ms/vsmarketplace](https://aka.ms/vsmarketplace)
* Create a **publisher** and a **personal access token (PAT)**

### Publish steps:

```bash
vsce login <your-publisher-name>
vsce publish
```

> ⚠️ Make sure the `publisher` field in your `package.json` matches your VSCode publisher ID.

---

## 🧠 Recap

| Step           | Purpose                                     |
| -------------- | ------------------------------------------- |
| `server/`      | Contains your language server (`server.js`) |
| `client/`      | VS Code extension files                     |
| `vsce package` | Packages your extension                     |
| `vsce publish` | (Optional) Uploads to the marketplace       |

---

Would you like:

* A TypeScript version of the same setup?
* Diagnostics or hover support added?
* A template repo to clone and modify?

Let me know!
