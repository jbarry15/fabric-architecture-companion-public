# Fabric MCP Server (local) - development-focused Fabric MCP

Microsoft Learn: [Get started with Fabric MCP Server (local)](https://learn.microsoft.com/en-us/rest/api/fabric/articles/mcp-servers/pro-dev-local/get-started-local)

Source repo: [microsoft/mcp - Fabric.Mcp.Server](https://github.com/microsoft/mcp/tree/main/servers/Fabric.Mcp.Server)

Package: `@microsoft/fabric-mcp`

## What it is

Fabric MCP Server (local) is the open-source Microsoft Fabric MCP server that runs as a local subprocess for development workflows.

Use it when an architecture or delivery scenario needs Fabric API documentation, item schemas, best-practice help, OneLake file operations, item creation, local file access, or offline development assistance.

## What it covers

| Area | Why it matters |
|------|----------------|
| Local subprocess | Runs on the developer machine from a VS Code extension, npm/npx, or source build. |
| Fabric API specs | Lets agents inspect API operations, schemas, and sample request bodies. |
| OneLake file operations | Enables local agent workflows that read or write OneLake files where authorized. |
| Item creation | Supports development tasks for creating Fabric items through local tooling. |
| Offline docs and best practices | Provides guidance even when a live Fabric connection isn't available. |

## Best-fit scenarios

- Exploring Fabric REST API operations and item schemas.
- Building or validating item definition files.
- Working with local project files and Fabric API docs in the same agent workflow.
- Downloading or managing OneLake files during development.
- Creating repeatable prompt/runbook patterns for Fabric development tasks.

## Watch-outs

- Local MCP servers can expose local files and configuration to the MCP host; run from a dedicated project folder.
- Keep secrets, certificates, tokens, and tenant-specific MCP configuration out of committed repo content.
- Validate Node.js, .NET SDK, VS Code extension, or package-version prerequisites based on the chosen install method.
- Treat generated item definitions and file changes like human-authored code: review diffs and promote through Git or deployment pipelines where supported.
