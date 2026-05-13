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
- `account_id`: Your Cloudflare account ID
- `workers_dev`: Set to `true` for `*.workers.dev` deployment, or `false` for custom domain
- For custom domain:
  - `route`: Your worker route (e.g., `example.com/api/proxy/*`)
  - `zone_id`: Your domain's zone ID

### 4. Update WORKER_BASE in index.js
Replace `{url here}` with your worker's URL (e.g., `https://yumezone.example.com` or `https://yourname.workers.dev`)

### 5. Deploy
```bash
npm run deploy
# or
wrangler deploy
```

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
