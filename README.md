# LeaveFlow Desktop Agent

Activity monitoring and time tracking desktop agent for Windows. Part of the LeaveFlow HR Management platform.

## Download

| Version | Environment | Download |
|---------|-------------|----------|
| v2.3.4 | **Production** | [LeaveFlow-Agent-Prod-v2.3.4.zip](https://github.com/Novestra/LeaveFlow-Agent/releases/download/v2.3.4/LeaveFlow-Agent-Prod-v2.3.4.zip) |
| v2.3.3 | **Development** | [LeaveFlow-Agent-Dev-v2.3.3.zip](https://github.com/Novestra/LeaveFlow-Agent/releases/download/v2.3.3/LeaveFlow-Agent-Dev-v2.3.3.zip) |

## Features

- **Activity Tracking** — Monitors foreground application usage with configurable polling intervals
- **Screenshot Capture** — Periodic screenshots synced to server for activity verification
- **Idle Detection** — Detects inactivity with warning prompt and auto-pause after configurable threshold
- **Session Management** — Start, pause, resume, and finish work sessions with system tray controls
- **Mood Check Prompts** — Periodic mood surveys during active sessions
- **Work Reminder Notifications** — Configurable reminders to stay on track (adjustable interval in Settings)
- **Wellness Reminder Toasts** — Non-blocking toast notifications during active sessions at server-configured intervals
- **Automatic Update Checker** — Notifies when a new version is available on GitHub
- **Automatic Token Refresh** — Thread-safe re-authentication with no session interruptions
- **Settings Window** — Configure API URL and work reminder interval from the UI
- **Local Data Storage** — LiteDB-backed offline storage with automatic batch sync to server
- **System Tray Integration** — Runs minimized in system tray with status icons (idle/working/paused)

## Changelog

### v2.3.4 — Idle Detection Overflow Fix
- **Fixed TickCount overflow in idle detection** — `Environment.TickCount` wraps around after ~24.9 days of uptime, causing `GetIdleTime()` to return negative values and break idle detection. Changed to `uint` arithmetic with unsigned subtraction so the wrap-around is handled correctly.
- Extracted pure logic from agent services into testable internal methods
- Added 74+ unit tests covering idle detection, session management, activity tracking, and update checking

### v2.3.3 — Wellness Reminder Toasts
- **Wellness reminder toast notifications** — Non-blocking toast notifications appear during active work sessions at each reminder's configured interval
- Reminders are fetched from the API on session start and stop when session is paused/finished
- Catppuccin dark theme toast with green accent, auto-dismisses after 30 seconds
- Only one toast shown at a time to avoid notification spam

### v2.3.1 — Bug Fix
- **Fixed session timer showing wall-clock time instead of active time** — The timer calculated elapsed time as `now - sessionStart` which included all paused periods. After pause/resume cycles (idle, screen lock, manual pause) the displayed time became inflated (e.g. 8h shown vs 1h52m actual). Now correctly accumulates only active segments using server's `TotalDurationSeconds`.

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
| `Tracking:SyncIntervalSeconds` | 60 | Data sync interval |
