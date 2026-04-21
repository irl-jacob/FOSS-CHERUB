<h1 align="center">FOSS-CHERUB</h1>

<p align="center">
  <strong>AI-powered vulnerability scanner with AST analysis for open-source projects</strong>
</p>

<p align="center">
  <a href="https://github.com/irl-jacob/FOSS-CHERUB/commits/main">
    <img src="https://img.shields.io/github/last-commit/irl-jacob/FOSS-CHERUB?style=flat-square" alt="Last Commit">
  </a>
  <a href="https://github.com/irl-jacob/FOSS-CHERUB/stargazers">
    <img src="https://img.shields.io/github/stars/irl-jacob/FOSS-CHERUB?style=flat-square" alt="Stars">
  </a>
  <a href="https://github.com/irl-jacob/FOSS-CHERUB/issues">
    <img src="https://img.shields.io/github/issues/irl-jacob/FOSS-CHERUB?style=flat-square" alt="Issues">
  </a>
  <img src="https://img.shields.io/badge/python-3.8+-blue?style=flat-square&logo=python" alt="Python">
</p>

<p align="center">
  Proactively identify and mitigate security risks in your FOSS dependencies and codebases using advanced LLM-powered analysis and Abstract Syntax Tree inspection.
</p>

---

## Why FOSS-CHERUB?

Traditional vulnerability scanners rely on signature-based detection and miss context-aware security issues. FOSS-CHERUB combines:

- **LLM-powered analysis** using fine-tuned Qwen2.5-Coder models for intelligent vulnerability detection
- **AST-based taint tracking** across Python, JavaScript, Java, and C/C++ for precise data flow analysis
- **Real-time NVD synchronization** to stay current with the latest CVE disclosures
- **Actionable remediation** with code quality improvements, safer alternatives, and compliance mapping

Perfect for security teams who need more than pattern matching and developers who want to ship secure code faster.

---

## Demo

https://github.com/user-attachments/assets/e47e007d-2e34-4a18-92a1-ef3fc02388aa

<details>
<summary>View Screenshots</summary>

<img width="1810" alt="Dashboard Overview" src="https://github.com/user-attachments/assets/4a0c8a5b-e263-49bc-b0c3-82396e7ff759" />
<img width="1811" alt="Vulnerability Analysis" src="https://github.com/user-attachments/assets/0458e013-d71c-40ab-8bdc-0344174f3ff0" />

</details>

---

## Features

| Feature | Description |
|---------|-------------|
| **AI-Powered Scanning** | Fine-tuned Qwen2.5-Coder model detects context-aware vulnerabilities |
| **Multi-Language AST** | Taint analysis for Python, JavaScript, Java, C/C++ with tree-sitter |
| **CVE/CWE Integration** | Real-time NVD sync and automatic CWE classification |
| **Code Quality Analysis** | Complexity metrics, refactoring suggestions, and maintainability scoring |
| **Compliance Mapping** | OWASP Top 10, PCI DSS, SOC 2 violation detection |
| **Interactive Dashboard** | Next.js + React UI with FastAPI backend for real-time insights |
| **Remediation Priority** | Intelligent scoring based on severity, exploitability, and business impact |
| **Docker Ready** | One-command deployment with PostgreSQL persistence |


---

## Tech Stack

**Backend:** Python 3.8+ | FastAPI | PostgreSQL | Transformers (Qwen2.5-Coder) | tree-sitter  
**Frontend:** Next.js | React | TypeScript  
**DevOps:** Docker | Docker Compose  
**Analysis:** NumPy | Pandas | Pydantic | AST parsers
---

## Quick Start

**Prerequisites:** Docker and Git

```bash
# Clone and start
git clone https://github.com/irl-jacob/FOSS-CHERUB.git
cd FOSS-CHERUB
sh start-all.sh

# Access the application
# Frontend: http://localhost:3002
# Backend:  http://localhost:8082
```

**Using Docker Compose:**
```bash
docker-compose up -d
```

**Pre-built Images:**
- Application: [jacpacd/mal-ai](https://hub.docker.com/r/jacpacd/mal-ai)
- Database: [jacpacd/postgres](https://hub.docker.com/r/jacpacd/postgres)

**Fine-tuned Model:**
- [Qwen2.5-Coder:7B on Hugging Face](https://huggingface.co/jacpacd/Foss-Cherub-Vuln-Detector)

---

## Configuration

Create a `.env` file in the root directory:

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `DB_HOST` | PostgreSQL hostname | `localhost` | Yes |
| `DB_NAME` | Database name | `foss_cherub` | Yes |
| `DB_USER` | Database user | `postgres` | Yes |
| `DB_PASSWORD` | Database password | `foss_cherub_2024` | Yes |
| `NVD_API_KEY` | NVD API key for CVE sync | - | No |
| `APP_PORT` | Dashboard port | `3002` | No |
| `LOG_LEVEL` | Logging level | `INFO` | No |

---

## Project Structure

<details>
<summary>View directory structure</summary>

```
FOSS-CHERUB/
├── api/                          # FastAPI backend endpoints
├── backend/                      # Core backend services
├── foss-cherub-ui/              # Next.js frontend application
├── vulnerability-detection-tool/ # Detection modules and plugins
├── data_processing/             # Data transformation scripts
├── database/                    # Database initialization scripts
├── foss_scanner.py              # Main vulnerability scanner
├── cwe_classifier.py            # CWE classification engine
├── nvd_sync.py                  # NVD synchronization service
├── import_cve.py                # CVE data importer
├── import_cvd.py                # CVD data importer
├── dashboard.py                 # Dashboard entry point
├── db_connector.py              # Database operations
├── database_schema.sql          # Database schema definition
├── docker-compose.yml           # Docker orchestration
├── start-all.sh                 # Startup script
└── setup_database.sh            # Database initialization
```

</details>

---

## Development

**Core Scripts:**

| Script | Purpose |
|--------|---------|
| `foss_scanner.py` | Main vulnerability scanner with AST analysis |
| `dashboard.py` | Web dashboard entry point |
| `nvd_sync.py` | Sync CVE data from NVD |
| `cwe_classifier.py` | CWE classification engine |
| `import_cve.py` / `import_cvd.py` | CVE/CVD data importers |

**Local Development:**
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python dashboard.py
```

**Testing:**
```bash
pip install pytest
pytest
```

For detailed development guidelines, see [ENHANCED_FEATURES.md](vulnerability-detection-tool/ENHANCED_FEATURES.md).

---

## Deployment

**Docker Compose (Recommended):**
```bash
docker-compose up -d
```

**Production Checklist:**
- Secure `.env` with strong credentials
- Enable appropriate logging levels
- Set resource limits in `docker-compose.yml`
- Use HTTPS with reverse proxy (nginx/traefik)

**Kubernetes:** Convert `docker-compose.yml` to K8s manifests for scale.

---

## API Reference

The FastAPI backend exposes RESTful endpoints for vulnerability management and scanning operations.

**Base URL:** `http://localhost:8082`

**Key Endpoints:**
- `GET /api/vulnerabilities` - List detected vulnerabilities with filtering
- `GET /api/vulnerabilities/{id}` - Get vulnerability details
- `POST /api/scan` - Initiate new codebase scan
- `GET /api/scans/{id}` - Get scan status and results

For complete API documentation, visit `http://localhost:8082/docs` (Swagger UI) after starting the backend.

---

## Contributing

Contributions are welcome. To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -m 'Add improvement'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Open a Pull Request

Please ensure your code follows the existing style and includes appropriate tests.

---

## License

This project is open source. Please check the repository for license details.

---

## Acknowledgments

- **NVD** for comprehensive vulnerability data
- **CWE** for weakness classification standards
- **Qwen Team** for the base language model
- **tree-sitter** community for multi-language parsing support

---

## Support

- **Issues:** [GitHub Issues](https://github.com/irl-jacob/FOSS-CHERUB/issues)
- **Discussions:** [GitHub Discussions](https://github.com/irl-jacob/FOSS-CHERUB/discussions)

---

<div align="center">

**Star this repo if you find it helpful**

Built by [irl-jacob](https://github.com/irl-jacob)

</div>