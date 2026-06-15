---
title: "Use Your Mac for AI Agents: Self-Host Gemma 4 12 B with Pulumi and Tailscale"
url: "https://www.pulumi.com/blog/self-host-gemma4-llama-cpp-k8s-tailscale-pulumi/"
date: "2026-06-04"
author: "Pablo Seibelt"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
This post demonstrates running Gemma 4 12B open-weight model locally on an Apple M3 Max at approximately 20 output tokens per second using llama.cpp with Metal acceleration, achieving roughly 9% GPU memory savings versus Adam optimizer-based setups. The stack combines k3d for local Kubernetes orchestration, Open WebUI for a chat interface, Pulumi for infrastructure-as-code management, and Tailscale for secure remote access. Open-weight models running on consumer hardware keep data local, work offline, and eliminate per-token cloud costs.
