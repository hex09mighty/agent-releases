# Employee Agent Installation Guide

## 1. Overview

Employee Agent is distributed as a platform-specific release package containing:

* Employee Agent binary
* Go-based installer
* Platform-specific configuration
* Platform-specific service configuration

Supported platforms:

* Linux
* Windows

Each release package contains the version defined in the corresponding platform configuration file.

---

# 2. Linux Installation

## 2.1 Requirements

Before installation, ensure:

* Linux `amd64` system
* `sudo` or root access
* `systemd` is available
* The release package has been downloaded

The Linux release package has the following structure:

```text
employee-agent-linux-amd64-<VERSION>.tar.gz

├── employee-agent
├── installer
├── configs/
│   └── config.yaml
└── deployments/
    └── systemd/
        └── employee-agent.service
```

## 2.2 Extract the Release

Extract the package:

```bash
tar -xzf employee-agent-linux-amd64-<VERSION>.tar.gz
```

Verify the files:

```bash
ls -la
```

You should see:

```text
employee-agent
installer
configs/
deployments/
```

## 2.3 Run the Installer

The installer must run with root privileges.

```bash
sudo ./installer
```

The installer will:

1. Check root privileges
2. Create `/opt/employee-agent`
3. Create `/opt/employee-agent/configs`
4. Copy the Employee Agent binary
5. Copy the configuration file
6. Install the systemd service
7. Reload systemd
8. Enable the service
9. Start/restart the service
10. Verify that the service is running

After successful installation, the application will be located at:

```text
/opt/employee-agent/
├── employee-agent
└── configs/
    └── config.yaml
```

The `data` and `logs` directories are created by the Employee Agent itself when it starts.

## 2.4 Check Service Status

Check the service:

```bash
sudo systemctl status employee-agent
```

For a concise status check:

```bash
sudo systemctl is-active employee-agent
```

Expected output:

```text
active
```

## 2.5 View Agent Logs

View recent service logs:

```bash
sudo journalctl -u employee-agent
```

Follow the logs in real time:

```bash
sudo journalctl -u employee-agent -f
```

View logs from the current boot:

```bash
sudo journalctl -u employee-agent -b
```

## 2.6 Restart the Agent

```bash
sudo systemctl restart employee-agent
```

## 2.7 Stop the Agent

```bash
sudo systemctl stop employee-agent
```

## 2.8 Start the Agent

```bash
sudo systemctl start employee-agent
```

## 2.9 Enable Agent at Boot

The installer automatically enables the service.

To verify:

```bash
sudo systemctl is-enabled employee-agent
```

Expected output:

```text
enabled
```

---

# 3. Windows Installation

## 3.1 Requirements

Before installation, ensure:

* Windows `amd64` system
* Administrator privileges
* The release package has been downloaded

The Windows release package contains:

```text
employee-agent-windows-amd64-<VERSION>.zip

├── employee-agent.exe
├── installer.exe
└── configs/
    └── config.yaml
```

## 3.2 Extract the Release

Extract the ZIP file to a temporary installation directory.

For example:

```text
C:\employee-agent-install\
```

The directory should contain:

```text
C:\employee-agent-install\
├── employee-agent.exe
├── installer.exe
└── configs\
    └── config.yaml
```

## 3.3 Run the Installer as Administrator

Open **Command Prompt** or **PowerShell as Administrator**.

Navigate to the extracted directory:

```powershell
cd C:\employee-agent-install
```

Run:

```powershell
.\installer.exe
```

The installer will:

1. Check Administrator privileges
2. Create:

```text
C:\Program Files\MonitorTrackAgent
```

3. Create the `configs`, `data`, and `logs` directories as required
4. Copy `employee-agent.exe`
5. Copy `config.yaml`
6. Register the Windows service
7. Configure the service to start automatically
8. Start the service
9. Verify the service is running

After installation:

```text
C:\Program Files\MonitorTrackAgent\
├── employee-agent.exe
├── configs\
│   └── config.yaml
├── data\
└── logs\
```

The runtime `data` and `logs` directories may be created by the Employee Agent during startup if they do not already exist.

## 3.4 Check Windows Service

Open PowerShell as Administrator:

```powershell
Get-Service -Name MonitorTrackAgent
```

Expected status:

```text
Status   Name                  DisplayName
------   ----                  -----------
Running  MonitorTrackAgent    MonitorTrackAgent
```

You can also use:

```powershell
sc.exe query MonitorTrackAgent
```

## 3.5 Start the Agent

```powershell
Start-Service -Name MonitorTrackAgent
```

## 3.6 Stop the Agent

```powershell
Stop-Service -Name MonitorTrackAgent
```

## 3.7 Restart the Agent

```powershell
Restart-Service -Name MonitorTrackAgent
```

## 3.8 Configure Automatic Startup

The installer configures the service with automatic startup.

Verify:

```powershell
Get-Service -Name MonitorTrackAgent
```

The service should automatically start when Windows boots.

---

# 4. Installation Locations

## Linux

Application:

```text
/opt/employee-agent/
```

Binary:

```text
/opt/employee-agent/employee-agent
```

Configuration:

```text
/opt/employee-agent/configs/config.yaml
```

Systemd service:

```text
/etc/systemd/system/employee-agent.service
```

Runtime data:

```text
/opt/employee-agent/data/
```

Logs:

```text
/opt/employee-agent/logs/
```

## Windows

Application:

```text
C:\Program Files\MonitorTrackAgent\
```

Binary:

```text
C:\Program Files\MonitorTrackAgent\employee-agent.exe
```

Configuration:

```text
C:\Program Files\MonitorTrackAgent\configs\config.yaml
```

Windows service:

```text
MonitorTrackAgent
```

Runtime data:

```text
C:\Program Files\MonitorTrackAgent\data\
```

Logs:

```text
C:\Program Files\MonitorTrackAgent\logs\
```

---

# 5. Troubleshooting

## Linux

### Service failed to start

Check the service:

```bash
sudo systemctl status employee-agent --no-pager
```

Check detailed logs:

```bash
sudo journalctl -u employee-agent -n 100 --no-pager
```

Check the installed files:

```bash
ls -la /opt/employee-agent
```

Check the configuration:

```bash
cat /opt/employee-agent/configs/config.yaml
```

Check the service definition:

```bash
cat /etc/systemd/system/employee-agent.service
```

After modifying the service file:

```bash
sudo systemctl daemon-reload
sudo systemctl restart employee-agent
```

### Installer says it must run as root

Run:

```bash
sudo ./installer
```

instead of:

```bash
./installer
```

---

# 6. Windows Troubleshooting

### Service is not running

Check:

```powershell
Get-Service -Name MonitorTrackAgent
```

Check detailed service information:

```powershell
sc.exe query MonitorTrackAgent
```

Try restarting:

```powershell
Restart-Service -Name MonitorTrackAgent
```

### Service does not exist

Run the installer again from an **Administrator PowerShell**:

```powershell
.\installer.exe
```

### Installer says Administrator privileges are required

Close the current terminal.

Open:

**Start → PowerShell → Run as administrator**

Then:

```powershell
cd C:\employee-agent-install
.\installer.exe
```

---

# 7. Updating the Agent

## Linux

Download the new release package and extract it.

Run the installer again:

```bash
sudo ./installer
```

The installer will replace the installed binary and configuration and restart the service.

Verify:

```bash
sudo systemctl status employee-agent
```

## Windows

Extract the new release package.

Open PowerShell as Administrator:

```powershell
cd C:\employee-agent-install
.\installer.exe
```

The installer will stop the existing service, update the application, and start the service again.

Verify:

```powershell
Get-Service -Name MonitorTrackAgent
```

---

# 8. Release Package

Release packages are generated using the project's release process.

Linux:

```bash
make release platform=linux/amd64
```

Windows:

```bash
make release platform=windows/amd64
```

The resulting packages are stored in:

```text
releases/
```

Example:

```text
releases/
├── employee-agent-linux-amd64-1.0.0.tar.gz
└── employee-agent-windows-amd64-1.0.0.zip
```

The release version is read from:

```text
configs/config.linux.yaml
```

for Linux and:

```text
configs/config.windows.yaml
```

for Windows.

# 9. Quick Installation

## Linux

```bash
tar -xzf employee-agent-linux-amd64-<VERSION>.tar.gz
cd employee-agent-linux-amd64-<VERSION>
sudo ./installer
sudo systemctl status employee-agent
```

## Windows

Run **PowerShell as Administrator**:

```powershell
Expand-Archive employee-agent-windows-amd64-<VERSION>.zip -DestinationPath C:\employee-agent-install
cd C:\employee-agent-install
.\installer.exe
Get-Service -Name MonitorTrackAgent
```

# 10. Uninstallation

## Linux

Stop and disable the service:

```bash
sudo systemctl stop employee-agent
sudo systemctl disable employee-agent
```

Remove the systemd service:

```bash
sudo rm -f /etc/systemd/system/employee-agent.service
sudo systemctl daemon-reload
```

Remove the application:

```bash
sudo rm -rf /opt/employee-agent
```

## Windows

Stop the service:

```powershell
Stop-Service -Name MonitorTrackAgent
```

Delete the service:

```powershell
sc.exe delete MonitorTrackAgent
```

Then remove:

```text
C:\Program Files\MonitorTrackAgent
```

# 11. Summary

The Employee Agent uses native platform installation mechanisms:

* **Linux:** systemd service
* **Windows:** Windows Service
* **Linux installer:** Go executable
* **Windows installer:** Go executable
* **Runtime directories:** created/managed by the agent
* **Configuration:** packaged with each release
* **Release packages:** generated separately for Linux and Windows
