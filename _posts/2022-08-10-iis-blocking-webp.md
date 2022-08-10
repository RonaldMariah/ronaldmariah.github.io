---
title: Unblocking .webp files from being served out on IIS
categories:
- Windows
- IIS
- WEBP
exerpt: "Unblocking .webp files from being served out on IIS"
---

Today I encountered an issue with IIS that prevents .webp files from being served out on IIS. The easiest way to enable .webp files is to go to
Features View > MIME Types and add an entry for .webp like the below

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/main/assets/iis-webp-enable/Screenshot%202022-08-10%20174443.png" />

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/main/assets/iis-webp-enable/Screenshot%202022-08-10%20174652.png" />