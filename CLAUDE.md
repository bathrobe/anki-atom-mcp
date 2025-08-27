# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an MCP (Model Context Protocol) server that enables LLMs to interact with Anki flashcard software through AnkiConnect. The server exposes both tools (for actions) and resources (for data access) following the MCP specification.

## Development Commands

**Build and Development:**
```bash
npm run build          # Build for production
npm run watch          # Development with auto-rebuild
npm run prebuild       # Generate version file (runs automatically before build)
```

**Testing:**
```bash
npm test              # Run test suite
npm run test:watch    # Run tests in watch mode
npm run test:coverage # Run tests with coverage report
```

**Code Quality:**
```bash
npm run format        # Format code with Biome
npm run lint          # Lint code with Biome  
npm run check         # Run Biome check with auto-fixes
```

**Debugging:**
```bash
npm run inspector     # Launch MCP Inspector for debugging MCP communication
```

## Architecture

**Core Components:**
- `AnkiMcpServer` (ankiMcpServer.ts) - Main MCP server that handles protocol communication
- `McpToolHandler` (mcpTools.ts) - Manages all MCP tools (actions like create_note, list_decks)
- `McpResourceHandler` (mcpResource.ts) - Manages MCP resources (data access like anki://decks/all)
- `AnkiClient` (utils.ts) - Anti-corruption layer wrapping yanki-connect library

**Data Flow:**
1. MCP requests come through stdio transport to AnkiMcpServer
2. Server routes tool calls to McpToolHandler and resource requests to McpResourceHandler
3. Both handlers use AnkiClient to communicate with Anki via AnkiConnect
4. AnkiClient provides error handling, retry logic, and normalized interfaces

**Key Patterns:**
- All handlers validate Anki connection before processing requests
- Tools return consistent JSON response format with success/error states
- Resources use caching (5min TTL) to avoid repeated Anki API calls
- Dynamic tool generation creates model-specific note creation tools

## Testing Strategy

Tests are located in `**/src/**/*.test.ts` and use Jest with ts-jest preset. The setup supports ESM modules and runs tests sequentially (`--runInBand`) to avoid conflicts with Anki's single-instance nature.

## MCP Protocol Implementation

**Tools vs Resources:**
- Tools = Actions (create_note, delete_note, search_notes)
- Resources = Data access (anki://decks/all, anki://note-types/{modelName})

**URI Patterns:**
- `anki://decks/all` - All available decks
- `anki://note-types/all` - List of note types
- `anki://note-types/all-with-schemas` - Note types with detailed field/template info
- `anki://note-types/{modelName}` - Specific note type schema

## Dependencies

**Core:**
- `@modelcontextprotocol/sdk` - MCP protocol implementation
- `yanki-connect` - Anki communication wrapper
- `axios` - HTTP client (used by yanki-connect)

**Build/Dev:**
- `tsup` - TypeScript bundler (outputs both ESM/CJS)
- `biome` - Formatter and linter
- `jest` + `ts-jest` - Testing framework

## Error Handling

Three custom error types in utils.ts:
- `AnkiConnectionError` - Anki not running or AnkiConnect unavailable
- `AnkiTimeoutError` - Communication timeouts
- `AnkiApiError` - Anki-specific API errors (e.g., collection unavailable)

All errors are converted to MCP errors with appropriate error codes before being returned to clients.