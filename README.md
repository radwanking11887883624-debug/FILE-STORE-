# AVI FILE STORE BOT

Production-ready Telegram File Store Bot — single file (`main.py`), SQLite, fully async.

## ⚠️ First: rotate your bot token

Your original prompt contained your live bot token in plain text. Treat it as
compromised — open **@BotFather → /mybots → your bot → API Token → Revoke
current token** and generate a new one before deploying.

## Setup

1. Install dependencies:
   ```
   pip install -r requirements.txt
   ```

2. Provide your bot token **without hardcoding it in the source**. Either:
   - Set an environment variable:
     ```
     export BOT_TOKEN="123456:your-new-token"
     ```
     (on Windows: `set BOT_TOKEN=123456:your-new-token`)
   - OR create a file named `token.txt` next to `main.py` containing only the
     token (this file is gitignored-style — never commit it or share it).

3. (Optional) Override these via environment variables if needed:
   - `OWNER_ID` — your Telegram numeric user ID (defaults to the one you gave: `6889457333`)
   - `BOT_USERNAME` — your bot's username without `@` or the `t.me/` prefix (defaults to `AVI_FILE_STORE_BOT`)

4. Run it:
   ```
   python main.py
   ```

`database.db` and every table are created automatically on first launch — no
manual setup needed. The user running the bot (`OWNER_ID`) is auto-seeded as
the first admin; use the 👑 Admin Panel → 👥 User Manager to promote other
admins by directly inserting into the `admins` table, or ask the owner to run
a one-off `INSERT INTO admins ...` (a dedicated "add admin" UI button can be
added on request).

## What's implemented

- **Force Join** — inline-keyboard-only channel gate; admin adds channels by
  `@username`, bot verifies it's an admin there and pulls the real title so
  users never see the raw username.
- **File Store** — admin upload wizard (file → name → description → price →
  preview → publish), user browsing/pagination, free vs paid unlock, forever
  access after purchase.
- **Wallet & Coins** — balance, full transaction log (referral / redeem /
  admin gift / purchase / payment), daily bonus with 24h cooldown.
- **Manual Stars Payments** — user picks a package, sends stars manually,
  submits a screenshot, every admin gets an Accept/Reject card, duplicate
  pending requests are blocked.
- **Referrals** — unique `?start=<id>` links, self-referral blocked, one-time
  reward, top-10 leaderboard.
- **Redeem Codes** — coin + optional premium-day rewards, usage limits,
  expiry dates, one-redeem-per-user enforcement.
- **Admin Panel** — upload/manage files, wallet manager, redeem manager,
  force-channel manager, broadcast (all media types, with success/fail
  counts), user manager (search, ban/unban, add/remove coins, grant/revoke
  premium), premium manager, statistics dashboard, maintenance mode, and a
  live settings editor (nothing is hardcoded — referral reward, daily bonus,
  coin packages, support/payment usernames, bot name, force-join toggle).
- **Premium** — 7/30/90-day or lifetime grants, auto-expiry via a background
  job that runs hourly.
- **Reliability** — every handler is wrapped so the bot won't crash; errors
  are logged and forwarded to all admins; inputs are validated throughout.

## Notes

- I could not run this against Telegram's live servers in the environment
  that generated it (no outbound network access there), so please do a quick
  smoke test after installing dependencies: `/start`, upload a file as admin,
  buy it as a test user, try a redeem code, and toggle maintenance mode.
- Everything lives in `main.py` per your spec — no `config.py`, `database.py`,
  `handlers.py`, `utils.py`, or subfolders.
