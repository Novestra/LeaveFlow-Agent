# LeaveFlow Desktop Agent

Activity monitoring and time tracking desktop agent for Windows. Part of the LeaveFlow HR Management platform.

## Download

| Version | Environment | Download |
|---------|-------------|----------|
| v2.3.0 | **Production** | [LeaveFlow-Agent-Prod-v2.3.0.zip](https://github.com/Novestra/LeaveFlow-Agent/releases/download/v2.3.0/LeaveFlow-Agent-Prod-v2.3.0.zip) |
| v2.3.0 | **Development** | [LeaveFlow-Agent-Dev-v2.3.0.zip](https://github.com/Novestra/LeaveFlow-Agent/releases/download/v2.3.0/LeaveFlow-Agent-Dev-v2.3.0.zip) |

## Features

- **Activity Tracking** — Monitors foreground application usage with configurable polling intervals
- **Screenshot Capture** — Periodic screenshots synced to server for activity verification
- **Idle Detection** — Detects inactivity with warning prompt and auto-pause after configurable threshold
- **Session Management** — Start, pause, resume, and finish work sessions with system tray controls
- **Mood Check Prompts** — Periodic mood surveys during active sessions
- **Work Reminder Notifications** — Configurable reminders to stay on track (adjustable interval in Settings)
- **Automatic Update Checker** — Notifies when a new version is available on GitHub
- **Automatic Token Refresh** — Thread-safe re-authentication with no session interruptions
- **Settings Window** — Configure API URL and work reminder interval from the UI
- **Local Data Storage** — LiteDB-backed offline storage with automatic batch sync to server
- **System Tray Integration** — Runs minimized in system tray with status icons (idle/working/paused)

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
| `Tracking:ScreenshotIntervalMinutes` | 10 | Screenshot capture interval |
| `Tracking:SyncIntervalSeconds` | 60 | Data sync interval |
