# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Build and Development
- `npm run build` - Compile TypeScript files to JavaScript in dist/ directory
- `npm start` - Run the compiled server from dist/index.js
- `npm run auth` - Run OAuth authentication flow

### Running Tests and Evaluations
- `OPENAI_API_KEY=your-key npx mcp-eval src/evals/evals.ts src/index.ts` - Run MCP evaluations (no rebuild needed between tests)

### Package Management
- `npm install` - Install dependencies
- `npm run prepare` - Runs automatically after npm install (builds the project)
- `npm run prepublishOnly` - Runs automatically before npm publish

## Architecture Overview

This is a Model Context Protocol (MCP) server for Gmail integration, enabling AI assistants to manage Gmail through natural language. The architecture consists of:

### Core Components
1. **MCP Server** (`src/index.ts`): Entry point that runs as either an MCP server or authentication mode
   - Uses StdioServerTransport for AI assistant communication
   - Implements 13 Gmail tools for comprehensive email management
   - Handles OAuth2 authentication flow with browser-based consent

2. **Label Manager** (`src/label-manager.ts`): Dedicated module for Gmail label operations
   - Create, update, delete, and list labels
   - Get-or-create pattern for label management
   - Batch operations for efficient label modifications

3. **Utilities** (`src/utl.ts`): Helper functions for email processing
   - Email address validation
   - MIME message construction (plain text, HTML, multipart)
   - RFC 2047 encoding for international characters
   - Recursive MIME parsing for complex email structures

### Authentication System
- OAuth2 flow with offline access for persistent authentication
- Credentials stored in `~/.gmail-mcp/` directory
- Supports both Desktop and Web application OAuth credentials
- Local HTTP server on port 3000 for OAuth callback handling

### Gmail API Integration
- Uses official Google APIs client library
- Full Gmail API access (scope: `gmail.modify`)
- Comprehensive error handling and validation
- Batch processing with automatic chunking and retry logic

### Key Design Patterns
- Zod schemas for runtime validation, converted to JSON Schema for MCP
- TypeScript for type safety throughout
- Modular architecture with clear separation of concerns
- Generic batch processing function with configurable batch sizes
- Graceful error recovery for batch operations