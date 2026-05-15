```


 .d8888b.                     .d8888b.  888      888     
d88P  Y88b                   d88P  Y88b 888      888     
Y88b.                        888    888 888      888     
 "Y888b.    .d88b.   .d8888b 888        88888b.  888  888
    "Y88b. d8P  Y8b d88P"    888        888 "88b 888 .88P
      "888 88888888 888      888    888 888  888 888888K 
Y88b  d88P Y8b.     Y88b.    Y88b  d88P 888  888 888 "88b
 "Y8888P"   "Y8888   "Y8888P  "Y8888P"  888  888 888  888

```
# ⚡ SecChk version 0.2.0

<div align="center">

[![GitHub stars](https://img.shields.io/github/stars/jcanfield/SecChk?style=for-the-badge)](https://github.com/jcanfield/SecChk/stargazers)

[![GitHub forks](https://img.shields.io/github/forks/jcanfield/SecChk?style=for-the-badge)](https://github.com/jcanfield/SecChk/network)

[![GitHub issues](https://img.shields.io/github/issues/jcanfield/SecChk?style=for-the-badge)](https://github.com/jcanfield/SecChk/issues)

[![GitHub license](https://img.shields.io/github/license/jcanfield/SecChk?style=for-the-badge)](LICENSE)

**Command-line tool to scan and check projects/domains for security exploits and other behavior used for security hardening.**

</div>

## 📖 Overview

SecChk is a powerful command-line utility designed to assist developers, system administrators, and security professionals in assessing the security posture of their projects and domains. It automates various checks to identify potential security exploits, misconfigurations, and other vulnerabilities that could compromise system integrity or data privacy. By leveraging a comprehensive suite of checks, SecChk aims to streamline the security hardening process, making it easier to pinpoint and address weak points before they can be exploited.

## ✨ Features

-   **Comprehensive Scanning:** Scans projects and domains for a wide range of security-related issues.
-   **Vulnerability Detection:** Identifies common security exploits and insecure behaviors.
-   **Configuration Analysis:** Helps detect misconfigurations that could lead to vulnerabilities.
-   **Target Management:** Supports scanning multiple targets defined in a dedicated targets file.
-   **Environment-driven Configuration:** Customizable behavior through easy-to-manage environment variables.
-   **Security Hardening Guidance:** Provides insights to improve overall security posture.
-   **_Feature Requests_:** Please feel free to [request features](https://github.com/jcanfield/SecChk/issues) or offer constructive feedback.

## 🛠️ Tech Stack

**Primary Language:**

![Shell Script](https://img.shields.io/badge/Shell-121011?style=for-the-badge&logo=gnu-bash&logoColor=white)

**Configuration:**

![Dotenv](https://img.shields.io/badge/Dotenv-FAE03C?style=for-the-badge&logo=dotenv&logoColor=black)

## 🚀 Quick Start

### Prerequisites

-   A Unix-like operating system (Linux, macOS, WSL).
-   Basic command-line tools (e.g., `curl`, `dig`, `whois`, etc. - actual dependencies will depend on the script's internal workings).
-   nuclei v3.8.0
-   netcat v1.10

### Installation

1.  **Clone the repository**
    ```bash
    git clone https://github.com/jcanfield/SecChk.git
    cd SecChk
    ```

2.  **Make the script executable**
    ```bash
    chmod +x secchk
    ```

3.  **Environment setup**
    Copy the example environment file and configure it:
    ```bash
    cp .env .env.local # Or simply edit .env directly
    # Open .env.local and adjust variables as needed.
    ```
    **Note:** The `.env` file contains various configuration settings for the tool. Review its contents and set values according to your needs.

4.  **Prepare your targets file**
    Copy the example targets file:
    ```bash
    cp targets.txt.example targets.txt
    # Open targets.txt and add the domains/projects you want to scan, one per line.
    ```

5.  **Add to PATH (Optional)**
    To run `secchk` from any directory, add it to your system's PATH. For example:
    ```bash
    # For current session
    export PATH="$(pwd):$PATH"

    # For permanent access (add to your shell's config, e.g., ~/.bashrc or ~/.zshrc)
    echo 'export PATH="/path/to/SecChk:$PATH"' >> ~/.bashrc
    source ~/.bashrc
    ```

## 📖 Usage

### Basic Execution

After installation, you can run `secchk` directly. The tool is designed to read configuration from `.env` and targets from `targets.txt`.

```bash

# Run the scanner
./secchk
```

### Available Commands & Options

While a detailed list of commands and options would be derived from the script's internal parsing logic, typical usage for a security scanning CLI might include:

```bash

# Show help message
./secchk --help
./secchk -h

# Scan a specific target directly (if supported by script)
./secchk --target example.com

# Use a custom targets file (if supported by script)
./secchk --file my_custom_targets.txt

# Run a specific type of scan (e.g., "web" or "dns")
./secchk --scan-type web
```
*(TODO: Replace with actual commands and options by analyzing the `secchk` script and `PROJECT_NOTES.md` for flags and subcommands.)*

### Examples

```bash

# Example 1: Basic scan using configured targets.txt and .env

# Ensure targets.txt is populated with your desired targets (e.g., example.com)

# And .env is configured with any necessary API keys or settings.
./secchk

# Example 2: Run a dry run or verbose output (hypothetical options)
./secchk --verbose --dry-run
```

## 📁 Project Structure

```
SecChk/
├── .env                  # Environment variables for configuration
├── LICENSE               # Project license (MIT)
├── PROJECT_NOTES.md      # Detailed development notes, ideas, and internal documentation
├── README.md             # This README file
├── assets/               # Directory for potential static assets (e.g., future screenshots, logos)
├── secchk                # Main executable shell script for the CLI tool
└── targets.txt.example   # Example file for defining scan targets
```
### 📁 App Structure
```
secchk
├── .env loader          (lines 1–20)   — source config
├── Defaults             (lines 22–28)  — fallbacks if .env missing
├── Color variables      (lines 30–40)  — R, G, Y, B, C, W, DIM, RST
├── Utility functions    (lines 43–90)  — print_banner, info, ok, warn,
│                                          sep, heading, ask, confirm,
│                                          need, mk_outdir
├── run_nuclei()         (lines 93–115) — nuclei wrapper, all scans use this
│
├── do_scan1–6()         Nuclei scans
├── do_nc_common/range/banner()  Netcat
├── do_whois/dns/ssl/headers/shodan()  Tools
│
├── main_menu()          — the interactive loop, add new entries here
└── case "${1:-}"        — the --flag dispatcher at the bottom
```

## ⚙️ Configuration

### Environment Variables

The `.env` file is crucial for configuring SecChk's behavior. It allows you to customize various settings without modifying the main script.

| Variable | Description | Default | Required |

|----------|-------------|---------|----------|

| `DOMAIN_LIST` | Path to the file containing target domains (defaults to `targets.txt`) | `targets.txt` | Yes |

| `API_KEY_VIRUSTOTAL` | API key for VirusTotal integration (if used) | `(empty)` | No |

| `WHOIS_SERVER` | Custom WHOIS server to use | `whois.arin.net` | No |

| `NMAP_OPTIONS` | Custom options to pass to Nmap (if Nmap is integrated) | `"-F"` | No |

| `REPORT_FORMAT` | Output format for reports (e.g., `text`, `json`, `html`) | `text` | No |

| `LOG_LEVEL` | Level of logging detail (e.g., `INFO`, `DEBUG`, `ERROR`) | `INFO` | No |

| `EMAIL_ALERTS` | Enable/disable email alerts | `false` | No |

| `EMAIL_RECIPIENT` | Email address for alerts | `(empty)` | If `EMAIL_ALERTS=true` |

| `PROXY_SERVER` | Proxy server for requests (e.g., `http://127.0.0.1:8080`) | `(empty)` | No |

| `TIMEOUT_SECONDS` | Timeout for network requests in seconds | `30` | No |

| `EXCLUDE_IPS` | Comma-separated list of IPs/CIDRs to exclude from scans | `(empty)` | No |

| `MAX_CONCURRENT_SCANS` | Maximum number of concurrent scans | `5` | No |

| `USER_AGENT` | Custom User-Agent string for HTTP requests | `(default browser UA)` | No |

| `SSL_VERIFY` | Enable/disable SSL certificate verification for HTTP requests | `true` | No |

*(TODO: Review `.env` content and `PROJECT_NOTES.md` for a precise list of supported environment variables and their descriptions.)*

### Configuration Files

-   **`.env`**: Stores environment variables that control the script's behavior.
-   **`targets.txt`**: A plain text file where each line specifies a domain or project to be scanned.

## 🔧 Development

### Development Setup
To contribute or modify `secchk`, you primarily need a text editor and a shell environment.

1.  Clone the repository as described in the installation steps.
2.  Ensure the `secchk` script has executable permissions.
3.  Open `secchk` in your preferred text editor to view or modify its code.
4.  Consult `PROJECT_NOTES.md` for insights into the project's design and ongoing development.

### Running Tests
As a shell script, testing might involve running the script with various inputs and checking outputs.
*(TODO: If there are specific test scripts or a testing methodology outlined in `PROJECT_NOTES.md`, describe it here.)*

### Making Changes
Examples of adding new tools to the script.

1. Write the function (follow the same pattern):
```
do_nmap() {
    heading "nmap Service Scan"
    need nmap || return 1
    local h; h="$(ask "Target host" "$DEFAULT_HOST")"
    nmap -sV -sC --open "$h"
}

```
2. Add it to main_menu():
```
printf "  %s16%s  nmap service scan\n" "$C" "$RST"
# ...and in the case block:
16) do_nmap ;;
```

3. Add a --flag to the bottom case block:
```
--nmap) print_banner; do_nmap ;;
```
```

### Future menu structure based on possible apps/tools
```
secchk
├── [1] VULNERABILITY SCANNING (nuclei)
│   ├── Basic CVE scan
│   ├── Critical/high severity
│   ├── Network range scan
│   ├── Linux/infra full scan
│   ├── Windows 11 (SMB/RPC/web)
│   └── Full audit to file
│
├── [2] NETWORK TOOLS
│   ├── Port scan — common ports (nc)
│   ├── Port scan — custom range (nc)
│   ├── Banner grab (nc)
│   ├── DNS enumeration (dig)
│   ├── WHOIS lookup
│   ├── Ping sweep (fping/ping)
│   ├── Traceroute
│   └── SSL/TLS check (openssl)
│
├── [3] WEB / HTTP TOOLS
│   ├── HTTP security headers (curl)
│   ├── Nikto web server scan
│   ├── Directory brute-force (gobuster/ffuf)
│   └── Shodan lookup
│
├── [4] DOCKER SECURITY
│   ├── Scan image for CVEs (trivy)
│   ├── Scan running containers (trivy)
│   ├── Scan local project/Dockerfile (trivy)
│   ├── Docker bench security (docker-bench-security)
│   ├── List images + check for outdated
│   └── Inspect container network exposure
│
├── [5] SYSTEM / BLUE TEAM
│   ├── Open ports on THIS host (ss/netstat)
│   ├── Active connections
│   ├── Failed SSH logins (auth.log)
│   ├── Listening services
│   ├── Check for SUID binaries
│   └── System information (sysinfo)
│
└── [6] UTILITY
    ├── Configuration
    ├── View scan results
    └── Help / Quit
```


## 🤝 Contributing

We welcome contributions to improve SecChk! Please consider the following:

-   **Bug Reports:** If you find a bug, please open an issue with a clear description and steps to reproduce.
-   **Feature Requests:** Suggest new features or enhancements by opening an issue.
-   **Pull Requests:** Feel free to fork the repository, make your changes, and submit a pull request. Please ensure your code adheres to the existing style and includes relevant documentation or comments.

## 📄 License

This project is licensed under the [MIT License](LICENSE) - see the LICENSE file for details.

## 🙏 Acknowledgments

-   Inspired by the need for robust and automated security checks.
-   Built leveraging standard Unix-like command-line utilities.
-   This project was coded by a human with help from an AI/LLM [Crush](https://github.com/charmbracelet/crush).
-   The documentation was generated by [README Studio](https://readmestudio.zenui.net/editor).

## 📞 Support & Contact

-   🐛 Issues: [GitHub Issues](https://github.com/jcanfield/SecChk/issues)
-   📧 Contact: [TODO: Add a contact email, if desired by the author]

---

<div align="center">

**⭐ Star this repo if you find it helpful!**

Made with ❤️ by [jcanfield](https://github.com/jcanfield)

</div>

