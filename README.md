# AzureHound Device Code Automation Script

This Bash script automates the data collection process from Entra ID (Azure AD) and AzureRM using **AzureHound Community Edition** via the **Device Code** authentication flow.

It was specifically designed for scenarios where the user encounters **MFA (Multi-Factor Authentication)** restrictions or **Conditional Access Policies (CAP)**, eliminating the need for complex manual PowerShell interactions to obtain and manage Refresh Tokens.

---

## 🚀 How It Works

1. **Automated Request:** The script sends an HTTP request to Microsoft endpoints requesting a device code using the Azure PowerShell Client ID (`1950a258-227b-4e31-a9cf-717495945fc2`).
2. **Authentication Polling:** It enters a loop waiting for you to enter the generated code into your browser and complete the authentication process.
3. **Automatic Collection:** As soon as login is confirmed, the script dynamically captures the Refresh Token and immediately triggers `azurehound`, generating the final JSON file ready for import into BloodHound.

---

## 📋 Prerequisites

Ensure you have the following tools installed on your Linux/macOS environment:

* **Debian / Ubuntu / Parrot OS / Kali**
```bash
  sudo apt install curl jq -y
```

* **RHEL / CentOS / Fedora**
```bash
  sudo dnf install curl jq -y
```

> ⚠️ **Important:** The `azurehound` binary must also be present in the directory (or the path correctly configured in the script variables).

---

## ⚙️ Configuration

Before running, open the script and adjust the global variables located at the top of the file:

```bash
TENANT="YOUR_TENANT_ID_HERE"        # Preferably use the target Tenant GUID - Check it at: https://www.whatismytenantid.com/
AZUREHOUND_PATH="/bin/azurehound"   # Path to the AzureHound executable
OUTPUT_FILE="output.json"           # Output filename for BloodHound
```
> 💡 **Red Team Tip:** Using the Tenant ID in GUID format (e.g., `0fe1c33c-50ee-467f-9405-8396b8b74e3d`) instead of the domain name avoids user scope errors (such as *User was not found*) if the target account is a Guest User in the environment.

---

## 📖 Usage

1. Grant execution permission to the script:
```bash
   chmod +x azhound-dc
```

2. Start execution:
```bash
   ./azhound-dc
```

3. The terminal will display a highlighted message. Open the indicated browser, navigate to the login URL, enter the generated code, and authenticate with the corresponding account.

4. Return to the terminal. The script will automatically detect the completed login and generate the output file. Simply upload the `output.json` file directly into the BloodHound CE interface.

---

## 🔗 References

This script was developed based on the official SpecterOps documentation for bypassing MFA and Conditional Access in AzureHound:
* [BloodHound Documentation - Dealing with Multi-Factor Auth and Conditional Access Policies](https://bloodhound.specterops.io/collect-data/ce-collection/azurehound#dealing-with-multi-factor-auth-and-conditional-access-policies)
