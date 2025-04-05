# 🐧 silly.install

[![⚠Nginx Black Site](https://img.shields.io/badge/install.silly.enterprises-routed_via_secret_nginx_black_site-000000?style=for-the-badge&logo=nginx&logoColor=white)](https://github.com/silly-enterprises/silly.install)

Welcome to the official **Silly Enterprises™ Installer** — your one-liner gateway to the ridiculous world of secure,
custom OS setups and optional chaos. ☁️⚙️

Whether you're spinning up a new server, configuring WSL, or just need to apply your favorite config snacks™, this
script helps you get started with a proper `deb.silly.enterprises` APT repo and optionally more.

---

## 🚀 Quickstart

### 🐧 For Linux / WSL (Ubuntu/Debian)

Run this to add the Silly APT repo and install your base packages:

```bash
curl -fsSL https://install.silly.enterprises | bash
```

---

### 🪟 For Windows PowerShell

Runs the Linux-side install script **inside WSL** (if installed):

```powershell
irm https://install.silly.enterprises | iex
```

This will:

- Detect WSL
- Run the Linux-side bootstrap automatically
- Show friendly debug output 💬

---

## 💡 Features

- 🐧 Adds the APT repository: `https://deb.silly.enterprises`
- 🔐 Configures keyring verification (no `trusted=yes` nonsense!)
- ⚙️ Installs a minimal or full set of silly packages
- 🪛 Supports debug & dry-run modes

---

## 🧪 Debug / Dry-run Mode

Use this to preview what would be done without making changes:

```bash
curl -fsSL https://install.silly.enterprises | bash -s -- --dry-run
```

For verbose logging:

```bash
curl -fsSL https://install.silly.enterprises | bash -s -- --debug
```

All logs are saved to `/tmp/silly-install-<timestamp>.log`

---

## 🛠️ Optional Flags

| Flag           | Description                             |
|----------------|-----------------------------------------|
| `--debug`      | Enables verbose debug output            |
| `--dry-run`    | Prints commands, makes no changes       |
| `--no-keyring` | Adds repo without keyring (less secure) |

---

## 💼 For Developers

This repo lives at:  
📦 [`silly.install`](https://github.com/silly-enterprises/silly.install)

It is served via:
🛰️ [`static.silly.enterprises`](https://github.com/silly-enterprises/static.silly.enterprises) → GitHub Pages (proxy)

---

## Secret Nginx Black Site (powered by Silly Magic™)

> 🧙‍♂️ *Buried within the infrastructure layer of Silly Enterprises lies a forbidden spell…*  
> A way to reroute the forces of the web, based only on whispers from the User-Agent string…

```nginx
map $http_user_agent $install_script_url {
    default "https://static.silly.enterprises/scripts/install.sh";
    ~*powershell "https://static.silly.enterprises/scripts/install.ps1";
}

server {
    listen                  443 ssl http2;
    listen                  [::]:443 ssl http2;
    server_name             install.silly.enterprises;


    location / {
        proxy_pass $install_script_url;
        proxy_set_header Host static.silly.enterprises;
        proxy_redirect off;
    }

    ssl_certificate /etc/letsencrypt/live/deb.silly.enterprises/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/deb.silly.enterprises/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}

server {
    listen                  443 ssl http2;
    listen                  [::]:443 ssl http2;
    server_name             deb.silly.enterprises;
    
    location = / {
        proxy_pass https://static.silly.enterprises/apt/index.html;
        proxy_set_header Host $host;
     }

      location / {
        proxy_pass https://static.silly.enterprises/apt/;
        proxy_set_header Host static.silly.enterprises;
        proxy_redirect off;
      }

    ssl_certificate /etc/letsencrypt/live/deb.silly.enterprises/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/deb.silly.enterprises/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}
```

---

### ⚙️ What does this eldritch block do?

- 🧼 Routes `curl` requests to `install.sh`
- 🪟 Detects PowerShell via User-Agent and sends `install.ps1`
- 🪞 Masquerades as a GitHub Pages host with perfectly clean URLs
- ✨ Enables `curl install.silly.enterprises | bash` and `irm install.silly.enterprises | iex` to *just work* (it's wild
  that it works tbh XD)

---

### ☣️ Warning:

> This spell is highly experimental.  
> Do not stare directly at the config file for too long, lest you attract reverse proxies from another dimension.

Remember to always give your sudo password to random scripts <3

---

## 🌈 Built with love by the Silly OS™ Division™

> Be silly.™