# YumeZone HLS Proxy Worker

A Cloudflare Worker for proxying HLS streams with custom headers and M3U8 rewriting.

## Setup & Deployment

### 1. Install Wrangler CLI
```bash
npm install -g @cloudflare/wrangler
```

### 2. Authenticate with Cloudflare
```bash
wrangler login
```

### 3. Configure wrangler.toml
Edit `wrangler.toml` and add:
- `account_id`: Your Cloudflare account ID (required)
- `workers_dev`: Set to `true` for `*.workers.dev` deployment, or `false` for custom domain
- For custom domain:
  - `route`: Your worker route (e.g., `example.com/api/proxy/*`)
  - `zone_id`: Your domain's zone ID

**Important**: You MUST add your `account_id` to wrangler.toml before deploying!

### 4. Update WORKER_BASE in index.js
Replace the hardcoded URL with your actual worker URL after deployment:
- If using workers.dev: `https://yume-proxy.<your-subdomain>.workers.dev`
- If using custom domain: `https://your-domain.com`

**This is critical**: The M3U8 rewriting feature uses this constant to rewrite playlist URLs. If it doesn't match your actual worker URL, the rewritten links will point to the wrong location and fail.

### 5. Deploy
```bash
npm run deploy
# or
wrangler deploy
```

After deployment, Wrangler will output your worker URL. Copy that URL and update `WORKER_BASE` in `index.js`, then redeploy.

### 6. Test
```bash
# Health check
curl https://yourname.workers.dev/health

# Proxy a URL
curl "https://yourname.workers.dev/proxy?url=https://example.com/video.m3u8"
```

## Features

- **Base64URL encoding**: `/p/<base64url>` - secure, obfuscated requests
- **Legacy support**: `/proxy?url=...&ref=...` - backward compatible
- **CDN Rules**: Pre-configured headers for popular CDN hosts
- **M3U8 Rewriting**: Auto-rewrites playlist URLs to go through proxy
- **Browser impersonation**: Spoofs User-Agent and other headers
- **Range requests**: Supports HTTP Range header for video segments
- **CORS enabled**: Works with cross-origin requests

## Environment

This is a Service Worker - no external dependencies required. Uses native Cloudflare Worker APIs.

## File Structure
```
.
├── index.js          # Main worker code
├── package.json      # Project metadata
├── wrangler.toml     # Cloudflare Worker config
├── .gitignore        # Git ignore rules
└── README.md         # This file
```
