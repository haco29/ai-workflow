# start-playwrite

Starts the Vite development server with HTTPS and opens a Playwright MCP browser to watch the site.

## Description

This command starts the Vite dev server with HTTPS and uses the Playwright MCP integration to open a browser window for interactive testing with automatic cookie sharing.

## Commands

**1. Start the development server:**

```bash
mise run dev:https
```

**2. Then use Playwright MCP to open browser:**

- Navigate to: `https://local.visor-staging.verbit.co:8080`
- Set cookie: `selected_tenant_id=1770`

## Expected Outcome

A Playwright browser window opens showing:

- ✅ The Visor web application
- ✅ Running on https://local.visor-staging.verbit.co:8080
- ✅ Cookies automatically shared with staging services
- ✅ Iframes work seamlessly with authentication
- ✅ Production-like HTTPS environment

## Usage Flow

1. Start dev server: `mise run dev:https`
2. Wait for server to be ready (shows "Local: https://local.visor-staging.verbit.co:8080")
3. Use Playwright MCP tools to open browser and navigate to the site
4. Playwright MCP allows you to interact with, test, and monitor the application

> **Note:** First-time setup required. Run `mise run setup:local-domain` once. See [docs/local-dev-setup.md](../../docs/local-dev-setup.md)

## Prerequisites

- Node.js and pnpm installed via mise
- **Playwright MCP** configured and available in Cursor
- Playwright browser installed (via MCP)
- All dependencies installed via `mise run install`
- Local subdomain setup completed (run `mise run setup:local-domain` once)

## Server Configuration

- **Port**: 8080 (configured in vite.config.ts)
- **Host**: :: (IPv6, accessible from all interfaces)
- **Proxies**:
  - `/insightsApi` → AI API
  - `/api/v2` → Orders API
  - `/visorApi` → Visor API

## Browser Configuration

- **URL**: https://local.visor-staging.verbit.co:8080
- **Protocol**: HTTPS with locally-trusted certificates
- **Required Cookie**: `selected_tenant_id=1770`
- **Cookie Domain**: .verbit.co (shared with staging services)
- **Benefits**: Automatic cookie sharing with iframes and staging services
