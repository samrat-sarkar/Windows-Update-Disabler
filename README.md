# 🛡️ Windows Update Disabler

## 📝 Description
A simple batch script to stop and disable Windows services and tasks related to updates, with an option to restore them. This script requires administrative privileges to run.

## ✨ Features
- 🛑 Stops and disables Windows Update service
- 🛑 Stops and disables Windows Modules Installer service
- 🛑 Stops and disables Windows Update Medic Service
- 🛑 Stops and disables Connected User Experiences and Telemetry service
- 🛑 Kills Windows Update and WaaSMedic tasks
- 🔄 Restores the above services and tasks

## 🚀 Usage

### 📥 Installation
1. Download the script or clone the repository:
   ```bash
   git clone https://github.com/samrat-sarkar/Windows-Update-Disabler.git
   ```

### ⚙️ Running the Script
1. Open a command prompt with administrative privileges
2. Navigate to the directory where the script is located
3. Run the script to disable services and tasks:
   ```bash
   Windows-Update-Disabler.bat
   ```
4. Run the script to restore services and tasks:
   ```bash
   Windows-Update-Restorer.bat
   ```

## 🔧 Script Details

### ⚠️ Prerequisites
- Administrative privileges
- Windows operating system

### 🔄 Services Modified

#### Disable Script
- `wuauserv` (Windows Update service)
- `TrustedInstaller` (Windows Modules Installer service)
- `WaasMedicSvc` (Windows Update Medic Service)
- `DiagTrack` (Connected User Experiences and Telemetry service)
- `WindowsUpdate.exe` and `WaaSMedic.exe` tasks

#### Restore Script
- `wuauserv` (Windows Update service)
- `TrustedInstaller` (Windows Modules Installer service)
- `WaasMedicSvc` (Windows Update Medic Service)
- `DiagTrack` (Connected User Experiences and Telemetry service)

## 📄 Code Examples

### Windows-Update-Disabler.bat
```batch
@echo off
:: Batch script to stop Windows services and tasks related to updates
:: Check if running with administrative privileges
>nul 2>&1 "%SYSTEMROOT%\system32\cacls.exe" "%SYSTEMROOT%\system32\config\system"

REM If error flag set, we do not have administrative privileges.
if %errorlevel% NEQ 0 (
    echo Requesting administrative privileges...
    goto UACPrompt
) else (goto gotAdmin)

:UACPrompt
    echo Set UAC = CreateObject^("Shell.Application"^) > "%temp%\getadmin.vbs"
    echo UAC.ShellExecute "%~s0", "", "", "runas", 1 >> "%temp%\getadmin.vbs"

    "%temp%\getadmin.vbs"
    exit /B

:gotAdmin
    if exist "%temp%\getadmin.vbs" (del "%temp%\getadmin.vbs")

:: Actual script starts here
echo Stopping Windows services and tasks...
echo.

:: Stop and disable Windows services
sc stop wuauserv > nul 2>&1
sc config wuauserv start= disabled > nul 2>&1
echo Windows Update service stopped and disabled.

sc stop TrustedInstaller > nul 2>&1
sc config TrustedInstaller start= disabled > nul 2>&1
echo Windows Modules Installer service stopped and disabled.

sc stop WaasMedicSvc > nul 2>&1
sc config WaasMedicSvc start= disabled > nul 2>&1
echo Windows Update Medic Service stopped and disabled.

sc stop DiagTrack > nul 2>&1
sc config DiagTrack start= disabled > nul 2>&1
echo Connected User Experiences and Telemetry service stopped and disabled.

:: Stop Windows Update and WaaSMedic tasks
taskkill /F /IM WindowsUpdate.exe /T > nul 2>&1
taskkill /F /IM WaaSMedic.exe /T > nul 2>&1
echo Windows Update and WaaSMedic tasks killed.

echo.
echo All specified services and tasks stopped and disabled.

pause
```

### Windows-Update-Restorer.bat
```batch
@echo off
:: Batch script to restore Windows services and tasks related to updates
:: Check if running with administrative privileges
>nul 2>&1 "%SYSTEMROOT%\system32\cacls.exe" "%SYSTEMROOT%\system32\config\system"

REM If error flag set, we do not have administrative privileges.
if %errorlevel% NEQ 0 (
    echo Requesting administrative privileges...
    goto UACPrompt
) else (goto gotAdmin)

:UACPrompt
    echo Set UAC = CreateObject^("Shell.Application"^) > "%temp%\getadmin.vbs"
    echo UAC.ShellExecute "%~s0", "", "", "runas", 1 >> "%temp%\getadmin.vbs"

    "%temp%\getadmin.vbs"
    exit /B

:gotAdmin
    if exist "%temp%\getadmin.vbs" (del "%temp%\getadmin.vbs")

:: Actual script starts here
echo Restoring Windows services and tasks...
echo.

:: Enable and start Windows services
sc config wuauserv start= auto > nul 2>&1
sc start wuauserv > nul 2>&1
echo Windows Update service enabled and started.

sc config TrustedInstaller start= manual > nul 2>&1
sc start TrustedInstaller > nul 2>&1
echo Windows Modules Installer service enabled and started.

sc config WaasMedicSvc start= manual > nul 2>&1
sc start WaasMedicSvc > nul 2>&1
echo Windows Update Medic Service enabled and started.

sc config DiagTrack start= auto > nul 2>&1
sc start DiagTrack > nul 2>&1
echo Connected User Experiences and Telemetry service enabled and started.

echo.
echo All specified services and tasks restored.

pause
```

## ⚠️ Disclaimer
This script should be used with caution. Disabling Windows Update services may prevent your system from receiving important security updates. Use at your own risk.

## 🤝 Contributing
Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author
- **Samrat Sarkar**
  - LinkedIn: [samratsarkar9999](https://www.linkedin.com/in/samratsarkar9999/)
  - Website: [samratsarkar.in](https://samratsarkar.in/)
