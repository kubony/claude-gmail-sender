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

## Setup (First-time only)

### For Existing gmail-reader Users

If you already have OAuth credentials from gmail-reader, you just need to create a new token with send permission:

```bash
# Activate your virtual environment
source .venv/bin/activate

# Run authentication (will request send permission)
python ~/.claude/skills/gmail-sender/scripts/gmail_client.py
```

The browser will open asking for **additional permission to send emails**. A separate token `gmail_send_token.pickle` will be saved.

### For New Users

#### Step 1: Google Cloud Console Setup

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select existing one
3. Navigate to **APIs & Services > Library**
4. Search for **Gmail API** and click **Enable**
5. Go to **APIs & Services > OAuth consent screen**
   - Select **External** user type
   - Fill in app name and developer email
   - Add scopes: `gmail.send` (and `gmail.readonly`, `gmail.modify` if using gmail-reader too)
   - Add your email to **Test users**
6. Go to **APIs & Services > Credentials**
   - Click **Create Credentials > OAuth client ID**
   - Select **Desktop app** as application type
   - Enter a name and create
7. Download the JSON file (click the download icon)
8. Save the file:
   ```
   ~/.claude/skills/gmail-sender/.creds/oauth_client.json
   ```
   Or if installed in a project:
   ```
   .claude/skills/gmail-sender/.creds/oauth_client.json
   ```

#### Step 2: First-time Authentication

```bash
# Activate your virtual environment
source .venv/bin/activate

# Run authentication
python ~/.claude/skills/gmail-sender/scripts/gmail_client.py
```

What happens:
1. Browser opens automatically
2. Sign in with your Google account
3. **Grant permission to send emails** (important!)
4. Token is saved to `gmail_send_token.pickle`

### Why Separate Tokens?

This skill intentionally uses a separate token (`gmail_send_token.pickle`) from gmail-reader (`gmail_token.pickle`) to:
- Require explicit consent for send permission
- Allow read-only usage without send capability
- Provide security separation between read and write operations

### Troubleshooting Authentication

#### "OAuth credentials 파일이 없습니다" / "OAuth credentials file not found"
→ Download OAuth client JSON from Google Cloud Console and save to `.creds/oauth_client.json`

#### "Insufficient Permission"
→ The token doesn't have send permission. Delete and re-authenticate:
```bash
rm ~/.claude/skills/gmail-sender/.creds/gmail_send_token.pickle
python ~/.claude/skills/gmail-sender/scripts/gmail_client.py
```

#### "Access blocked: This app's request is invalid"
→ Go to Google Cloud Console > OAuth consent screen:
- Make sure your email is added to **Test users**
- Or publish the app to production (for personal use, this is safe)

## Features

- **Email Sending** - Send emails with recipient, subject, and body
- **Dry-run Preview** - Preview email before sending
- **Confirmation Prompt** - Safety confirmation before sending
- **Body from File** - Load email body from a text file

## Usage Examples

### Basic Send

```bash
# Basic email send
python scripts/send_email.py \
  --to "recipient@example.com" \
  --subject "Meeting reminder" \
  --body "Hi, just a reminder about our meeting tomorrow at 3pm."
```

### Preview Before Sending

```bash
# Dry-run: preview without actually sending
python scripts/send_email.py \
  --to "recipient@example.com" \
  --subject "Test" \
  --body "Test body" \
  --dry-run
```

### Skip Confirmation

```bash
# Send without confirmation prompt
python scripts/send_email.py \
  --to "recipient@example.com" \
  --subject "Subject" \
  --body "Body" \
  --yes
```

### Load Body from File

```bash
# Read email body from a file
python scripts/send_email.py \
  --to "recipient@example.com" \
  --subject "Weekly Report" \
  --body "" \
  --body-file /path/to/email_body.txt
```

## Use Cases

### Coffee Chat Reminder

```bash
python scripts/send_email.py \
  --to "john@example.com" \
  --subject "[Reminder] Coffee chat today at 5pm" \
  --body "Hi John,

Just a reminder about our coffee chat today.

Time: 5:00 PM
Location: Google Meet (link)

Looking forward to it!

Best regards"
```

### Follow-up Thank You

```bash
python scripts/send_email.py \
  --to "contact@company.com" \
  --subject "Thank you for the great conversation" \
  --body "Hi,

Thank you for taking the time to chat today. I really enjoyed our discussion about [topic].

I'll follow up with the resources I mentioned.

Best regards"
```

## Triggers

- "메일 보내줘"
- "이메일 발송해줘"
- "리마인더 메일 보내줘"
- "[인물명]에게 메일 보내줘"

## Requirements

```bash
pip install google-api-python-client google-auth-httplib2 google-auth-oauthlib
```

## File Structure

```
gmail-sender/
├── SKILL.md              # Skill definition
├── README.md             # This file
├── requirements.txt      # Dependencies
├── plugin.json           # Plugin manifest
├── LICENSE               # MIT License
└── scripts/
    ├── gmail_client.py   # Gmail API client (auth with send permission)
    └── send_email.py     # Email sending script
```

## Safety Notes

- **Confirmation by default**: Always asks for confirmation before sending (use `--yes` to skip)
- **Separate token**: Uses different token from gmail-reader for security
- **No undo**: Sent emails cannot be recalled - use `--dry-run` first
- **Personal accounts**: OAuth2 authentication for personal Gmail

## Related

- [gmail-reader](https://github.com/kubony/claude-gmail-reader) - Search and read emails (companion skill)

## License

MIT
