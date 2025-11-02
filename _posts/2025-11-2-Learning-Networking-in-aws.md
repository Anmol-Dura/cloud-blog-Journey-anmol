---
layout: post
title: "Learning Networking in AWS"
date: 2025-11-2
tags: [aws, cloud, learning]
---

It has been few weeks I started learning about, the networking part of aws which covers form APIs concepts where how APIs are divided or distributed form the CIDR(Class inter-domain-Routing) block of ips and how subnets is just a chunk of CIDR block.

**SUBNETTING:** it is an important concept in networking where we can and in most cases have to separate the network portion form the host portion form an ip address. Meaning in the network there are ip addresses for host:(**a client(simply a computer) in a network**) and network portion.

**Why is networking important:why don't we just use just hoast ips for communicating between the client?**
Imagine, you have a bunch of computer in a network, and what is a network?: a group of computer which can communicate with each other using switch. if a client in the network is trying to connect to other client then it sends a broadcast on the network: **who is ip:195.195.195.0?** and then every other computer on the network sends a broadcast to ever other computer saying **not me** except for that one computer **ip:195.195.195.0**.

This can create chaos and problem in the network, therefore we need to allocate ips for network portion so that we can have manageable network chunks and does not crash the network.

<style>
  div { font-family: Arial, sans-serif; background: #0b0c10; color: #e8e8e8; text-align:center; padding:2rem; }
  svg { background: #111317; border-radius: 12px; box-shadow: 0 0 20px rgba(0,0,0,0.4); margin: 2rem auto; display:block; max-width: 800px; }
  h2 { color: #7c5cff; }
</style>
<div class="black-out">
<h2>🧩 Simple Personal Company Network</h2>

  <svg width="700" height="400">
  <!-- Internet Cloud -->
  <ellipse cx="350" cy="50" rx="90" ry="35" fill="#444" stroke="#888" stroke-width="2"/>
  <text x="320" y="55" fill="#fff">Internet</text>

  <!-- Router -->
  <rect x="300" y="120" width="100" height="40" rx="6" ry="6" fill="#222" stroke="#7c5cff" stroke-width="2"/>
  <text x="325" y="145" fill="#fff">Router</text>

  <!-- Switch -->
  <rect x="300" y="200" width="100" height="40" rx="6" ry="6" fill="#222" stroke="#00c896" stroke-width="2"/>
  <text x="325" y="225" fill="#fff">Switch</text>

  <!-- Devices -->
  <rect x="150" y="280" width="80" height="40" rx="6" ry="6" fill="#222" stroke="#fff" stroke-width="1.5"/>
  <text x="160" y="305" fill="#fff">Laptop</text>

  <rect x="310" y="280" width="80" height="40" rx="6" ry="6" fill="#222" stroke="#fff" stroke-width="1.5"/>
  <text x="320" y="305" fill="#fff">Desktop</text>

  <rect x="470" y="280" width="80" height="40" rx="6" ry="6" fill="#222" stroke="#fff" stroke-width="1.5"/>
  <text x="485" y="305" fill="#fff">Printer</text>

  <!-- Lines -->
  <line x1="350" y1="85" x2="350" y2="120" stroke="#888" stroke-width="2" stroke-dasharray="4 2"/>
  <line x1="350" y1="160" x2="350" y2="200" stroke="#7c5cff" stroke-width="2"/>
  <line x1="340" y1="240" x2="190" y2="280" stroke="#00c896" stroke-width="2"/>
  <line x1="350" y1="240" x2="350" y2="280" stroke="#00c896" stroke-width="2"/>
  <line x1="360" y1="240" x2="510" y2="280" stroke="#00c896" stroke-width="2"/>
</svg>

<h2>🏢 Sophisticated Company Network (with VLANs, Firewall & Servers)</h2>

<svg width="900" height="600">
  <!-- Internet Cloud -->
  <ellipse cx="450" cy="60" rx="100" ry="35" fill="#444" stroke="#888" stroke-width="2"/>
  <text x="410" y="65" fill="#fff">Internet</text>

  <!-- Firewall -->
  <rect x="400" y="120" width="100" height="40" rx="6" ry="6" fill="#222" stroke="#ff4d4d" stroke-width="2"/>
  <text x="420" y="145" fill="#fff">Firewall</text>

  <!-- Core Router -->
  <rect x="400" y="190" width="100" height="40" rx="6" ry="6" fill="#222" stroke="#7c5cff" stroke-width="2"/>
  <text x="420" y="215" fill="#fff">Core Router</text>

  <!-- Distribution Switches -->
  <rect x="230" y="260" width="100" height="40" rx="6" ry="6" fill="#222" stroke="#00c896" stroke-width="2"/>
  <text x="245" y="285" fill="#fff">Switch (VLAN A)</text>
  <rect x="570" y="260" width="100" height="40" rx="6" ry="6" fill="#222" stroke="#00c896" stroke-width="2"/>
  <text x="585" y="285" fill="#fff">Switch (VLAN B)</text>

  <!-- Office Devices VLAN A -->
  <rect x="160" y="350" width="80" height="40" rx="6" ry="6" fill="#222" stroke="#fff" stroke-width="1.5"/>
  <text x="170" y="375" fill="#fff">Admin PC</text>
  <rect x="280" y="350" width="80" height="40" rx="6" ry="6" fill="#222" stroke="#fff" stroke-width="1.5"/>
  <text x="290" y="375" fill="#fff">Finance PC</text>

  <!-- Server VLAN B -->
  <rect x="550" y="350" width="80" height="40" rx="6" ry="6" fill="#222" stroke="#fff" stroke-width="1.5"/>
  <text x="560" y="375" fill="#fff">Web Server</text>
  <rect x="670" y="350" width="80" height="40" rx="6" ry="6" fill="#222" stroke="#fff" stroke-width="1.5"/>
  <text x="675" y="375" fill="#fff">DB Server</text>

  <!-- WiFi Access Point -->
  <rect x="400" y="460" width="100" height="40" rx="6" ry="6" fill="#222" stroke="#ffb84d" stroke-width="2"/>
  <text x="425" y="485" fill="#fff">WiFi Access</text>

  <!-- Connections -->
  <line x1="450" y1="95" x2="450" y2="120" stroke="#888" stroke-width="2" stroke-dasharray="4 2"/>
  <line x1="450" y1="160" x2="450" y2="190" stroke="#ff4d4d" stroke-width="2"/>
  <line x1="450" y1="230" x2="280" y2="260" stroke="#7c5cff" stroke-width="2"/>
  <line x1="450" y1="230" x2="620" y2="260" stroke="#7c5cff" stroke-width="2"/>
  <line x1="280" y1="300" x2="200" y2="350" stroke="#00c896" stroke-width="2"/>
  <line x1="280" y1="300" x2="320" y2="350" stroke="#00c896" stroke-width="2"/>
  <line x1="620" y1="300" x2="590" y2="350" stroke="#00c896" stroke-width="2"/>
  <line x1="620" y1="300" x2="710" y2="350" stroke="#00c896" stroke-width="2"/>
  <line x1="450" y1="230" x2="450" y2="460" stroke="#ffb84d" stroke-width="2"/>
</svg>
</div>
