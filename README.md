
# cleanmcp

MCP Server with Streamable HTTP Transport built with LeanMCP SDK

## Quick Start

```bash
# Install dependencies
npm install

# Start development server (hot reload)
npm run dev

# Build for production
npm run build

# Run production server
npm start
```

## Project Structure

```
cleanmcp/
├── main.ts              # Server entry point
├── mcp/                 # Services directory (auto-discovered)
│   └── example/
│       └── index.ts     # Example service
├── .env                 # Environment variables
└── package.json
```

## Adding New Services

Create a new service directory in `mcp/`:

```typescript
// mcp/myservice/index.ts
import { Tool, SchemaConstraint } from "@leanmcp/core";

// Define input schema
class MyToolInput {
  @SchemaConstraint({ 
    description: "Message to process",
    minLength: 1
  })
  message!: string;
}

export class MyService {
  @Tool({ 
    description: "My awesome tool",
    inputClass: MyToolInput
  })
  async myTool(input: MyToolInput) {
    return {
      content: [{
        type: "text",
        text: `You said: ${input.message}`
      }]
    };
  }
}
```

Services are automatically discovered and registered - no need to modify `main.ts`!

## Features

- **Zero-config auto-discovery** - Services automatically registered from `./mcp` directory
- **Type-safe decorators** - `@Tool`, `@Prompt`, `@Resource` with full TypeScript support
- **Schema validation** - Automatic input validation with `@SchemaConstraint`
- **HTTP transport** - Production-ready HTTP server with session management
- **Hot reload** - Development mode with automatic restart on file changes

## Deploy to LeanMCP Cloud

Deploy your MCP server to LeanMCP's global edge network with a single command:

```bash
# Login to LeanMCP (first time only)
leanmcp login

# Deploy your server
leanmcp deploy .
```

After deployment, your server will be available at:
- **URL**: `https://your-project.leanmcp.app`
- **Health**: `https://your-project.leanmcp.app/health`
- **MCP Endpoint**: `https://your-project.leanmcp.app/mcp`

Manage your deployments at [ship.leanmcp.com](https://ship.leanmcp.com)

## Testing with MCP Inspector

```bash
# Test locally
npx @modelcontextprotocol/inspector http://localhost:3001/mcp

# Test deployed server
npx @modelcontextprotocol/inspector https://your-project.leanmcp.app/mcp
```

## License

MIT
