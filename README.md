# Community Telegram Bot

Python Telegram bot for lightweight community moderation. The bot watches membership changes, welcomes new users, and temporarily bans new members who do not send a message within 60 seconds.

## What the bot does

- Tracks where the bot is installed across private chats, groups, and channels.
- Welcomes newly added group members.
- Schedules a temporary ban for new members who stay silent after joining.
- Cancels the pending ban as soon as the new member sends any text message.
- Sends formatted exception reports to a developer account.
- Exposes a `/show_chats` command to list the chat IDs currently seen by the bot.

## Stack

- Python
- `python-telegram-bot==13.5`
- APScheduler via the Telegram job queue

## Project layout

- `main.py`: bot startup, handlers, moderation flow, and error reporting
- `requirements.txt`: Python dependencies
- `Procfile`: process definition for platform deployment
- `secrets.json`: local runtime configuration file expected by the app

## Configuration

Create a `secrets.json` file in the repository root:

```json
{
  "secrets": {
    "BOT_API_TOKEN": "telegram-bot-token",
    "DEV_USER": 123456789,
    "CHAT_HANDLE": -1000000000000
  }
}
```

Notes:

- `BOT_API_TOKEN` is the Telegram bot token from BotFather.
- `DEV_USER` is the Telegram user ID that receives stack traces.
- `CHAT_HANDLE` is used when kicking users; in practice this should be the target group or supergroup chat ID.

## Run locally

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python main.py
```

The bot uses long polling, so no webhook setup is required.

## Behavior notes

- New members get 60 seconds to introduce themselves.
- If they do not send a text message, the bot bans them for 24 hours.
- Any text message from the pending user cancels the scheduled removal job.
- Chat membership tracking is stored in memory, so it resets when the process restarts.

## Limitations

- There is no persistence layer for tracked chat IDs or moderation history.
- The moderation flow assumes a single target chat for kicking members.
- The repository currently includes a committed `secrets.json`; treat real credentials as sensitive and keep them out of version control.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. See [LICENSE](LICENSE).
