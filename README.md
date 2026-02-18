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

## Changelog

### v2.3.0 — Bug Fixes
- **Fixed random logout during active sessions** — Concurrent API calls (usage sync + screenshot upload) could race to refresh tokens simultaneously, causing the second thread to use a revoked refresh token and trigger a logout. Added SemaphoreSlim lock to serialize refresh attempts with stale-token detection.
- **Fixed multiple agent windows after inactivity pause** — After logout/reauth cycles, hidden StatusWindow instances retained active event subscriptions, causing duplicate pause popups and multiple windows. Added proper Cleanup() and ForceClose() lifecycle management.
- **Fixed Start Work button visible during active session** — The Start button remained visible in the Working state due to incorrect visibility logic. Now correctly shows Start only when idle, Pause only when active, Resume only when paused.
- **Fixed screenshot upload triggering immediate logout on 401** — Screenshot upload had no token refresh retry, immediately clearing auth on any 401 response. Added retry loop with lock-protected token refresh.
- **Fixed token expiry timezone mismatch** — LiteDB stores DateTime in Local time but expiry was compared against UTC, causing premature expiry detection. Now converts to UTC before comparison with a 2-minute buffer.

### v2.2.0 — Features
- Automatic re-login when token expires (seamless token refresh)
- Update checker with GitHub release version comparison
- Work reminder notifications with configurable interval
- Settings window for API URL and reminder configuration

### v2.1.0
- Updated API URLs for new infrastructure

### v2.0.0 — Initial Release
- Activity tracking, idle detection, session management
- Mood check prompts, system tray integration
- Local LiteDB storage with batch sync

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
