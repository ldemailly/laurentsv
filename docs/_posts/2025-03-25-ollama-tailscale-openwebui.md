---
layout: post
title:  "How to setup secure access to your local LLM UI"
categories: blog
comments: true # they still work!
date: 2025-03-25
modified_date: 2024-03-25 10:01:47 -0700
---


So like many I've been playing with local LLMs, here is how I've setup secure access to my local instance, using tailscale and my golang light proxy

## What you need

- A machine with a suitable GPU (in my case a desktop with a rtx 4090 but could also be an apple silicon mac, an Nvidia DGX Spark, etc)
- Docker ([Docker desktop](https://www.docker.com/products/docker-desktop/) or something compatible)
- [Tailscale](https://tailscale.com/download)
- [Fortio proxy](https://github.com/fortio/proxy#fortio-proxy)

That's it!

Optionally install [ollama](https://ollama.com/download) directly (I did) if you prefer local ollama vs running inside [Open Web UI's](https://docs.openwebui.com/getting-started/) image.

## Enabling https/TLS certs for Tailscale

In the admin DNS page of the admin console [login.tailscale.com/admin/dns](https://login.tailscale.com/admin/dns), under HTTPS Certificates, select Enable HTTPS (and read the certificate transparency disclaimer).

If you haven't done so already pick a "fun name" for your ts.net while you're at it.

See more details or updates [in Tailscale's doc](https://tailscale.com/kb/1153/enabling-https) if needed.

## Starting the LLM Web UI

From [getting started docs](https://docs.openwebui.com/getting-started/quick-start#step-2-run-the-container):

```sh
docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main
```

This gives you access on http://localhost:3000 but the next step will give https from anywhere on your tailscale network

## Starting the https proxy

If you have `go` installed you can `go install fortio.org/proxy@latest` otherwise get the binary for your host
at [github.com/fortio/proxy/releases](https://github.com/fortio/proxy/releases/)

Note: I use a binary directly on the host as accessing the tailscale daemon from inside docker isn't easy, or rather I didn't know how to map the unix domain socket it expects on linux to the host windows pipe for instance, if you know how, please
let me know, I'd rather use docker run -d for the proxy too (the image is `fortio/proxy:latest` if you get it working)

```sh
proxy -tailscale -default-route localhost:3000
```

(ps: on mac/linux you can run it through systemd, or plain `nohup proxy &` to keep it running; on windows `start /b` etc)

## That's it

Enjoy https://yourmachine.yourtsnet.ts.net/ access!


![Secure access sshot](/assets/tailscale_llm.png)
