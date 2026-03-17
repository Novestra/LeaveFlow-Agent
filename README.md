# LeaveFlow Desktop Agent

Activity monitoring and time tracking desktop agent for Windows. Part of the LeaveFlow HR Management platform.

## Download

| Version | Environment | Download |
|---------|-------------|----------|
| v2.4.1 | **Production** | [LeaveFlow-Agent-Prod-v2.4.1.zip](https://github.com/Novestra/LeaveFlow-Agent/releases/download/v2.4.1/LeaveFlow-Agent-Prod-v2.4.1.zip) |
| v2.3.6 | **Development** | [LeaveFlow-Agent-Dev-v2.3.6.zip](https://github.com/Novestra/LeaveFlow-Agent/releases/download/v2.3.6/LeaveFlow-Agent-Dev-v2.3.6.zip) |

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
- **Shift End Auto-Restart** — Automatically finishes session at configured shift end time and starts a new session after a delay
- **Midnight Session Split** — Automatically splits sessions at midnight to prevent activity leaking across dates
- **System Tray Integration** — Runs minimized in system tray with status icons (idle/working/paused)

## Changelog

### v2.4.1 — Version Fix & Latest Changes
- **Fixed version display** — Agent was showing v2.0.0 instead of the actual version number
- Includes all v2.4.0 changes (shift-end auto-restart, midnight split, server config sync)

### v2.4.0 — Shift End Auto-Restart
- **Shift End Auto-Restart** — Agent automatically finishes the session at the configured shift end time and starts a new session after a configurable delay (1-10 minutes). Prevents sessions from running overnight and leaking activity data into the next day.
- **DPI Scaling Fix** — Forms now scale properly on high-DPI displays when running in debug mode
- Configure `ShiftEndTime` (e.g., "17:30") and `AutoRestartDelayMinutes` (default: 2) in Productivity Settings via the admin panel
- Feature is disabled by default (`ShiftEndTime` = null). Existing agents continue to work — they simply ignore the new config fields

### v2.3.6 — Settings UI & Error Log API Key
- **Agent Error Log API Key display** — Settings window now shows the masked Error Log API Key used for error reporting via `X-Agent-Error-Key` header
- **Agent version reporting** — Version is now sent with session data and displayed in the web dashboard next to sync time
- Settings window height increased to accommodate new fields

### v2.3.5 — Sleep/Wake Auto-Pause Fix
- **Fixed auto-pause not reaching server during sleep/hibernate** — The fire-and-forget API call could fail silently if the network shut down before completion, leaving the server session as Working through the entire sleep period
- Added Resume handler that verifies pause state on server after system wake
- Added SessionUnlock handler with same verification
- Sleep recovery: forces auto-pause if Suspend handler failed entirely

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
