# Cloudflare
Useful Cloudflare tricks and configs.

## Get real visitor IPs in nginx
This allows you to get the real visitor IPs in `nginx` instead of Cloudflare's proxy IPs. Useful if you use Cloudflare's proxy.

Edit `/etc/nginx/nginx.conf` and add the following inside `http` section:
```nginx
# Show original visitor IP from Cloudflare proxy requests
set_real_ip_from 103.21.244.0/22;
set_real_ip_from 103.22.200.0/22;
set_real_ip_from 103.31.4.0/22;
set_real_ip_from 104.16.0.0/13;
set_real_ip_from 104.24.0.0/14;
set_real_ip_from 108.162.192.0/18;
set_real_ip_from 131.0.72.0/22;
set_real_ip_from 141.101.64.0/18;
set_real_ip_from 162.158.0.0/15;
set_real_ip_from 172.64.0.0/13;
set_real_ip_from 173.245.48.0/20;
set_real_ip_from 188.114.96.0/20;
set_real_ip_from 190.93.240.0/20;
set_real_ip_from 197.234.240.0/22;
set_real_ip_from 198.41.128.0/17;
set_real_ip_from 2400:cb00::/32;
set_real_ip_from 2606:4700::/32;
set_real_ip_from 2803:f800::/32;
set_real_ip_from 2405:b500::/32;
set_real_ip_from 2405:8100::/32;
set_real_ip_from 2a06:98c0::/29;
set_real_ip_from 2c0f:f248::/32;
real_ip_header CF-Connecting-IP;
```
Note: `set_real_ip_from` must be [Cloudflare IPs](https://www.cloudflare.com/ips/). Check if IPs are diferent and edit config as needed.

Reload `nginx` config:
```bash
systemctl reload nginx.service
```

## Get real visitor IPs in Apache2
This allows you to get the real visitor IPs in `apache2` instead of Cloudflare's proxy IPs. Useful if you use Cloudflare's proxy. This also applies if you use `apache2` behind some other reverse proxy like `nginx`.

First we need to enable remoteip module with:

```bash
a2enmod remoteip
```

Next create remoteip config telling what header and IP ranges to use. Create the file:
```
/etc/apache2/conf-available/remoteip.conf
```

With the following config:
```apache
RemoteIPHeader X-Forwarded-For
RemoteIPTrustedProxy 103.21.244.0/22
RemoteIPTrustedProxy 103.22.200.0/22
RemoteIPTrustedProxy 103.31.4.0/22
RemoteIPTrustedProxy 104.16.0.0/13
RemoteIPTrustedProxy 104.24.0.0/14
RemoteIPTrustedProxy 108.162.192.0/18
RemoteIPTrustedProxy 131.0.72.0/22
RemoteIPTrustedProxy 141.101.64.0/18
RemoteIPTrustedProxy 162.158.0.0/15
RemoteIPTrustedProxy 172.64.0.0/13
RemoteIPTrustedProxy 173.245.48.0/20
RemoteIPTrustedProxy 188.114.96.0/20
RemoteIPTrustedProxy 190.93.240.0/20
RemoteIPTrustedProxy 197.234.240.0/22
RemoteIPTrustedProxy 198.41.128.0/17
RemoteIPTrustedProxy 2400:cb00::/32
RemoteIPTrustedProxy 2606:4700::/32
RemoteIPTrustedProxy 2803:f800::/32
RemoteIPTrustedProxy 2405:b500::/32
RemoteIPTrustedProxy 2405:8100::/32
RemoteIPTrustedProxy 2a06:98c0::/29
RemoteIPTrustedProxy 2c0f:f248::/32
```
Note: `RemoteIPTrustedProxy` must be [Cloudflare IPs](https://www.cloudflare.com/ips/). Check if IPs are diferent and edit config as needed.

Enable remoteip config and restart apache2:
```bash
a2enconf remoteip
systemctl restart apache2
```
