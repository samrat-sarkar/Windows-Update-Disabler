# Windows Update Disabler

A simple batch script to stop and disable Windows services and tasks related to updates, with an option to restore them. This script requires administrative privileges to run.

## Features

- Stops and disables Windows Update service
- Stops and disables Windows Modules Installer service
- Stops and disables Windows Update Medic Service
- Stops and disables Connected User Experiences and Telemetry service
- Kills Windows Update and WaaSMedic tasks
- Restores the above services and tasks

## Usage

1. Download the script or clone the repository:
    ```sh
    git clone https://github.com/samrat-sarkar/Windows-Update-Disabler.git
    ```
2. Open a command prompt with administrative privileges.
3. Navigate to the directory where the script is located.
4. Run the script to disable services and tasks:
    ```sh
    Windows-Update-Disabler.bat
    ```
5. Run the script to restore services and tasks:
    ```sh
    Windows-Update-Restorer.bat
    ```

## Script Details

The scripts check for administrative privileges before executing the following actions:

### Disable Script

- Stops and disables `wuauserv` (Windows Update service)
- Stops and disables `TrustedInstaller` (Windows Modules Installer service)
- Stops and disables `WaasMedicSvc` (Windows Update Medic Service)
- Stops and disables `DiagTrack` (Connected User Experiences and Telemetry service)
- Kills `WindowsUpdate.exe` and `WaaSMedic.exe` tasks

### Restore Script

- Enables and starts `wuauserv` (Windows Update service)
- Enables and starts `TrustedInstaller` (Windows Modules Installer service)
- Enables and starts `WaasMedicSvc` (Windows Update Medic Service)
- Enables and starts `DiagTrack` (Connected User Experiences and Telemetry service)

### Code

#### Windows-Update-Disabler.bat

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
