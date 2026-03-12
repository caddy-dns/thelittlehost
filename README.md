The Little Host module for Caddy
=================================

This package contains a DNS provider module for [Caddy](https://github.com/caddyserver/caddy). It can be used to manage DNS records with [The Little Host](https://thelittlehost.com).

## Caddy module name

```
dns.providers.thelittlehost
```

## Config examples

To use this module for the ACME DNS challenge, [configure the ACME issuer in your Caddy JSON](https://caddyserver.com/docs/json/apps/tls/automation/policies/issuer/acme/) like so:

```json
{
	"module": "acme",
	"challenges": {
		"dns": {
			"provider": {
				"name": "thelittlehost",
				"api_token": "{env.TLH_API_TOKEN}",
			},
			"propagation_delay": "60s",
			"propagation_timeout": "120s"
		}
	}
}
```

or with the Caddyfile:

```
# globally
{
	acme_dns thelittlehost {
		api_token  {env.TLH_API_TOKEN}
	}
}
```

```
# one site
tls {
	dns thelittlehost {
		api_token  {env.TLH_API_TOKEN}
	}
	propagation_delay  60s
	propagation_timeout 120s
}
```

The `api_token` can also be provided inline:

```
tls {
	dns thelittlehost {env.TLH_API_TOKEN}
	propagation_delay  60s
	propagation_timeout 120s
}
```

> **Note:** The Little Host's DNS backend may take approximately 30 seconds to propagate new records to its authoritative nameservers. The `propagation_delay` and `propagation_timeout` values above account for this and are recommended for ACME DNS-01 challenges to succeed. These are standard Caddy TLS options — they cannot be set as defaults by the provider module.
