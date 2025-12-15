# WelcomeScreen

WelcomeScreen is a **server-enforced, full-screen onboarding screen** for **Minecraft Forge 1.12.2**.

I built it because I wanted new players joining a server to **mandatorily see the server’s main information** on first login — and optionally **see it again on every login**, so players always know where to find the rules, starter tips, commands, and important links.

This isn’t a “mod info” button tucked away in a menu. It’s intentionally **in your face**: full-screen, impossible to miss, and players can’t just hit **ESC** to dodge it. Server admins control everything through configs, including text, countdown gating, and an optional multi-page info menu.

---

## What it does

On player login, WelcomeScreen shows a full-screen GUI with:

- Title + subtitle
- Scrollable body text (supports long guides)
- **ACCEPT / REFUSE** buttons
- A clearly visible countdown before **ACCEPT** is enabled (optional)

### Server-authoritative enforcement

- If the player **refuses**, they are **kicked** with a configurable message.
- If the player **accepts**, their UUID is stored and the screen can be skipped next time  
  (or shown every time — your choice).

### Optional Info Menu (3rd button)

An optional third button on the main page opens an **Info Menu** with:

- Categories (headers, not clickable)
- Subpages (clickable buttons)
- Subpages can either:
  - show information pages, or
  - open a URL (Discord, wiki, rules page, etc.) via Minecraft’s link confirmation prompt

### No plugins required

- Works on servers without needing plugins (pure Forge mod).

---

## How it works (server behavior)

On player login, the server decides whether to show the screen:

### First-login only mode (default)

Players see the screen until they accept once.

### Every-login mode (optional)

Players see the screen **every time they join**. This is useful for servers that want to remind players that they can browse server info on login.

### Delay countdown is enforced server-side

The countdown (delay before accepting) is enforced server-side, so clients can’t bypass it by hacking the GUI.

---

## Installation

1. Install **Minecraft Forge 1.12.2**
2. Put the mod `.jar` in your `/mods` folder (server + clients)
3. Start the game/server once to generate:
   - `config/welcomescreen.cfg`
   - `config/welcomescreen_pages.json` (if enabled)

---

## Configuration

### `config/welcomescreen.cfg`

Key options you’ll likely care about:

#### Behavior

- `enabled`  
  Master toggle.

- `showEveryLogin`  
  - `false` = show until accepted once  
  - `true` = show on every login

- `pauseGame`  
  Pauses the game while the GUI is open (client-side behavior).

#### Countdown / gating

- `enableAcceptDelay`  
  Enables the initial forced wait.

- `acceptDelaySeconds`  
  How long before **ACCEPT** becomes clickable.

- `allowRefuseDuringDelay`  
  If `false`, **REFUSE** is also locked during the countdown.

- `bigCountdownTextFormat`  
  The prominent text shown while locked. Must include `%d`.  
  Example: `You can close this screen in %d seconds.`

#### Text

- `title`, `subtitle`, `bodyLines`
- `acceptButtonText`, `refuseButtonText`

- `centerBodyText`  
  If you have long paragraphs, `false` (left-aligned) is usually best.

#### Info Menu button

- `enableInfoButton`  
  Enables the optional 3rd button.

- `infoButtonText`  
  Label for that button.

- `infoMenuTitle`  
  Title shown on the menu and subpages.

#### Kick messages

- `refusalKickMessage`
- `earlyAcceptKickMessage`

---

### `config/welcomescreen_pages.json` (Info Menu)

This file controls the categories and pages shown when players click the optional 3rd button.

#### Structure

- `categories[]`
  - `title` (category header, not clickable)
  - `pages[]`
    - `buttonText` (what shows in the menu)
    - `title` (shown at the top of the subpage)
    - `bodyLines` (content lines)

#### Optional URL pages

Pages may also include:

- `openUrlInBrowser: true`
- `url: "https://..."`

When clicked, Minecraft shows a confirmation prompt and then opens the link.

---

## Notes / Design goals

- **Impossible to miss:** full-screen on login, ESC blocked.
- **Server-controlled:** admins configure the content; the server sends it to clients.
- **Repeatable onboarding:** show once or every login, depending on your server style.
- **Practical info delivery:** rules, starter steps, commands, links — right when players join.
