# Forum-based Support Bot

A self-hosted forum-based support Discord bot for communities.

## How to Use

This bot has only one command to use, for auto closing the posts from forum channels.

- Make sure the forum post has tags that is required before posting.
- Add proper permission to the bot to avoid unexpected issues.

### Commands

To close thread using prefix command.

```bash
!close
```

To close the thread and resolved by the username mentioned

```bash
!close @username
```

## Configurations

### Config File

```json
{
    "resolution_tag_name": "Solved",
    "command_prefix": "!",
    "utc_offset": -8,
    "datasheet_init": "init",
    "datasheet_response": "response",
    "datasheet_resolve": "resolve",
    "mention_message": "Hello there! If you need help, please read the information in <#1074862134284005396> and post your questions or issues in the <#1029543258822553680> channel. Our team and community members are always ready to help you out. Thank you for building with us!"
}
```

| Config Name | Description |
| --- | --- |
| `resolution_tag_name` | The name of the tag to mark the post resolved. |
| `command_prefix` | A prefix you want to use with `close` command to be recognized by the bot. |
| `utc_offset` | Time and date UTC offset for recording the data. |
| `datasheet_init` | Refers to the sheet name in your google spreadsheet. This should be the first sheet in the order. |
| `datasheet_response` | Refers to the sheet name in your google spreadsheet. This should be the second sheet in the order. |
| `datasheet_resolve` | Refers to the sheet name in your google spreadsheet. This should be the third sheet in the order. |
| `mention_message` | The message bot will send if the bot mentioned by the user, you can use use the channel ids to mention it within the bot message. |

## Environment Variables

```env
DISCORD_BOT_TOKEN=<discord_bot_token>
DISCORD_SUPPORT_ROLE_ID=<role_id>,<role_id>
GOOGLE_PRIVATE_KEY=<private_key>
GOOGLE_SERVICE_ACCOUNT_EMAIL=<service_email_account>
GOOGLE_SPREADSHEET_ID=<google_spreadsheet_id>
```

### Google Sheets Setup

This bot uses Google Sheets to record support data. You'll need to set up a Google Service Account to authenticate with Google Sheets API.

#### Step 1: Create a Google Cloud Project and Service Account

1. Go to the [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Enable the Google Sheets API:
   - Go to "APIs & Services" > "Library"
   - Search for "Google Sheets API" and enable it
4. Create a service account:
   - Go to "APIs & Services" > "Credentials"
   - Click "Create Credentials" > "Service account"
   - Fill in the service account details and create it
5. Generate a key for the service account:
   - Click on the created service account
   - Go to the "Keys" tab
   - Click "Add Key" > "Create new key"
   - Choose "JSON" format and download the key file

#### Step 2: Extract Credentials

From the downloaded JSON file, extract the following values:

- `GOOGLE_SERVICE_ACCOUNT_EMAIL`: The `client_email` field
- `GOOGLE_PRIVATE_KEY`: The `private_key` field (keep the `\n` characters as they are)

#### Step 3: Configure Google Spreadsheet

1. Create a new Google Spreadsheet or use an existing one
2. Copy the spreadsheet ID from the URL (the long string between `/d/` and `/edit`)
3. Share the spreadsheet with your service account email with "Editor" permissions
4. Create three sheets in your spreadsheet with the names specified in your `config.json`:
   - Sheet for `datasheet_init` (default: "init")
   - Sheet for `datasheet_response` (default: "response")
   - Sheet for `datasheet_resolve` (default: "resolve")

#### Step 4: Set Environment Variables

Set the `GOOGLE_SPREADSHEET_ID` to your spreadsheet ID from step 3.

> **Note**: For more detailed information about Google Sheets authentication, refer to the [node-google-spreadsheet documentation](https://theoephraim.github.io/node-google-spreadsheet/#/guides/authentication?id=service-account).

## Development

### Install the packages

```bash
yarn install
```

### Run the bot

```bash
yarn dev
```

## 📃 License

The Forum-based Support Discord Bot is licensed under [GNU General Public License v3](https://opensource.org/licenses/GPL-3.0).

## 🍀 Sponsor

Love what I do? Send me some [love](https://github.com/sponsors/warengonzaga) or [coffee](https://buymeacoff.ee/warengonzaga)!? 💖☕

Can't send love or coffees? 😥 Nominate me for a **[GitHub Star](https://stars.github.com/nominate)** instead!

> **Note**: Your support means a lot to me as it allows me to dedicate my time and energy to creating and maintaining open-source projects that benefit the community. Thank you for supporting my mission to make technology better, accessible, and inclusive for everyone.🙏😇

## 📝 Author

The Forum-based Support Bot (thirdweb support) is developed and maintained by **[Waren Gonzaga](https://github.com/warengonzaga)**, with the help of awesome [contributors](https://github.com/warengonzaga/forum-based-support-discord-bot/graphs/contributors).

[![contributors](https://contrib.rocks/image?repo=warengonzaga/forum-based-support-discord-bot)](https://github.com/warengonzaga/forum-based-support-discord-bot/graphs/contributors)

---

💻💖☕ by [Waren Gonzaga](https://warengonzaga.com) | [He is Awesome](https://www.youtube.com/watch?v=HHrxS4diLew&t=44s) 🙏
