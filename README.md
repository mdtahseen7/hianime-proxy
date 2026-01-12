# HiAnime Proxy - Cloudflare Worker

A high-performance HLS proxy built with Cloudflare Workers. This proxy handles CORS, manifest rewriting, and bypasses port restrictions using raw TCP/TLS sockets.

## Why Cloudflare Workers?

1. **Unlimited Bandwidth**: Unlike Vercel's free plan (10GB), Cloudflare offers effectively unlimited bandwidth for edge functions.
2. **Low Latency**: Runs on Cloudflare's global edge network, minimizing TTFB (Time to First Byte).
3. **Streaming Architecture**: Uses `ReadableStream` and `TransformStream` to pipe video data directly from the socket to the client, ensuring high speed and low memory usage.

## Advanced Features

* **Socket-Based Proxying**: Cloudflare's `fetch()` API blocks non-standard ports like **2228**. This project implements a custom HTTP/1.1 client over raw TCP/TLS sockets (`cloudflare:sockets`) to bypass this restriction.
* **On-the-fly De-chunking**: Handles source CDNs that use `Transfer-Encoding: chunked` by reconstructing the stream in real-time.
* **Stealth Headers**: Spoof a modern Chrome browser environment to prevent CDN blocks.
* **M3U8 Tag Rewriting**: Automatically updates playlists, variant manifests, and decryption keys to route through the proxy.

## Deployment

### Prerequisites

* Node.js installed.
* A Cloudflare account.
* A Cloudflare API Token with `Edit Cloudflare Workers` permissions.

### Steps

1. Install dependencies:

    ```bash
    npm install
    ```

2. Deploy to Cloudflare:

    ```bash
    $env:CLOUDFLARE_API_TOKEN='your_token_here'; npm run deploy
    ```

## Usage

```http
GET https://your-worker.workers.dev/?url=ENCODED_URL&referer=ENCODED_REFERER
```

### Parameters

* `url`: The video/m3u8 URL to proxy (fully URL-encoded).
* `referer`: The referer header to spoof (optional, defaults to `https://megacloud.tv`).

## Development

To run locally in development mode:

```bash
npm run dev
```

## Author

**RY4N**
* GitHub: [@ryanwtf88](https://github.com/ryanwtf88)
