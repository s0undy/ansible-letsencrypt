# Let's Encrypt SSL Automation

Ansible playbook for automated SSL certificate generation and renewal using Let's Encrypt and Cloudflare DNS validation.

## What It Does

Generates SSL certificates through the ACME protocol with DNS-01 challenge validation via Cloudflare. Supports wildcard certificates, automatic renewals, and multiple certificate formats.

## How It Works

1. Creates or reuses ACME account with Let's Encrypt
2. Generates private key and certificate signing request
3. Creates temporary DNS TXT records in Cloudflare for domain validation
4. Retrieves signed certificate and chain from Let's Encrypt
5. Exports certificates in PEM and PFX formats
6. Cleans up validation records

The playbook is idempotent. It checks certificate expiration and only renews when necessary (30 days before expiry).

## Features

- Configurable key types (RSA or ECC/ECDSA)
- Multi-domain and wildcard support via Subject Alternative Names
- Automated certificate backups before renewal
- Password-protected private keys
- PFX/PKCS#12 export (standard and legacy formats)
- Staging and production environment support

## Setup

Copy example configuration files:

```bash
cp vars/vars.yaml.example vars/vars.yaml
cp vars/vault.yaml.example vars/vault.yaml
```

Edit `vars/vars.yaml` with your domains and preferences.

Edit `vars/vault.yaml` with your Cloudflare API token and email. Encrypt it:

```bash
ansible-vault encrypt vars/vault.yaml
```

Run the playbook:

```bash
ansible-playbook letsencrypt.yaml --ask-vault-pass
```

## Certificate Output

Certificates are stored in `certs/{domain}/{name}/`:

- `cert.pem` - Server certificate
- `chain.pem` - Intermediate certificates
- `fullchain.pem` - Certificate and chain combined
- `privatekey.pem` - Private key (password protected)
- `certificate.pfx` - PKCS#12 format (standard)
- `certificate-legacy.pfx` - PKCS#12 format (legacy compatibility)
- `passphrase.txt` - Private key password

## Requirements

- Ansible with `community.crypto` and `community.general` collections
- Cloudflare account with API token
- Domain DNS managed by Cloudflare

## License

GNU Affero General Public License v3.0
