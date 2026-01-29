# Gmail Sender

> Gmail API를 통해 이메일을 발송합니다. 커피챗 리마인더, 후속 연락, 네트워킹 메일 발송에 활용.

A Claude Code skill for sending emails via Gmail API.

## Installation

### Claude Code Plugin (Recommended)

```bash
/plugin install kubony/claude-gmail-sender
```

### Manual Installation

```bash
git clone https://github.com/kubony/claude-gmail-sender.git
cp -r claude-gmail-sender ~/.claude/skills/gmail-sender
```

## Prerequisites

1. Create OAuth credentials in [Google Cloud Console](https://console.cloud.google.com/)
2. Enable Gmail API
3. Download OAuth client JSON and save to `.creds/oauth_client.json`
4. Run first-time authentication (requires send permission)

## Features

- **Email Sending** - Send emails with recipient, subject, and body
- **Dry-run Preview** - Preview email before sending
- **Confirmation Prompt** - Safety confirmation before sending

## Usage Examples

```bash
# Basic email send
python scripts/send_email.py \
  --to "recipient@example.com" \
  --subject "Subject" \
  --body "Body content"

# Preview without sending
python scripts/send_email.py \
  --to "recipient@example.com" \
  --subject "Test" \
  --body "Test body" \
  --dry-run

# Send without confirmation
python scripts/send_email.py \
  --to "recipient@example.com" \
  --subject "Subject" \
  --body "Body" \
  --yes
```

## Triggers

- "메일 보내줘"
- "이메일 발송해줘"
- "리마인더 메일 보내줘"
- "[인물명]에게 메일 보내줘"

## Requirements

- Python 3.8+
- google-api-python-client
- google-auth-httplib2
- google-auth-oauthlib

## Note

This skill uses a separate token from gmail-reader (`gmail_send_token.pickle`) to require explicit send permission.

## License

MIT
