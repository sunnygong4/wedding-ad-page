# Deploying wedding.sunnygong.com

Static site → cloned on your VPS → served by nginx → Cloudflare in front for DNS + HTTPS.

---

## 1. On the server: clone the repo

SSH into your VPS, then:

```bash
sudo mkdir -p /var/www
cd /var/www

# Private repo: paste the GitHub token when prompted for password,
# username is your GitHub username (sunnygong4).
sudo git clone https://github.com/sunnygong4/wedding-ad-page.git
sudo chown -R www-data:www-data /var/www/wedding-ad-page
```

To pull future updates:

```bash
cd /var/www/wedding-ad-page && sudo git pull
```

> Tip: to avoid typing the token every pull, store it once:
> `git config --global credential.helper store` (saves it to ~/.git-credentials in plaintext — fine on a server you control).

## 2. On the server: configure nginx

```bash
sudo cp /var/www/wedding-ad-page/deploy/nginx-wedding.conf \
        /etc/nginx/sites-available/wedding.sunnygong.com
sudo ln -s /etc/nginx/sites-available/wedding.sunnygong.com \
           /etc/nginx/sites-enabled/
sudo nginx -t            # should say "syntax is ok / test is successful"
sudo systemctl reload nginx
```

## 3. In Cloudflare: point the subdomain at the server

Cloudflare dashboard → your domain `sunnygong.com` → **DNS** → **Add record**:

| Field   | Value                          |
|---------|--------------------------------|
| Type    | `A`                            |
| Name    | `wedding`                      |
| IPv4    | your server's public IP        |
| Proxy   | **Proxied** (orange cloud) ✅  |
| TTL     | Auto                           |

(If your server only has IPv6, use an `AAAA` record with the IPv6 address instead.)

## 4. HTTPS

Because the record is **Proxied**, Cloudflare gives you HTTPS automatically on the edge.
Set the encryption mode under **SSL/TLS → Overview**:

- **Fastest to go live:** `Flexible` — Cloudflare serves HTTPS, talks HTTP to your server.
  Works with the port-80 nginx config as-is. Fine for launch; not end-to-end encrypted.
- **Recommended:** `Full (strict)` — install a free **Cloudflare Origin Certificate**
  on the server (SSL/TLS → Origin Server → Create Certificate), add a `listen 443 ssl;`
  block pointing at the cert, and you get end-to-end TLS. Ask Claude to generate the
  443 block when you're ready.

## 5. Verify

```bash
curl -I http://YOUR_SERVER_IP            # direct: expect HTTP 200
```
Then open https://wedding.sunnygong.com in a browser (DNS can take a few minutes).
