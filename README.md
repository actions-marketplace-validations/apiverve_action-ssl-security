# APIVerve SSL &amp; Security Action

> Monitor SSL certificates, check TLS configuration, and detect security issues

> **Beta Release** - This action is in beta. We'd love your feedback! [Open an issue](https://github.com/apiverve/action-ssl-security/issues) if you encounter any problems.

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-SSL_%26_Security-blue?logo=github)](https://github.com/apiverve/action-ssl-security)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**[Browse All APIs](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=ssl-security)** | **[Get Free API Key](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=ssl-security)** | **[Documentation](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=ssl-security)**

---

## What does this action do?

This action provides access to APIVerve's SSL &amp; Security APIs directly in your GitHub workflows:

- Alert before SSL certificates expire
- Verify TLS configuration meets security standards
- Check if domains are flagged as phishing
- Verify IPs are not blacklisted

### Available APIs

| API | Description |
|-----|-------------|
| `sslchecker` | SSL Checker inspects a website&#x27;s SSL certificate. It returns the certificate details plus derived signals — whether it is currently valid or expired, how many days until it expires, whether it expires soon, and whether it is self-signed. |
| `tlschecker` | tlschecker API |
| `phishingcheck` | Phishing Domain Checker verifies whether a domain or URL appears in a comprehensive database of known phishing sites. Updated every 6 hours with 850,000+ active phishing domains. |
| `ipblacklistlookup` | IP Blacklist Lookup checks whether a given IP address appears on known malicious IP blocklists. Identifies both inbound threats (attackers, spammers) and outbound threats (C2 servers, malware hosts). |

---

## Quick Start

```yaml
- name: SSL &amp; Security
  uses: apiverve/action-ssl-security@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: sslchecker
    params: '{&quot;domain&quot;: &quot;example.com&quot;}'
```

---

## Setup

### 1. Get Your API Key

Sign up for a free account at [dashboard.apiverve.com/signup](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=ssl-security) and create an API key.

### 2. Add Secret to Repository

Go to your repository **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

- Name: `APIVERVE_KEY`
- Value: Your API key from the dashboard

### 3. Use in Workflow

```yaml
- name: SSL &amp; Security
  uses: apiverve/action-ssl-security@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: sslchecker
    params: '{"your": "parameters"}'
```

---

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `api_key` | Your APIVerve API key (or set `APIVERVE_API_KEY` env var) | Yes* | - |
| `api` | API to use: `sslchecker`, `tlschecker`, `phishingcheck`, `ipblacklistlookup` | No | `sslchecker` |
| `params` | JSON parameters for the API | No | `{}` |
| `output_file` | Path to save binary output (images, PDFs) | No | - |
| `format` | Response format: `json`, `yaml`, or `xml` | No | `json` |
| `fail_on_error` | Fail workflow if API returns error | No | `true` |

*\*API key is required but can be provided via input OR `APIVERVE_API_KEY` / `APIVERVE_KEY` environment variable.*

## Outputs

| Output | Description |
|--------|-------------|
| `result` | Full API response as JSON |
| `data` | The `data` field from response as JSON |
| `status` | API status (`ok` or `error`) |
| `file` | Path to downloaded file (if `output_file` was used) |

---

## Examples

### SSL Certificate Check

Check SSL certificate expiration and validity

```yaml
- name: SSL Certificate Check
  id: ssl-security-0
  uses: apiverve/action-ssl-security@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: sslchecker
    params: '{&quot;domain&quot;: &quot;example.com&quot;}'

- name: Use result
  run: echo "Result: ${{ steps.ssl-security-0.outputs.data }}"
```

### TLS Configuration

Analyze TLS/SSL configuration

```yaml
- name: TLS Configuration
  id: ssl-security-1
  uses: apiverve/action-ssl-security@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: tlschecker
    params: '{&quot;domain&quot;: &quot;example.com&quot;}'

- name: Use result
  run: echo "Result: ${{ steps.ssl-security-1.outputs.data }}"
```

### Security Check

Check if a domain is flagged as malicious

```yaml
- name: Security Check
  id: ssl-security-2
  uses: apiverve/action-ssl-security@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: phishingcheck
    params: '{&quot;domain&quot;: &quot;example.com&quot;}'

- name: Use result
  run: echo "Result: ${{ steps.ssl-security-2.outputs.data }}"
```


---

## Full Workflow Example

```yaml
name: SSL &amp; Security Workflow

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  ssl-security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run SSL &amp; Security
        id: result
        uses: apiverve/action-ssl-security@v1
        with:
          api_key: ${{ secrets.APIVERVE_KEY }}
          api: sslchecker
          params: '{&quot;domain&quot;: &quot;example.com&quot;}'

      - name: Show result
        run: |
          echo "Status: ${{ steps.result.outputs.status }}"
          echo "Data: ${{ steps.result.outputs.data }}"
```

---

## Related Actions

Looking for more APIVerve actions?

- [apiverve/action](https://github.com/apiverve/action) - Generic action for all 350+ APIs
- [apiverve/action-release-assets](https://github.com/apiverve/action-release-assets) - Generate QR codes, barcodes, and badges for your GitHub releases
- [apiverve/action-visual-testing](https://github.com/apiverve/action-visual-testing) - Capture screenshots and generate PDFs for visual regression testing and documentation
- [apiverve/action-dns-monitor](https://github.com/apiverve/action-dns-monitor) - Verify DNS configuration, check propagation, and validate DNSSEC after deployments

**[Browse all APIVerve Actions →](https://github.com/marketplace?query=apiverve)**

---

## Pricing

- **Free tier** - Get started with generous free limits
- **Pro plans** - Higher rate limits and priority support for production use

Check out [pricing details](https://apiverve.com/pricing?utm_source=github&utm_medium=action&utm_campaign=ssl-security).

---

## Resources

- **API Documentation**: [docs.apiverve.com](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=ssl-security)
- **API Marketplace**: [apiverve.com/marketplace](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=ssl-security)
- **Issues & Support**: [GitHub Issues](https://github.com/apiverve/action-ssl-security/issues)
- **Email**: support@apiverve.com

---

## License

MIT - see [LICENSE](LICENSE)

---

Built by [APIVerve](https://apiverve.com?utm_source=github&utm_medium=action&utm_campaign=ssl-security) - 350+ APIs for developers
