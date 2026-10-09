# Log10x

Rank log patterns by cost, then compact, tier down or offload them. No log data reaches log10x.

This plugin installs the Log10x MCP server (npm package [`log10x-mcp`](https://www.npmjs.com/package/log10x-mcp), source at [github.com/log-10x/log10x-mcp](https://github.com/log-10x/log10x-mcp), MIT) for Cursor, Grok Bot and Grok Build. The server runs on the machine the agent runs on, over stdio, started with `npx` and pinned to one release. Documentation: [doc.log10x.com/apps/mcp](https://doc.log10x.com/apps/mcp/).

## Without an account

The server starts read-only against the public Log10x demo dataset. Nothing needs configuring. The tool that writes engine policy is not available in this mode.

## With an account

Ask the agent to sign in. It calls `log10x_signin_start`, the browser opens a sign-in page, and `log10x_signin_complete` saves the key to `~/.log10x/credentials`. Setting `LOG10X_API_KEY` in the environment works too.

## Network endpoints

| Host | When it is called | What it receives |
|---|---|---|
| `prometheus.log10x.com` | Every metrics query | PromQL queries over per-pattern metrics, with the account key or the public demo key |
| `api.log10x.com` | Sign-in and licence checks | The sign-in token and licence requests |
| `auth.log10x.com` | Sign-in | The device sign-in request |
| `dl.log10x.com` | Never, in this plugin | The server can fetch tool descriptions from here; the plugin turns that off with `LOG10X_MANIFEST_DISABLED=1`, so the descriptions that run are the ones in the pinned release |

The server also calls endpoints the user configures, such as a SIEM's API or a Kubernetes cluster. Usage telemetry is off unless `LOG10X_API_KEY` and `PROMETHEUS_REMOTE_WRITE_URL` (or `LOG10X_TELEMETRY_URL`) are both set.

## Local commands

Some tools run command-line programs that are already installed:

- the 10x engine (`tenx`, or its Docker image with `LOG10X_TENX_MODE=docker`) to name sample lines locally; the lines never leave the machine
- `kubectl` and `aws` to read cluster and account state during discovery
- `kubectl` to write the engine's policy ConfigMap, or `gh` to open a policy pull request, only with an account, only when the agent asks to apply, and subject to the client's approval prompt
- the system browser, for sign-in

`LOG10X_MCP_READ_ONLY=1` blocks the policy writes, the account changes and sign-in.

## License

MIT. See [LICENSE](LICENSE).
