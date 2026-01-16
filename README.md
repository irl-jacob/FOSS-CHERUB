# FOSS-CHERUB

<div align="center">

<!-- TODO: Add project logo -->

[![GitHub stars](https://img.shields.io/github/stars/CipherSaber/FOSS-CHERUB?style=for-the-badge)](https://github.com/CipherSaber/FOSS-CHERUB/stargazers)

[![GitHub forks](https://img.shields.io/github/forks/CipherSaber/FOSS-CHERUB?style=for-the-badge)](https://github.com/CipherSaber/FOSS-CHERUB/network)

[![GitHub issues](https://img.shields.io/github/issues/CipherSaber/FOSS-CHERUB?style=for-the-badge)](https://github.com/CipherSaber/FOSS-CHERUB/issues)

**An advanced, open-source vulnerability detection and security analysis platform.**

</div>

## 📖 Overview

FOSS-CHERUB is a robust and extensible platform designed for comprehensive vulnerability detection and security analysis of open-source projects. It integrates multiple components to scan software, classify vulnerabilities, import and synchronize with public vulnerability databases (like NVD), and present insights through an interactive web dashboard. This platform empowers developers and security teams to proactively identify and mitigate security risks within their FOSS dependencies and codebases.

## ✨ Features

-   🎯 **Automated Vulnerability Scanning**: Scans codebases for known vulnerabilities using sophisticated detection logic.
-   🔒 **CVE/CVD Data Import & Sync**: Continuously imports and synchronizes with national and common vulnerability databases (e.g., NVD for CVEs, potentially other sources for CVDs) to ensure up-to-date threat intelligence.
-   🧠 **CWE Classification**: Utilizes a classifier to categorize detected weaknesses according to the Common Weakness Enumeration (CWE) standard, providing structured insights into vulnerability types.
-   📊 **Interactive Web Dashboard**: A user-friendly web interface (`dashboard.py`) for visualizing scan results, vulnerability trends, and detailed security reports.
-   ⚙️ **Database Integration**: Persists vulnerability data, scan results, and classification outcomes in a structured SQL database for robust data management and historical analysis.
-   🐳 **Containerized Deployment**: Leverages Docker and Docker Compose for easy setup, consistent development environments, and scalable deployment.
-   🐍 **Python-centric Ecosystem**: Built predominantly with Python, offering flexibility and leveraging a rich ecosystem of data analysis and security tools.

## 🖥️ Demo

A `sample.webm` video demonstrates core functionalities.
[Watch the sample video](https://github.com/CipherSaber/FOSS-CHERUB/raw/3a9a9ad1f40055ccfb74430eacef5c798a6148f4/sample.webm)

## 🛠️ Tech Stack

**Core:**

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)

**Web Application (Dashboard & API):**
*   **Frameworks**: Nextjs.

**Database:**

[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)

**DevOps:**

[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)

[![Docker Compose](https://img.shields.io/badge/Docker_Compose-000000?style=for-the-badge&logo=docker&logoColor=white)](https://docs.docker.com/compose/)

## 🚀 Quick Start

Follow these steps to get FOSS-CHERUB up and running on your local machine using Docker Compose.

### Prerequisites
-   **Docker Desktop** (or Docker Engine and Docker Compose installed)
-   **Git**

### Installation

1.  **Clone the repository**
    ```bash
    git clone https://github.com/CipherSaber/FOSS-CHERUB.git
    cd FOSS-CHERUB
    ```

2.  **Environment setup**
    Create a `.env` file in the root directory by copying the example (if one exists, otherwise create a new one):
    ```bash
    cp .env.example .env # If .env.example exists
    # Otherwise, create a .env file and add necessary variables.
    # For a typical setup, you might need:
    # DB_HOST=db
    # DB_NAME=cherub_db
    # DB_USER=cherub_user
    # DB_PASSWORD=your_secure_password
    # NVD_API_KEY=your_nvd_api_key_if_required # For nvd_sync.py
    ```
    *Note: The `.env` file is crucial for configuring database credentials and other settings. Ensure you use strong passwords for production.*

3.  **Build and start services with Docker Compose**
    This will build the necessary Docker images and start the PostgreSQL database and the FOSS-CHERUB application services.
    ```bash
    docker-compose up --build -d
    ```
    The `-d` flag runs the services in detached mode.

4.  **Initialize the Database**
    Run the database setup script to create the necessary schema and initial data:
    ```bash
    docker exec $(docker-compose ps -q backend) /bin/bash -c "sh /app/setup_database.sh"
    ```
    *Note: Replace `backend` with the actual service name of your Python application if it differs in `docker-compose.yml` (e.g., `app`). You may also need to adjust `/app/setup_database.sh` if the script path inside the container is different.*

    Alternatively, if `start-all.sh` handles initial setup:
    ```bash
    sh start-all.sh
    ```
    This script is designed to set up the database and start all components. Review its contents for specific instructions or dependencies.

5.  **Access the application**
    Once all services are running, the web dashboard should be accessible.
    Visit `http://localhost:[detected_port]` in your browser.
    *Common ports for Python web apps are 5000 or 8000. Check your `docker-compose.yml` or `dashboard.py` for the exact port.*

### Manual Installation (without Docker)

If you prefer to run the components directly:

1.  **Clone the repository**
    ```bash
    git clone https://github.com/CipherSaber/FOSS-CHERUB.git
    cd FOSS-CHERUB
    ```

2.  **Set up a Python virtual environment**
    ```bash
    python3 -m venv venv
    source venv/bin/activate
    ```

3.  **Install Python dependencies**
    *Note: A `requirements.txt` file is expected for managing dependencies. If not present, you'll need to install them manually based on the `.py` files.*
    ```bash
    # If requirements.txt exists in the root or a sub-directory (e.g., backend/)
    # pip install -r requirements.txt
    
    # Otherwise, install common dependencies manually:
    pip install Flask Plotly Dash psycopg2-binary scikit-learn requests pandas
    ```

4.  **Set up PostgreSQL Database**
    Ensure PostgreSQL is installed and running locally. Create a database and user:
    ```sql
    CREATE DATABASE cherub_db;
    CREATE USER cherub_user WITH PASSWORD 'your_secure_password';
    GRANT ALL PRIVILEGES ON DATABASE cherub_db TO cherub_user;
    ```

5.  **Initialize database schema**
    Run the `database_schema.sql` script against your PostgreSQL database.
    ```bash
    psql -h localhost -U cherub_user -d cherub_db -f database_schema.sql
    ```

6.  **Configure Environment Variables**
    Set environment variables for database connection (e.g., `DB_HOST`, `DB_NAME`, `DB_USER`, `DB_PASSWORD`) and any other necessary keys (e.g., `NVD_API_KEY`).

7.  **Run the Dashboard and other services**
    ```bash
    python dashboard.py
    # If other services (e.g., scanner, API) are separate:
    # python foss_scanner.py
    # python api/main.py # Example, actual entry point may vary
    ```

## 📁 Project Structure

```
FOSS-CHERUB/
├── .gitignore               # Specifies intentionally untracked files to ignore
├── README.md                # Project documentation
├── api/                     # Contains backend API definitions and logic
├── backend/                 # Core backend services and modules
├── cwe_classifier.py        # Script for classifying Common Weakness Enumerations (CWEs)
├── dashboard.py             # Main entry point for the web-based dashboard
├── data_processing/         # Modules/scripts for data transformation and preparation
├── database/                # Database related scripts or initial data
├── database_schema.sql      # SQL script for database schema creation
├── db_connector.py          # Python module for database connection and operations
├── docker-compose.yml       # Docker Compose configuration for multi-service setup
├── foss-cherub-ui/          # (Potentially) Frontend UI assets or a separate frontend project
├── foss_scanner.py          # Core script for vulnerability scanning
├── import_cvd.py            # Script for importing Common Vulnerability Data
├── import_cve.py            # Script for importing Common Vulnerabilities and Exposures (CVE) data
├── list.py                  # Utility script (e.g., for listing data)
├── nvd_sync.py              # Script for synchronizing with the National Vulnerability Database (NVD)
├── sample.webm              # Demo video of the application
├── setup_database.sh        # Shell script for initial database setup
├── start-all.sh             # Shell script to start all components/services
└── vulnerability-detection-tool/ # Modules/plugins for vulnerability detection logic
```

## ⚙️ Configuration

### Environment Variables
FOSS-CHERUB relies on environment variables for sensitive information and configuration. It's recommended to use a `.env` file in the root directory.

| Variable        | Description                                     | Example                       | Required |

|-----------------|-------------------------------------------------|-------------------------------|----------|

| `DB_HOST`       | Hostname of the PostgreSQL database server      | `localhost` or `db`           | Yes      |

| `DB_NAME`       | Name of the database to connect to              | `cherub_db`                   | Yes      |

| `DB_USER`       | Username for database authentication            | `cherub_user`                 | Yes      |

| `DB_PASSWORD`   | Password for the database user                  | `your_secure_password`        | Yes      |

| `NVD_API_KEY`   | API key for accessing the NVD vulnerability data | `YOUR_API_KEY_HERE` (optional) | No       |

| `APP_PORT`      | Port for the web dashboard (if configurable)    | `5000`                        | No       |

| `LOG_LEVEL`     | Logging level for the application               | `INFO`, `DEBUG`, `WARNING`    | No       |

### Configuration Files
-   **`docker-compose.yml`**: Defines the services, networks, and volumes for the Docker-based deployment. Modify this file to adjust service configurations, ports, or add new services.

## 🔧 Development

### Available Scripts
The project uses various Python scripts for its core functionalities:

| Script                 | Description                                                        |

|------------------------|--------------------------------------------------------------------|

| `dashboard.py`         | Starts the main web dashboard application.                         |

| `foss_scanner.py`      | Executes the vulnerability scanning process.                       |

| `import_cve.py`        | Imports CVE data from specified sources into the database.         |

| `import_cvd.py`        | Imports general CVD data into the database.                        |

| `nvd_sync.py`          | Synchronizes vulnerability data with the National Vulnerability Database (NVD). |

| `cwe_classifier.py`    | Runs the CWE classification logic on detected weaknesses.          |

| `db_connector.py`      | Contains reusable functions for database interaction.              |

| `list.py`              | Utility script for listing various types of data.                  |

Shell scripts are provided for orchestration:

| Script                 | Description                                                        |

|------------------------|--------------------------------------------------------------------|

| `setup_database.sh`    | Initializes the database schema using `database_schema.sql`.       |

| `start-all.sh`         | Comprehensive script to set up the database, and start all core services. |

### Development Workflow
1.  Ensure prerequisites are met (Python 3.x, PostgreSQL, Docker if using containerized development).
2.  Set up environment variables in a `.env` file.
3.  For Docker-based development, use `docker-compose up` to start services.
4.  For local Python development, activate your virtual environment (`source venv/bin/activate`) and run individual Python scripts.
5.  Database schema changes require updating `database_schema.sql` and re-running `setup_database.sh` (or specific migration commands if an ORM with migrations is introduced).

## 🧪 Testing

While explicit testing framework configurations were not provided, Python projects typically use `pytest` or `unittest`.

To run tests (assuming `pytest`):
```bash

# If tests are structured in a 'tests/' directory

# pip install pytest

# pytest
```
Please consult `CONTRIBUTING.md` (if available) or the project maintainers for specific testing guidelines.

## 🚀 Deployment

The recommended deployment method is using Docker Compose, which provides a consistent and isolated environment.

### Production Build
The `docker-compose.yml` is suitable for both development and production. For production, ensure:
-   Environment variables (`.env`) are securely configured.
-   Appropriate logging is enabled.
-   Resource limits are set in `docker-compose.yml` if needed.

### Deployment Options
-   **Docker Compose**: The primary deployment method. Use `docker-compose up -d` on your production server.
-   **Kubernetes**: For larger scale deployments, the `docker-compose.yml` can be a starting point for converting to Kubernetes manifests.

## 📚 API Reference

The `api/` directory suggests a dedicated API component. While exact endpoints are not provided in the directory structure, based on the project's purpose, a typical API might include:

### Authentication
(Details to be provided in the API implementation)

### Endpoints (Inferred Examples)

#### `GET /api/vulnerabilities`
Retrieves a list of all detected vulnerabilities.
-   **Parameters**:
    -   `severity`: (Query, string, optional) Filter by severity (e.g., `CRITICAL`, `HIGH`, `MEDIUM`).
    -   `cve_id`: (Query, string, optional) Filter by specific CVE ID.
    -   `limit`: (Query, integer, optional) Maximum number of results to return.
-   **Responses**:
    -   `200 OK`: `[{ "id": "uuid", "cve_id": "CVE-XXXX-XXXX", "description": "...", "severity": "HIGH", ... }]`

#### `GET /api/vulnerabilities/{vulnerability_id}`
Retrieves details for a specific vulnerability.
-   **Parameters**:
    -   `vulnerability_id`: (Path, string, required) The unique identifier of the vulnerability.
-   **Responses**:
    -   `200 OK`: `{ "id": "uuid", "cve_id": "CVE-XXXX-XXXX", "description": "...", "severity": "HIGH", "recommendations": "...", ... }`
    -   `404 Not Found`: If the vulnerability does not exist.

#### `POST /api/scan`
Initiates a new vulnerability scan for a given codebase or repository.
-   **Request Body**:
    -   `repository_url`: (string, required) URL of the repository to scan.
    -   `branch`: (string, optional) Branch to scan (defaults to `main`).
-   **Responses**:
    -   `202 Accepted`: `{ "scan_id": "uuid", "status": "queued" }`
    -   `400 Bad Request`: If input is invalid.

#### `GET /api/scans/{scan_id}`
Retrieves the status and results of a specific scan.
-   **Parameters**:
    -   `scan_id`: (Path, string, required) The unique identifier of the scan.
-   **Responses**:
    -   `200 OK`: `{ "scan_id": "uuid", "status": "completed", "results_summary": { ... }, "vulnerabilities": [...] }`
    -   `404 Not Found`: If the scan does not exist.

## 🤝 Contributing

We welcome contributions to FOSS-CHERUB! Please consider contributing to help make this platform even better.

### Development Setup for Contributors
Refer to the "Quick Start" section for detailed setup instructions. We recommend using Docker Compose for a consistent development environment.

### Building and Testing
Follow the "Development" and "Testing" sections for guidance on running scripts and tests.

Please check for a `CONTRIBUTING.md` file in the repository for specific guidelines on submitting issues, pull requests, and coding standards.

## 🙏 Acknowledgments

-   **NVD (National Vulnerability Database)** for providing comprehensive vulnerability data, critical for `nvd_sync.py` and import scripts.
-   **Common Weakness Enumeration (CWE)** for the classification standard used in `cwe_classifier.py`.
-   All core Python libraries and their maintainers that make this project possible.

## 📞 Support & Contact

-   🐛 **Issues**: For bugs, feature requests, or questions, please use the [GitHub Issues](https://github.com/CipherSaber/FOSS-CHERUB/issues) page.

---

<div align="center">

**⭐ Star this repo if you find it helpful!**

Made with ❤️ by [CipherSaber](https://github.com/CipherSaber)

</div>


---
<img width="1810" height="940" alt="Screenshot from 2025-12-13 19-45-39" src="https://github.com/user-attachments/assets/4a0c8a5b-e263-49bc-b0c3-82396e7ff759" />
<img width="1811" height="938" alt="Screenshot from 2025-12-13 18-39-36" src="https://github.com/user-attachments/assets/0458e013-d71c-40ab-8bdc-0344174f3ff0" />

