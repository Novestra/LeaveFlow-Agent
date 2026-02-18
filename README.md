# LeaveFlow Desktop Agent

Activity monitoring and time tracking desktop agent for Windows. Part of the LeaveFlow HR Management platform.

## Download

| Version | Environment | Download |
|---------|-------------|----------|
| v2.3.0 | **Production** | [LeaveFlow-Agent-Prod-v2.3.0.zip](https://github.com/Novestra/LeaveFlow-Agent/releases/download/v2.3.0/LeaveFlow-Agent-Prod-v2.3.0.zip) |
| v2.3.0 | **Development** | [LeaveFlow-Agent-Dev-v2.3.0.zip](https://github.com/Novestra/LeaveFlow-Agent/releases/download/v2.3.0/LeaveFlow-Agent-Dev-v2.3.0.zip) |

## Features

- Automatic activity tracking (foreground application monitoring)
- Idle detection with auto-pause
- Session management (start, pause, resume, finish)
- Mood check prompts
- Work reminder notifications (configurable interval)
- Automatic update checker
- Settings window (API URL, work reminder interval)
- Automatic re-login on token expiry
- Local data storage with batch sync to server
- System tray integration

## Installation

1. Download the ZIP file for your environment from the table above
2. Extract to a folder (e.g., `C:\LeaveFlow\`)
3. Run `LeaveFlow.DesktopAgent.exe`
4. Login with your LeaveFlow credentials

## System Requirements

- Windows 10/11 (64-bit)
- No additional runtime required (self-contained)

## Configuration

The `appsettings.json` file contains configurable settings:

| Setting | Default | Description |
|---------|---------|-------------|
| `Api:BaseUrl` | (pre-configured) | API server URL |
| `Api:TimeoutSeconds` | 30 | API request timeout |
| `Tracking:ForegroundPollIntervalSeconds` | 10 | How often to check active window |
| `Tracking:IdleWarningMinutes` | 5 | Idle warning threshold |
| `Tracking:IdleAutoPasseMinutes` | 10 | Auto-pause after idle |
| `Tracking:SyncIntervalSeconds` | 60 | Data sync interval |
