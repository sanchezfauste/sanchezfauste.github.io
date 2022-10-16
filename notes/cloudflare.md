# Cloudflare
Useful Cloudflare tricks and configs.

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
```conf
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
