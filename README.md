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

#### Step 5: Configure Spreadsheet Structure

After creating your spreadsheet, you need to set up the proper structure for data collection:

##### Required Sheets

Create **3 sheets** with these exact names (matching your `config.json`):

1. **"init"** - Main data sheet (tracks all forum posts)
2. **"response"** - First response tracking
3. **"resolve"** - Resolution tracking

##### Sheet Headers Setup

**"init" Sheet (Row 1 Headers):**

```text
post_id | post_link | question | posted_by | posted | tags | responder | first_response | resolution_time | resolved_by
```

**"response" Sheet (Row 1 Headers):**

```text
post_id | first_response | responder
```

**"resolve" Sheet (Row 1 Headers):**

```text
post_id | resolution_time | resolved_by
```

##### Data Flow Overview

| Sheet | When Data is Added | What Gets Recorded |
|-------|-------------------|-------------------|
| **init** | New forum post created | Post details, tags, timestamps (formulas auto-link other data) |
| **response** | Support team first responds | Response time and responder username |
| **resolve** | `!close` command used | Resolution time and who resolved it |

##### Column Details

**"init" Sheet Columns:**

| Column | Field | Auto-Filled | Description |
|--------|-------|-------------|-------------|
| A | `post_id` | ✅ | Discord thread ID |
| B | `post_link` | ✅ | Direct link to Discord post |
| C | `question` | ✅ | Forum post title/question |
| D | `posted_by` | ✅ | Username who created the post |
| E | `posted` | ✅ | Creation timestamp (UTC-8) |
| F | `tags` | ✅ | Forum tags applied to post |
| G | `responder` | 🔗 | First responder (VLOOKUP formula) |
| H | `first_response` | 🔗 | First response time (VLOOKUP formula) |
| I | `resolution_time` | 🔗 | Resolution time (VLOOKUP formula) |
| J | `resolved_by` | 🔗 | Who resolved it (VLOOKUP formula) |

**"response" Sheet Columns:**

| Column | Field | Auto-Filled | Description |
|--------|-------|-------------|-------------|
| A | `post_id` | ✅ | Discord thread ID |
| B | `first_response` | ✅ | First response timestamp |
| C | `responder` | ✅ | Support team member username |

**"resolve" Sheet Columns:**

| Column | Field | Auto-Filled | Description |
|--------|-------|-------------|-------------|
| A | `post_id` | ✅ | Discord thread ID |
| B | `resolution_time` | ✅ | Resolution timestamp |
| C | `resolved_by` | ✅ | Who closed the post |

> **Legend:** ✅ = Automatically filled by bot | 🔗 = VLOOKUP formula links data from other sheets

##### Analytics Capabilities

Once set up, your spreadsheet will automatically track:

- **Response Times** - How quickly support team responds
- **Resolution Times** - How long issues take to resolve  
- **Team Performance** - Individual support member metrics
- **Popular Topics** - Most common tags and question types
- **Support Workflow** - Complete timeline from post to resolution

The **"init" sheet serves as your main dashboard** with all data connected through formulas, giving you comprehensive support analytics in one view.

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
