# Anki MCP Server

A Model Context Protocol (MCP) server that enables LLMs to interact with Anki flashcard software through AnkiConnect.

![Anki Icon](./assets/icon.png)

## Features

### Tools
- `list_decks` - List all available Anki decks
- `create_deck` - Create a new Anki deck
- `create_note` - Create a new note (Basic or Cloze)
- `batch_create_notes` - Create multiple notes at once
- `search_notes` - Search for notes using Anki query syntax
- `get_note_info` - Get detailed information about a note
- `update_note` - Update an existing note
- `delete_note` - Delete a note
- `list_note_types` - List all available note types
- `create_note_type` - Create a new note type
- `get_note_type_info` - Get detailed structure of a note type
- `create_atom` - Create a new atom document in Payload CMS

### Resources
- `anki://decks/all` - Complete list of available decks
- `anki://note-types/all` - List of all available note types
- `anki://note-types/all-with-schemas` - Detailed structure information for all note types
- `anki://note-types/{modelName}` - Detailed structure information for a specific note type

## Prerequisites

1. [Anki](https://apps.ankiweb.net/) installed on your system
2. [AnkiConnect](https://ankiweb.net/shared/info/2055492159) add-on installed in Anki

## Installation & Configuration

### Option 1: Install from GitHub (Recommended)

Install directly from this GitHub repository:

#### Usage with Claude Desktop

Add the server to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "anki-atom": {
      "command": "npx",
      "args": ["--yes", "github:bathrobe/anki-atom-mcp"]
    }
  }
}
```

#### Configuration for Cline

Add the server to your Cline MCP settings file `cline_mcp_settings.json`:

```json
{
  "mcpServers": {
    "anki-atom": {
      "command": "npx",
      "args": ["--yes", "github:bathrobe/anki-atom-mcp"]
    }
  }
}
```

### Option 2: Local Development Installation

For local development or testing:

1. Clone this repository:
```bash
git clone https://github.com/bathrobe/anki-atom-mcp.git
cd anki-atom-mcp
```

2. Install dependencies and build:
```bash
npm install
npm run build
```

3. Configure with absolute path:

#### Claude Desktop Configuration
```json
{
  "mcpServers": {
    "anki-atom": {
      "command": "node",
      "args": ["/absolute/path/to/anki-atom-mcp/dist/index.js"]
    }
  }
}
```

#### Cline Configuration  
```json
{
  "mcpServers": {
    "anki-atom": {
      "command": "node",
      "args": ["/absolute/path/to/anki-atom-mcp/dist/index.js"]
    }
  }
}
```

## Development

### Setup

1. Install dependencies:
```bash
npm install
```

2. Build the server:
```bash
npm run build
```

3. For development with auto-rebuild:
```bash
npm run watch
```

### Testing

Run the test suite:
```bash
npm test
```

This executes tests for:
- Server initialization
- AnkiConnect communication
- Note operations (create/read/update/delete)
- Deck management
- Error handling

### Debugging

Since MCP servers communicate over stdio, we recommend using the [MCP Inspector](https://github.com/modelcontextprotocol/inspector):

```bash
npm run inspector
```

This provides a browser-based interface for:
- Monitoring MCP messages
- Testing tool invocations
- Viewing server logs
- Debugging communication issues

## Example Usage

1. Create a new deck:
```
Create a new Anki deck called "Programming"
```

2. Add a basic card:
```
Create an Anki card in the "Programming" deck with:
Front: What is a closure in JavaScript?
Back: A closure is the combination of a function and the lexical environment within which that function was declared.
```

3. Add a cloze deletion card:
```
Create a cloze card in the "Programming" deck with:
Text: In JavaScript, {{c1::const}} declares a block-scoped variable that cannot be {{c2::reassigned}}.
```

4. Create an atom in Payload CMS:
```
Create an atom document with title "My Learning Note" and content "JavaScript closures are powerful" in my Payload CMS at https://my-payload.com
```



## Contributing

1. Fork the repository
2. Create your feature branch
3. Run tests: `npm test`
4. Submit a pull request

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=nailuoGG/anki-mcp-server&type=Date)](https://star-history.com/#nailuoGG/anki-mcp-server&Date)

## Credits

Icon courtesy of [macOS Icons](https://macosicons.com/#/?icon=mWDBpVXqbc)

## License

MIT License - see LICENSE file for details
