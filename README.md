# Telegram Keyword Alert Bot

Fork of [Hootrix/keyword_alert_bot](https://github.com/Hootrix/keyword_alert_bot) with English translation.

Monitor Telegram channels and groups for keywords and get instant notifications.

## Features

- Keyword-based message subscription with real-time alerts
- Regex support (JavaScript-like syntax)
- Multi-channel & multi-keyword subscriptions
- Private channels via ID or invite link
- Group message support

## Setup

### 1. Configuration

Copy `config.yml.example` to `config.yml` and fill in:

- **API credentials** — get `api_id` and `api_hash` at https://my.telegram.org/apps
- **Bot token** — create a bot via https://t.me/BotFather
- **Phone & username** — your Telegram account that will monitor channels

### 2. Docker (recommended)

```bash
# First run (interactive — enter SMS code)
docker compose run --rm keyword_alert_bot

# After successful login, run in background
docker compose up -d
```

Data is persisted via volume mounts (`db/`, `data/`).

### 3. Manual

```bash
pipenv install
pipenv shell
python3 ./main.py
```

## Usage

### Basic keyword matching

```
/subscribe  keyword  https://t.me/channel_name
/subscribe  coupon   https://t.me/channel1,https://t.me/channel2
```

### Regex matching

Use JavaScript-like regex syntax wrapped in `/`. Supported flags: `i`, `g`

```
# Match "iphone x" but exclude XR, XS (case insensitive)
/subscribe  /(iphone\s*x)(?:[^sr]|$)/ig  channel1,channel2

# Match any 2-char word followed by "券"
/subscribe  /([\S]{2}券)/g  https://t.me/channel_name
```

### Match multiple keywords simultaneously

```
/(?=.*cc)(?=.*bb)(?=.*aa).*/
```

## Commands

| Command | Description |
|---|---|
| `/subscribe` | Subscribe to keywords in channels |
| `/unsubscribe` | Unsubscribe keywords from channels |
| `/unsubscribe_id` | Unsubscribe by subscription ID |
| `/unsubscribe_all` | Remove all subscriptions |
| `/list` | Show all active subscriptions |
| `/cancel` | Cancel current operation |
| `/help` | Show help message |

## Q&A

> Bug reports: https://github.com/lawdt/keyword_alert_bot/issues

### 1. "You have joined too many channels/supergroups"

The Telegram account has hit the 500 channel limit. Use a dedicated account.

### 2. "sqlite3.OperationalError: unable to open database file"

Permission issue with mounted volumes. Ensure `db/` and `data/` dirs are writable by UID 65532 (nonroot):

```bash
chown -R 65532:65532 db/ data/
```

### 3. Some groups don't receive messages

Try updating Telethon to the latest version.

## License

[GPLv3](LICENSE) — forked from [Hootrix/keyword_alert_bot](https://github.com/Hootrix/keyword_alert_bot)
