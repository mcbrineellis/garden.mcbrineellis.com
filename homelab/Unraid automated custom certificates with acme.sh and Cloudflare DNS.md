#acme 

Let's install acme.sh:

```
curl https://get.acme.sh | sh -s email=your@email.com
```

Following the instructions on [acme.sh wiki (How to use DNS API - Cloudflare)](https://github.com/acmesh-official/acme.sh/wiki/dnsapi#dns_cf) I created a token in under My Profile > API Tokens in Cloudflare with permissions for `Zone.DNS` to all zones.

Then I added the token my `~/.bashrc` file:

```
export CF_Token="token123"
```

We will use the `--reloadcmd` option to combine the cert chain and key into a single file and place it into a file located in `/boot/config/ssl/certs/[servername]_unraid_bundle.pem` as  detailed in the [Unraid Docs - HTTPS with custom certificate](https://docs.unraid.net/unraid-os/manual/security/secure-webgui-ssl/#custom-certificates) page.

```
acme.sh --issue --server letsencrypt --dns dns_cf --fullchain-file /boot/config/ssl/certs/consbo_unraid.pem --key-file /boot/config/ssl/certs/consbo_unraid.key --reloadcmd "cat \$CERT_FULLCHAIN_PATH \$CERT_KEY_PATH >/boot/config/ssl/certs/[servername]_unraid_bundle.pem" -d [servername].[localTLD]
```

After this, set **Use SSL/TLS** to _Yes_ under the **Settings > Management Access** page and everything should work!