# Nginx Document Gateway Configuration

This document describes how the **Nginx “doc gateway”** is configured for the document-control system and related tools (e.g. purchase orders). It explains:

- Which files define the gateway.
- How Docker and Nginx are configured.
- What needs to change between **development (Windows)** and **production (Raspberry Pi)**.
- How this ties into the **`/files/` URL contract**.

For the logical URL/storage rules, see: `docs/url-contract.md`.

---

## 1. Purpose

The Nginx gateway provides a single HTTP endpoint that exposes files stored under the shared **`cad_iot`** folder.

Key points:

- All files live under a common storage root called `cad_iot`.
- The gateway serves everything under that root via the `/files/` URL prefix.
- Applications never need to know the physical host path, only the **relative path under `cad_iot`**.
- The same Docker setup is used for:
  - **Dev**: local folder on Windows.
  - **Prod**: CIFS-mounted share on the Raspberry Pi.

---

## 2. Files and directories

The gateway lives in the `doc-gateway` folder:

```text
doc-gateway/
  docker-compose.yml
  .env                       # Environment-specific host path
  nginx/
    default.conf             # Nginx server configuration
    html/
      index.html             # Landing page (optional override)
    logs/                    # Nginx logs (mounted from container)
