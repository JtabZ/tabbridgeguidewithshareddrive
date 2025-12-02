# 🌉 Tableau Bridge Playbook

**Website:** [https://jtabz.github.io/tabbridgeguidewithshareddrive/](https://jtabz.github.io/tabbridgeguidewithshareddrive/)

## 📖 What Is This?

The **Tableau Bridge Playbook** is an interactive best-practices guide for deploying Tableau Bridge, specifically designed for connecting Tableau Cloud to on-premise **Shared Network Drives**.

Connecting cloud analytics to legacy file shares (Excel/CSV on a Z: drive) is a notorious point of failure for many organizations. This webpage serves as a definitive "Runbook" to architect this correctly, avoiding common errors like file locking, permission issues, and pathing problems.

## 🎯 Purpose & Goals

The guide aims to standardize the deployment of Tableau Bridge to ensure **High Availability (HA)** and **Reliability**.

* **Solve the "It works on my desktop but fails on the server" problem:** By explaining UNC paths vs. Mapped Drives.
* **Prevent Downtime:** By advocating for Service Mode and Pooling.
* **Secure the Pipeline:** By defining the exact permissions required for the Service Account.

## ⚙️ Core Technical Concepts

### 1. Architecture: Service Mode & Pooling
* **Mandatory:** Use **Service Mode** (Windows Service) so refreshes run even when no user is logged in.
* **HA:** Deploy **Client Pools** (logical groups of 2+ machines) to prevent single points of failure.

### 2. Configuration: The "Golden Rules"
* **UNC Paths Only:** Never use mapped drives (`Z:\data.csv`). Always use Universal Naming Convention paths (`\\ServerName\Share\data.csv`).
* **Private Network Allowlist:** You must explicitly whitelist the internal file server hostname in Tableau Cloud settings.

### 3. Security: Least Privilege
* **Bridge Host:** The service account *must* be a Local Administrator.
* **File Share:** The service account should have **Read & Execute** only. (No Write access to prevent accidental data corruption).

### 4. Troubleshooting: File Locking
* **The Problem:** If a user has the Excel file open, Bridge cannot read it.
* **The Solution:** Use a "Staging" folder where scripts copy the data for Bridge to read, isolating it from human interference.

## 👥 Who Is This For?

* **Tableau Server/Cloud Administrators:** To properly set up the Bridge environment.
* **IT/Infrastructure Teams:** To understand the firewall and service account requirements.
* **Data Engineers:** To understand why their file-based refreshes are failing.

## 🚀 Key Takeaway
*"The single most important decision is the operational mode. For any enterprise use case involving shared drives, Service Mode is the only recommended option."*
