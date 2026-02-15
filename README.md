# LeaveFlow Desktop Agent

Activity monitoring and time tracking desktop agent for Windows. Part of the LeaveFlow HR Management platform.

## Features

- Automatic activity tracking (foreground application monitoring)
- Idle detection with auto-pause
- Session management (start, pause, resume, finish)
- Mood check prompts
- Local data storage with batch sync to server
- System tray integration

## Download

Go to [Releases](https://github.com/Novestra/LeaveFlow-Agent/releases) and download the appropriate version:

- **LeaveFlow-Agent-Prod** - For production use
- **LeaveFlow-Agent-Dev** - For development/testing

## Installation

1. Download the ZIP file from the latest release
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
| `Tracking:ScreenshotIntervalMinutes` | 10 | Screenshot capture interval |
| `Tracking:IdleWarningMinutes` | 5 | Idle warning threshold |
| `Tracking:IdleAutoPasseMinutes` | 10 | Auto-pause after idle |
| `Tracking:SyncIntervalSeconds` | 60 | Data sync interval |
