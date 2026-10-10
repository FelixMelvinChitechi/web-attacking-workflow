# Ars0n Framework v2 — Web Attacking Workflow

## Getting Started

Once you have the Ars0n Framework v2 running, you'll be presented with a welcome screen that offers several options to begin your bug bounty hunting journey.

---

## Option 1: Create a New Scope Target

> **Best for:** Starting fresh reconnaissance on a new target

### Choose Target Type

| Type | Description | Example |
|---|---|---|
| **Company** | Any asset owned by an organization | `Google` |
| **Wildcard** | Any subdomain under the root domain | `*.google.com` |
| **URL** | Specific attack vector targeting a single domain | `https://hackme.google.com` |

### Enter Target Information

- **Company targets:** Enter the company name
- **Wildcard targets:** Enter the wildcard domain pattern
- **URL targets:** Enter the specific URL

### Begin Reconnaissance

The framework will automatically start gathering intelligence about your target. You can then run individual tools or use the **Auto Scan** feature.

---

## Option 2: Import Existing Scan Data

> **Best for:** Learning from pre-scanned data or resuming previous sessions

### Import from File

1. Download a `.rs0n` file (like those available in the scan data repository)
2. Click **"Import Scan Data"** in the welcome screen
3. Select **"Upload File"** and choose your `.rs0n` file
4. The framework will import all scope targets and associated scan results

### Import from URL

1. Click **"Import Scan Data"** in the welcome screen
2. Select **"Import from URL"**
3. Enter the raw GitHub URL of a `.rs0n` file:

```
https://github.com/R-s0n/ars0n-framework-v2-scan-data/raw/refs/heads/main/Grammarly/rs0n-export-2025-07-27T18-19-17.rs0n
```

4. The framework will download and import the data automatically

---

## Option 3: Use Pre-Scanned Data for Learning

> **Best for:** Understanding the bug bounty hunting process without running scans

The **Ars0n Framework v2 Scan Data Repository** contains real-world scan data that you can import to learn:

- **Subdomain Discovery** — How various tools find subdomains and assets
- **Vulnerability Assessment** — Common security issues and how they're identified
- **Attack Surface Mapping** — Understanding an organization's digital footprint
- **Reconnaissance Methodology** — The systematic approach to bug bounty hunting

---

## Workflows

The Ars0n Framework v2 supports three distinct workflows, each designed for different reconnaissance scenarios and objectives. Each workflow follows rs0n's proven bug bounty methodology and automatically guides users through the correct sequence of tools and techniques.

---

### 1. Company Workflow

**Objective:** Discover and map all digital assets owned by an organization to build a comprehensive attack surface for security testing.

> *"What does this organization's entire digital footprint look like?"*

#### Key Steps

**Step 1 — ASN (On-Prem) Network Ranges**
- **Tools:** Amass Intel, Metabigor
- **Purpose:** Intelligence gathering and ASN enumeration for comprehensive network range discovery
- **How:** Uses company name to discover Autonomous System Numbers (ASNs) and associated IP network ranges through WHOIS data and BGP routing information

**Step 2 — Discover Live Web Servers (On-Prem)**
- **Tools:** IP/Port scanning and Live Web Server detection
- **Purpose:** Process discovered network ranges to identify live IP addresses and active web servers
- **How:** Port scans network ranges and validates HTTP/HTTPS services within the organization's infrastructure

**Step 3 — Root Domain Discovery (No API Key)**
- **Tools:** Google Dorking, CRT (Certificate Transparency), Reverse Whois
- **Purpose:** Discover company domains and subdomains using publicly available information
- **How:** Advanced Google search techniques, certificate transparency logs analysis, and reverse WHOIS lookups

**Step 4 — Root Domain Discovery (API Key)**
- **Tools:** SecurityTrails, GitHub Recon Tools, Shodan CLI/API, Censys
- **Purpose:** Comprehensive domain discovery using commercial APIs and databases
- **How:** Leverages premium DNS/domain data providers, searches GitHub repositories for organization mentions, and queries internet scanning databases

**Step 5 — Consolidate Root Domains**
- **Tools:** Domain consolidation engine
- **Purpose:** Create a unified list of unique root domains and prepare for subdomain enumeration
- **How:** Deduplicates discovered domains and automatically creates Wildcard targets for comprehensive subdomain discovery

**Step 6 — Cloud Asset Enumeration (DNS)**
- **Tools:** Amass Enum, DNSx
- **Purpose:** Advanced DNS enumeration and cloud asset discovery across all discovered domains
- **How:** DNS brute-forcing, certificate transparency analysis, and comprehensive DNS record discovery

> **Result:** Complete visibility into the company's attack surface, enabling security teams to explore all discovered targets and conduct comprehensive Nuclei scanning against live assets.

---

### 2. Wildcard Workflow

**Objective:** Comprehensively enumerate all subdomains under a root domain and prioritize the most valuable targets for manual testing.

#### Key Steps

**Step 1 — Amass Enum**
- **Tools:** Amass Enum
- **Purpose:** Powerful subdomain enumeration and OSINT tool for in-depth reconnaissance
- **How:** Active and passive techniques including DNS brute-forcing, certificate transparency, and multiple data sources

**Step 2 — Subdomain Scraping**
- **Tools:** Sublist3r, Assetfinder, GAU, CTL, Subfinder
- **Purpose:** Comprehensive subdomain discovery using various OSINT techniques and data sources
- **How:** Queries search engines, certificate databases, web archives (Wayback Machine, Common Crawl), and multiple subdomain databases

**Step 3 — Consolidate Subdomains & Discover Live Web Servers (Round 1)**
- **Tools:** Consolidation engine, Live Web Server detection
- **Purpose:** Remove duplicates and identify which subdomains are serving live content
- **How:** Deduplicates discovered subdomains and probes for HTTP/HTTPS responses

**Step 4 — Brute-Force**
- **Tools:** ShuffleDNS, CeWL
- **Purpose:** DNS brute-forcing with custom wordlists and mass DNS resolution
- **How:** Generates target-specific wordlists from discovered content and performs high-speed DNS brute-forcing

**Step 5 — Consolidate Subdomains & Discover Live Web Servers (Round 2)**
- **Tools:** Consolidation engine, Live Web Server detection
- **Purpose:** Process new brute-force discoveries and validate accessibility
- **How:** Incorporates newly discovered subdomains and confirms they're serving live content

**Step 6 — JavaScript/Link Discovery**
- **Tools:** GoSpider, Subdomainizer
- **Purpose:** Web scraping and crawling to discover hidden endpoints and JavaScript-embedded subdomains
- **How:** Crawls live web applications and analyzes JavaScript files for embedded URLs, API endpoints, and additional subdomains

**Step 7 — Consolidate Subdomains & Discover Live Web Servers (Round 3)**
- **Tools:** Consolidation engine, Live Web Server detection
- **Purpose:** Final validation of all discovered assets from JavaScript/link analysis

**Step 8 — Decision Point: ROI Analysis**
- **Tools:** Screenshot capture, Metadata analysis
- **Purpose:** Gather intelligence to identify targets with the greatest ROI for bug bounty hunting

#### ROI Scoring Algorithm

Based on rs0n's years of bug bounty hunting experience, each target receives an **ROI (Return on Investment)** score:

| Factor | Points |
|---|---|
| SSL/TLS Issues (expired, self-signed, misconfigured) | +25 each |
| Content Richness (discovered endpoints) | +1–3 per endpoint |
| Technology Stack (known-vulnerable technologies) | +3 per technology |
| Missing Security Headers (e.g. CSP) | +10 |
| Application Complexity (caching, dynamic content) | +10 |
| Error Pages (404s revealing information) | +50 |

> 💡 **Tip:** Focus on targets with scores above **100** for the best time investment.

#### MetaData Intelligence

The MetaData step provides detailed intelligence including:

- **Technologies Used** — Frameworks, CMS platforms, server software, third-party services
- **Security Headers** — Analysis of HTTP headers and configurations
- **SSL/TLS Information** — Certificate details and potential vulnerabilities
- **DNS Records** — Complete DNS record analysis for each subdomain
- **HTTP Response Analysis** — Status codes, content types, response characteristics
- **Architecture Insights** — Server configurations and application stack information

---

### 3. URL Workflow

**Objective:** Educational workflow that teaches manual bug bounty hunting techniques through dynamic, personalized lesson plans built specifically for your target.

> ⚠️ **Status:** Currently under development.

#### Educational Philosophy

The URL workflow bridges the gap between theoretical knowledge and practical application by providing guided, hands-on learning experiences with real targets. Each module follows the **"Discover → Understand → Test → Validate"** approach:

1. **Discover** — Automated tools find potential vulnerability locations
2. **Understand** — Educational content explains the vulnerability class and impact
3. **Test** — Guided manual testing with target-specific examples and payloads
4. **Validate** — Automated verification tools confirm findings and reinforce learning

#### Planned Educational Modules

**Client-Side Injection Testing**
- XSS discovery, classification (stored vs. reflected), payload walkthrough, and automated validation

**Server-Side Vulnerability Assessment**
- SQL Injection, Command Injection, File Upload vulnerabilities

**Authentication and Authorization**
- Session Management, Access Control, MFA Bypass techniques

**Business Logic Vulnerabilities**
- Workflow Manipulation, Parameter Tampering, Race Conditions

**Advanced Techniques**
- CSRF and SSRF, XXE and Deserialization, API Security (REST/GraphQL)

> 💡 **Current Alternative:** While the URL workflow is in development, use the Wildcard workflow to discover targets, then apply manual testing techniques learned through external resources.

---

## MCP Server (AI Assistant Integration)

The framework ships with a **Model Context Protocol (MCP) server** that exposes its data and scan controls to AI assistants (Claude Desktop, Claude Code, Cursor, and other MCP-capable clients).

### What It Exposes

**154 tools across 12 categories:**

| Category | Tools | Description |
|---|---|---|
| Scope and targets | 9 | List, add, activate, delete scope targets; manage scope rules |
| Scan execution and workflows | 11 | Run tools, check/cancel scans, drive auto-scan sessions |
| Recon data queries | 21 | Subdomains, domains, network ranges, live servers, URLs, DNS |
| Bug bounty analysis | 13 | Takeover candidates, exposed panels, API endpoints, sensitive files |
| Endpoints and attack surface | 12 | Consolidate endpoints, manage attack vectors, manual crawl |
| Request flow replay | 8 | Repeater, flow reconstruction, active flow detection |
| Vulnerability scanning | 15 | XSS, SQLi, command injection, SSTI, LFI, SSRF, GraphQL, fuzzing |
| Authentication and authorization | 23 | Register/login/MFA/reset flows, session tokens, role/policy models |
| Target behaviour probe | 9 | WAF and rate-limit probe configuration and execution |
| Threat model and notes | 3 | STRIDE threat model entries and notes |
| Methodology and guidance | 10 | Built-in methodology, knowledge base, attack playbooks |
| Settings and configuration | 20 | Rate limits, HTTP config, Burp Suite, API keys, wordlists |

### Connecting

The MCP server starts automatically with `docker-compose up` and listens on **port 3001** via SSE transport:

```
SSE endpoint:   http://localhost:3001/sse
Health check:   http://localhost:3001/health
```

Verify it's up:

```bash
curl http://localhost:3001/health
# → {"status":"ok","version":"2.0.0","tools":154}
```

**Claude Desktop (stdio bridge):**

```json
{
  "mcpServers": {
    "ars0n-framework": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "http://localhost:3001/sse"]
    }
  }
}
```

**Claude Code (CLI):**

```bash
claude mcp add --transport sse ars0n-framework http://localhost:3001/sse
```

### Authentication (Optional)

Set `MCP_AUTH_TOKEN` in `docker-compose.yml` for the `mcp-server` service:

```yaml
mcp-server:
  environment:
    MCP_AUTH_TOKEN: "your-secret-token"
```

Supply the token as `Authorization: Bearer your-secret-token` or `?token=your-secret-token` on the SSE URL.

---

## Remote Access

### Accessing the Framework

| Location | URL |
|---|---|
| Same machine | `http://localhost` |
| Another machine on your network | `http://<server-ip>` |

### Finding Your Server IP

```bash
# Linux/Mac
hostname -I

# Windows
ipconfig
```

### Firewall Configuration

```bash
# Linux (Ubuntu/Debian)
sudo ufw allow 80/tcp

# Linux (CentOS/RHEL)
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --reload
```

```powershell
# Windows (PowerShell as Administrator)
New-NetFirewallRule -DisplayName "Ars0n Framework" -Direction Inbound -Protocol TCP -LocalPort 80 -Action Allow
```

### Custom Port

Edit `docker-compose.yml`:

```yaml
nginx:
  ports:
    - "8080:80"
```

Then access at `http://<server-ip>:8080`.

---

## Troubleshooting

### Docker Not Installed

```bash
# Linux (Ubuntu/Debian)
sudo apt update
sudo apt install docker.io docker-compose
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
```

### Verify Docker Installation

```bash
docker run hello-world
```

### Common Errors & Fixes

| Error | Solution |
|---|---|
| `Cannot connect to the Docker daemon` | Start Docker Desktop (Win/Mac) or `sudo systemctl start docker` (Linux) |
| `permission denied` on Docker socket | `sudo usermod -aG docker $USER && newgrp docker` |
| `port is already allocated` | Check `sudo netstat -tulpn \| grep :80` and stop conflicting service |
| `no space left on device` | `docker system prune -a && docker volume prune` |
| `connection refused` on database | Wait 30+ seconds; check `docker logs ars0n-framework-v2-db-1` |
| Cannot connect from another machine | Check firewall, verify `docker-compose ps`, check nginx logs |

### Windows-Specific

```powershell
# Enable WSL 2
wsl --install

# Enable Hyper-V
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

### Check Container Logs

```bash
docker-compose logs [service-name]
docker logs ars0n-framework-v2-nginx-1
docker logs ars0n-framework-v2-db-1
```

### Clean Rebuild

```bash
docker builder prune
docker-compose build --no-cache
```

---

## Getting Help

### When to Use Discussions vs Issues

| Use **Discussions** for | Use **Issues** for |
|---|---|
| Installation and setup problems | Bugs in the tool itself |
| Docker configuration issues | Feature requests |
| Environment-specific problems | Security vulnerabilities |
| Questions about getting the tool running | Problems with tool functionality |

### Discussion Template

```markdown
## System Information
- Operating System:
- Docker Version: [docker --version]
- Docker Compose Version: [docker-compose --version]
- Available RAM:
- Available Disk Space:

## Error Details
- Complete error message(s):
- Step where the error occurred:

## Troubleshooting Steps Taken
- [ ] Verified Docker with `docker run hello-world`
- [ ] Checked container logs: `docker-compose logs [service-name]`
- [ ] Verified system meets minimum requirements
- [ ] Tried: `docker-compose up --build --verbose`

## Container Logs
[Paste output here]
```

> ⚠️ **Always provide complete logs.** Partial information significantly delays response times.

---

## FAQ

**Q: What makes this different from other bug bounty tools?**
A: It's fundamentally an educational tool designed to help people *earn while they learn*. The UI enforces rs0n's proven methodology — making it literally impossible to use incorrectly.

**Q: Do I need to be an expert?**
A: No. The framework is specifically designed to help beginners start real bug bounty hunting on day one.

**Q: Is this tool free?**
A: Yes — completely free and open-source under the **GPL-3.0 license**. Some integrated tools (SecurityTrails, Censys, Shodan) may require paid API keys for extended usage.

**Q: Which workflow should I use?**

| Target | Workflow |
|---|---|
| Entire organization's assets | Company Workflow |
| All subdomains under a domain | Wildcard Workflow |
| Manual learning on a specific target | URL Workflow (in development) |

**Q: What is the ROI score?**
A: A score predicting how likely a target is to contain vulnerabilities. Focus on targets **above 100** for the best use of your time.

**Q: How long do scans take?**

| Scan Type | Estimated Time |
|---|---|
| Individual tool | 5–30 minutes |
| Wildcard scan | 30 minutes – several hours |
| Company scan | Hours to days |

---

*Documentation based on the Ars0n Framework v2 by R-s0n. Licensed under GPL-3.0.*
