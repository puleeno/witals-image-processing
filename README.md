# 🚀 Witals-Image: Hybrid Edge/Native Image Optimizer

**Witals-Image** is the specialized image processing engine for the **WitalsPanel** ecosystem. Engineered for absolute **Core Web Vitals (CWV)** dominance, it leverages Rust to provide a "Write Once, Run Anywhere" solution. It features a unique **Hybrid Mode**: running natively on Ubuntu/VPS for bulk tasks or at the Edge via Cloudflare Workers (WASM) for global delivery.

---

## 🌟 Key Features

* **⚡ High-Performance Core:** Written in pure Rust, utilizing high-quality Lanczos3 resampling and turbo-charged encoders (`zune-jpeg`, `photon-rs`).
* **🔄 Hybrid Deployment:**
* **Native Mode:** Optimized for VPS environments (Ubuntu + OpenLiteSpeed). Perfect for background processing or origin-shielding.
* **Edge Mode (WASM):** Compiles to WebAssembly for Cloudflare Workers. Processes images at the nearest edge node to the user, slashing TTFB.


* **💎 CWV Optimized:** Automatic conversion to **WebP/AVIF**, aggressive EXIF metadata stripping, and integrated support for **BlurHash/LQIP** generation.
* **📦 Efficient Footprint:** Uses **PNPM** for the management dashboard to ensure zero-redundancy in `node_modules` and minimal disk usage on your server.

---

## 🏗️ Project Structure (Rust Workspace)

```text
.
├── core-logic/           # Shared Library: Core image manipulation logic
├── native-app/           # Native Mode: CLI/Service binary for Ubuntu/VPS
├── wasm-worker/          # Edge Mode: Cloudflare Workers WASM deployment
└── witals-ui/            # Dashboard: Management UI (Vue 3 + Vite + PNPM)

```

---

## 🛠️ Development Prerequisites (Ubuntu)

Ensure your Ubuntu environment is ready for Rust system-level compilation:

* **OS:** Ubuntu 22.04+ LTS.
* **Rust Toolchain:** `rustup` with `wasm32-wasi` and `x86_64-unknown-linux-gnu` targets.
* **System Dependencies:**
```bash
sudo apt update && sudo apt install build-essential pkg-config libssl-dev -y

```


* **Node.js Environment:** `fnm` for version management and `pnpm` for package storage.

---

## 🚀 Getting Started

### 1. Local Development (Native)

To process an image locally using the native binary:

```bash
cargo run -p native-app -- --input sample.jpg --output sample.webp --width 1200

```

### 2. Edge Deployment (WASM)

Deploy the optimizer to 300+ cities globally via Cloudflare:

```bash
cd wasm-worker
wrangler deploy

```

---

## 🎯 Roadmap

* [ ] Implement multi-threaded parallel processing using **Rayon**.
* [ ] Add native **AVIF** encoding support for maximum compression.
* [ ] Develop a **Smart-Cache Bridge** between RoadRunner and ProxySQL.
* [ ] Real-time CWV scoring dashboard for processed assets.

---

## ⚖️ License

Copyright © 2026 **Puleeno Nguyen**. All rights reserved. Built for the performance-obsessed web.
