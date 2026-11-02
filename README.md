<div align="center">

# Slothify Audio Services

![MIT License](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)
![Built for Slothify](https://img.shields.io/badge/Service_For-Slothify-007ACC.svg?style=for-the-badge)
![Cloudflare Workers](https://img.shields.io/badge/Cloudflare_Workers-F38020?style=for-the-badge&logo=cloudflare&logoColor=white)
![Hono](https://img.shields.io/badge/Hono-E36002?style=for-the-badge&logo=hono&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Bun](https://img.shields.io/badge/Bun-000000?style=for-the-badge&logo=bun&logoColor=white)

<p align="center">
  <b>High-fidelity, edge-accelerated audio streaming microservice and media resolver for the <a href="https://github.com/PurveshShinde/Slothify">Slothify</a> music platform.</b>
</p>

[Overview](#overview) • [Ownership & Fork Heritage](#ownership--fork-heritage) • [Key Capabilities](#key-capabilities) • [Endpoints](#endpoints) • [Running Locally](#running-locally) • [Cloudflare Deployment](#cloudflare-deployment) • [Disclaimer](#legal-disclaimer) • [License](#license)

</div>

---

## Overview

**Slothify Audio Services** is the dedicated serverless backend microservice powering audio resolution, media stream extraction, and track decryption for **[Slothify](https://github.com/PurveshShinde/Slothify)**. 

Built on the ultra-lightweight **[Hono](https://hono.dev/)** web framework and compiled to run natively on **Cloudflare Workers** edge network, this microservice directly resolves high-bitrate studio audio streams (up to 320 kbps AAC/MP4) from public content delivery networks without intermediary transcoding or audio degrading.

---

## Ownership & Fork Heritage

### Project Maintainer & Host
- **Maintainer**: **Purvesh Shinde** ([@PurveshShinde](https://github.com/PurveshShinde))
- **Primary Consumer**: [Slothify](https://github.com/PurveshShinde/Slothify) — Ad-free, modern music streaming web application.
- **Operating Model**: Privately hosted and deployed as an independent Cloudflare Worker to serve the Slothify frontend and backend instances reliably without downtime or rate-limit contention.

### Upstream Attribution & Open-Source Lineage
This project is an independently maintained fork built upon the open-source foundation created by:

> **[sumitkolhe/jiosaavn-api](https://github.com/sumitkolhe/jiosaavn-api)**  
> *Original Author & Creator*: **Sumit Kolhe**  
> *Original License*: [MIT License](https://opensource.org/licenses/MIT)

We gratefully acknowledge Sumit Kolhe's reverse-engineering research and architectural design for media extraction structures. 

### Why Slothify Uses a Dedicated Fork
1. **Elimination of Public Instance Dependencies**: Public third-party instances frequently experience rate-limits, CORS changes, or sudden outages. Hosting a dedicated, personally deployed worker guarantees 99.9% uptime for Slothify users.
2. **Optimized CORS & Headers**: Configured with custom HTTP headers and caching policies tailored specifically for Slothify web and backend clients.
3. **Decryption Efficiency**: Streamlined media decryption (DES-ECB via `node-forge`) optimized for serverless edge execution with minimal compute latency.

---

## Key Capabilities

- **320 kbps High-Fidelity Audio**: Extracts direct source audio stream links across multiple bitrates (`320kbps`, `160kbps`, `96kbps`, `48kbps`, `12kbps`).
- **Edge Acceleration**: Deployed to Cloudflare Workers across 300+ global edge locations for sub-100ms response times worldwide.
- **Full Media Metadata**: Extracts tracks, albums, artist discographies, and playlists with high-resolution cover artwork.
- **DES Media URL Decryption**: Decrypts encrypted media URLs on the fly using internal decryption algorithms.
- **Strict TypeScript & Zod Validation**: Built with complete type-safety and request/response schema validation.

---

## Endpoints

| Endpoint | Method | Description |
| :--- | :--- | :--- |
| `/api/songs?id={id}` | `GET` | Get song details and 320 kbps streaming links by Song ID |
| `/api/songs?link={url}` | `GET` | Get song details and stream links by JioSaavn URL |
| `/api/albums?id={id}` | `GET` | Get full album details and tracklist |
| `/api/playlists?id={id}` | `GET` | Get playlist details and song collection |
| `/api/artists?id={id}` | `GET` | Get artist details and overview |
| `/api/search/songs?query={q}` | `GET` | Search for songs with live hit scoring |
| `/api/search/albums?query={q}` | `GET` | Search for albums |
| `/docs` | `GET` | Interactive Scalar OpenAPI documentation UI |

---

## Running Locally

### Prerequisites
- **Bun** (1.0.29+) *[Recommended]* or **Node.js** (v20+)

### 1. Install Dependencies
```sh
bun install
```

### 2. Start Development Server
```sh
bun run dev
```
The development server will start at `http://localhost:3000`.

### 3. Running with Docker (Alternative)
```sh
docker-compose up
```

---

## Cloudflare Deployment

To deploy your own dedicated instance of this microservice to Cloudflare Workers:

### 1. Authenticate with Wrangler
```sh
bun run wrangler login
```

### 2. Deploy to Cloudflare Edge
```sh
bun run deploy
```

Once deployed, your worker will be accessible at:
```
https://slothify-audio-services.<your-subdomain>.workers.dev
```

You can then set this URL in your Slothify backend environment:
```env
SAAVN_API_URL=https://slothify-audio-services.<your-subdomain>.workers.dev
```

---

## Legal Disclaimer

1. **Non-Commercial Educational Purpose**: This microservice is strictly non-commercial, open-source software created for personal educational and development use.
2. **No Hosting of Copyrighted Audio**: This service **does not host, download, or store any copyrighted audio or video files**. All audio streams resolved by this service are hosted directly on JioSaavn's public Content Delivery Networks (CDNs).
3. **Attribution**: All media content, artist trademarks, and song metadata remain the exclusive property of their respective copyright owners.

---

## License

This project is licensed under the **[MIT License](LICENSE)**:

- **Base Software**: Copyright &copy; 2024 **Sumit Kolhe**
- **Slothify Maintenance & Adaptations**: Copyright &copy; 2026 **Purvesh Shinde**

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files, to deal in the Software without restriction, subject to preserving the original copyright notice.
