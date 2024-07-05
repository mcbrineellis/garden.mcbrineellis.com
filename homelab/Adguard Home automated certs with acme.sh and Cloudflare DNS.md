---
created: 2024-06-26T15:14
updated: 2024-06-26T21:32
---
#acme

Install **acme.sh** with the following command or following the instructions on their wiki:

```
curl https://get.acme.sh | sh -s email=your@email.com
```

Following the instructions on [acme.sh wiki (How to use DNS API - Cloudflare)](https://github.com/acmesh-official/acme.sh/wiki/dnsapi#dns_cf) I created a token in under My Profile > API Tokens in Cloudflare with permissions for `Zone.DNS` to all zones.

Then I added the token my `~/.bashrc` file:

```
export CF_Token="token123"
```

I requested the cert:

```
acme.sh --issue --server letsencrypt --dns dns_cf -d adguardhome.csilo.ca

...
truncated the output :)
...

[Wed Jun 26 15:14:03 EDT 2024] Your cert is in: /home/connor/.acme.sh/adguardhome.csilo.ca_ecc/adguardhome.csilo.ca.cer
[Wed Jun 26 15:14:03 EDT 2024] Your cert key is in: /home/connor/.acme.sh/adguardhome.csilo.ca_ecc/adguardhome.csilo.ca.key
[Wed Jun 26 15:14:03 EDT 2024] The intermediate CA cert is in: /home/connor/.acme.sh/adguardhome.csilo.ca_ecc/ca.cer
[Wed Jun 26 15:14:03 EDT 2024] And the full chain certs is there: /home/connor/.acme.sh/adguardhome.csilo.ca_ecc/fullchain.cer
```

Finally I added the cert & key paths provided by acme.sh into the Adguard Home GUI's [certificate settings](https://github.com/AdguardTeam/AdGuardHome/wiki/Encryption#configure-home).