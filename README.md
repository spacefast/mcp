# Spacefast MCP

**Build and share your AI work.**

Turn an idea into a website, dashboard, report, or small web tool you can share. Build it in your conversation, or bring something you already made, and publish it with Spacefast.

Keep improving it through chat. Change the content or design, share a preview, choose who can open it, connect your own domain, or restore an earlier version.

Connect your Spacefast account to publish and manage your team’s Spaces.

Spacefast is by [Automattic](https://automattic.com/). This repository contains the connection examples, documentation, and MCP Registry entry for the hosted Spacefast server. The installable plugins and their skills live in [spacefast/plugins](https://github.com/spacefast/plugins).

## Connect

Endpoint: **`https://mcp.spacefast.com`**

Transport: **Streamable HTTP**. Use a client that supports remote MCP servers and browser OAuth.

### GitHub Copilot and other compatible clients

Add this to `.mcp.json` in your project. Merge the `spacefast` entry into any existing `mcpServers` object.

```json
{
  "mcpServers": {
    "spacefast": {
      "type": "http",
      "url": "https://mcp.spacefast.com"
    }
  }
}
```

The same configuration is available in [`.mcp.json`](.mcp.json). For a user-wide Copilot configuration, use `~/.copilot/mcp-config.json`.

VS Code also supports `.vscode/mcp.json`, which uses `servers` as its top-level key:

```json
{
  "servers": {
    "spacefast": {
      "type": "http",
      "url": "https://mcp.spacefast.com"
    }
  }
}
```

See the [VS Code example](examples/vscode-mcp.json) and [VS Code MCP setup guide](https://code.visualstudio.com/docs/agent-customization/mcp-servers). Start the server from your client's MCP settings and complete the Spacefast sign-in flow. Enable its tools in your agent session.

### Claude Code

For the MCP connection:

```sh
claude mcp add --transport http spacefast https://mcp.spacefast.com
```

Run `/mcp` in Claude Code and follow the browser sign-in flow.

To install the Spacefast plugin with its skills, use:

```sh
claude plugin marketplace add spacefast/plugins
claude plugin install spacefast@spacefast
```

See the [Spacefast Claude Code setup guide](https://spacefast.com/docs/setup/claude-code/) for the full plugin setup.

### goose

In goose Desktop, open **Extensions → Add custom extension**. Choose **Streamable HTTP**, name it **Spacefast**, and set the URL to `https://mcp.spacefast.com`. Add the extension and complete the Spacefast sign-in flow in your browser. No API key or custom headers are needed.

For the CLI, run `goose configure`, select **Add Extension → Remote Extension (Streamable HTTP)**, and use the same name and URL.

You can also merge the `spacefast` entry from [the goose configuration example](examples/goose-config.yaml) into `extensions` in `~/.config/goose/config.yaml`. Keep your existing extensions and other settings.

To add all seven Spacefast skills to your project, run this from the project directory:

```sh
DISABLE_TELEMETRY=1 npx -y skills@1.5.23 add https://github.com/spacefast/plugins/tree/main/skills --agent goose --skill '*' --yes
```

Run `goose skills list` from that directory to check the installation. This installs skills such as `build-website` and `edit-space`; configure the MCP connection using the steps above. goose 1.54.0's plugin importer does not accept remote MCP declarations, so use this skills installer for that release. See the official [extension setup](https://goose-docs.ai/docs/getting-started/using-extensions/) and [skills guide](https://goose-docs.ai/docs/guides/context-engineering/using-skills/).

Spacefast is published as [`io.github.spacefast/mcp` in the official MCP Registry](https://registry.modelcontextprotocol.io/v0.1/servers/io.github.spacefast%2Fmcp/versions/latest). goose is [moving its directory to that registry](https://github.com/aaif-goose/goose/discussions/10830); appearing in goose's directory depends on that migration. You can connect directly now.

## Authentication

The hosted MCP server requires a Spacefast account and OAuth before tools can run. The client discovers the authorization server, registers through OAuth Dynamic Client Registration, and signs you in with an authorization code flow using PKCE (`S256`). You do not need to create an API key or configure a client secret.

OAuth resource metadata is available at [`/.well-known/oauth-protected-resource`](https://mcp.spacefast.com/.well-known/oauth-protected-resource). Your MCP client handles the access token. Keep credentials out of shared configuration files.

## What you can do

- Publish new inline files from your conversation and check the result with `publish` and `operation_status`.
- Edit a Space's saved source, deploy changes, manage sharing and domains, or restore a version through `execute`.
- Inspect a Space and choose a destination with `show_space` and `choose_space`.
- Find the team's connected services and their tools with `search`.
- Continue a task that paused for user input with `resume_execution`.

The hosted server works with inline files and Spacefast's stored source. For files on your computer, use the [Spacefast CLI](https://spacefast.com/docs/cli/). Interactive views depend on your client's MCP Apps support; available actions also depend on your account permissions and plan.

Try asking:

- “Build a website for my project and publish it on Spacefast.”
- “Publish this dashboard and give me a link to share.”
- “Update the content and design of my existing Spacefast site.”

## Skills

The [Spacefast plugin](https://github.com/spacefast/plugins) includes seven skills for clients that support them:

| Skill | Use it to |
| --- | --- |
| [spacefast](https://github.com/spacefast/plugins/tree/main/skills/spacefast) | Publish your work |
| [build-website](https://github.com/spacefast/plugins/tree/main/skills/build-website) | Build a website |
| [edit-space](https://github.com/spacefast/plugins/tree/main/skills/edit-space) | Update your site |
| [share-space](https://github.com/spacefast/plugins/tree/main/skills/share-space) | Share with the right people |
| [custom-domain](https://github.com/spacefast/plugins/tree/main/skills/custom-domain) | Use your own domain |
| [fix-deployment](https://github.com/spacefast/plugins/tree/main/skills/fix-deployment) | Fix or restore a deployment |
| [setup](https://github.com/spacefast/plugins/tree/main/skills/setup) | Get started |

## Help

If the connection returns `401 Unauthorized`, use your client's MCP login action to sign in again. If it cannot connect, confirm that it supports Streamable HTTP and OAuth.

- [Spacefast documentation](https://spacefast.com/docs/)
- [Agent setup](https://spacefast.com/docs/setup/)
- [Support](https://automattic.com/contact/)
- [Security reports](SECURITY.md)
- [Privacy policy](https://automattic.com/privacy/)
- [Terms of service](https://wordpress.com/tos/)

## MCP Registry

The registry name is `io.github.spacefast/mcp`. [`server.json`](server.json) describes the hosted endpoint for the [MCP Registry](https://registry.modelcontextprotocol.io/).

## License

The documentation and configuration files in this repository are available under the [MIT License](LICENSE). Use of the hosted Spacefast service is covered by its [terms of service](https://wordpress.com/tos/).
