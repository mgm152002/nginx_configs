# nginx_configs

A clean, ready-to-use Nginx configuration repository with a sample static website.

## Repository Structure

```
nginx_configs/
├── nginx/                        # Nginx configuration files
│   ├── nginx.conf                # Main Nginx configuration (global settings)
│   ├── conf.d/
│   │   └── default.conf          # Default catch-all server block
│   ├── sites-available/
│   │   └── default               # Virtual host config for example.com
│   └── sites-enabled/
│       └── default -> ../sites-available/default   # Symlink to enable the site
└── www/
    └── html/
        └── index.html            # Dummy static website
```

## Files Overview

| File | Purpose |
|------|---------|
| `nginx/nginx.conf` | Global Nginx settings: worker processes, events, HTTP defaults, gzip, logging |
| `nginx/conf.d/default.conf` | Catch-all default server block (port 80) |
| `nginx/sites-available/default` | Named virtual host for `example.com` |
| `nginx/sites-enabled/default` | Symlink that **enables** the virtual host |
| `www/html/index.html` | Dummy HTML landing page served as the document root |

## Usage

### Deploying to a Linux server

1. **Copy Nginx configs:**
   ```bash
   sudo cp nginx/nginx.conf /etc/nginx/nginx.conf
   sudo cp nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf
   sudo cp nginx/sites-available/default /etc/nginx/sites-available/default
   sudo ln -sf /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default
   ```

2. **Copy the dummy website:**
   ```bash
   sudo cp -r www/html/* /var/www/html/
   ```

3. **Test and reload:**
   ```bash
   sudo nginx -t && sudo systemctl reload nginx
   ```

### Enabling / Disabling sites

- **Enable** a site by creating a symlink in `sites-enabled/`:
  ```bash
  sudo ln -sf /etc/nginx/sites-available/mysite /etc/nginx/sites-enabled/mysite
  ```
- **Disable** a site by removing its symlink:
  ```bash
  sudo rm /etc/nginx/sites-enabled/mysite
  ```

## Notes

- `sites-available/` stores all virtual host configs (enabled or not).
- `sites-enabled/` only contains symlinks to configs that are **actively served**.
- `conf.d/` is loaded unconditionally — use it for global or default blocks.
