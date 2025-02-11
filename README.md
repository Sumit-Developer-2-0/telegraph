🎉 The image/video/file hosting project based on R2 storage is complete, welcome to deploy and test 👉[JSimages](https://github.com/0-RTT/JSimages)

# Introduction

An image/video/file hosting service based on Cloudflare Workers, Pages, and TG_BOT.

## Features

- Optional visitor authentication
- Optional image compression (enabled by default)
- Optional file size limit (20MB by default)
- Supports viewing local history
- Supports uploading all file formats
- Supports multi-file uploads and paste uploads
- Supports batch operations and display of upload time
- Cloudflare Cache API cache support
- File storage based on Telegram Bot API

## Changelog

> **Recent Update**: 2024-12-18
> - Updated the style of the management interface
> - Removed front-end file type and file size restrictions
> - Controlled the size of uploaded files through environment variables

<details>
<summary>History Update Records</summary>

### 2024-12-18
- Updated the style of the management interface
- Removed front-end file type and file size restrictions
- Controlled the size of uploaded files through environment variables

### 2024-12-17
- Added a compression button to the front end, used to control the compression function, which is enabled by default.

### 2024-12-13
- Used hash check to avoid duplicate uploads.
- Adjusted the compression rate to 0.75, and removed the resolution limit.
- Added authentication checks to the delete interface `/delete-images`.

### 2024-11-29
#### Management Page
- Added select all and copy functions
- Added a second confirmation before deletion
- Optimized resource loading logic
- Disabled automatic playback of video files
#### Homepage
- Fixed the issue where the remove button was not displayed during paste uploads

### 2024-11-21
- Optimized the upload experience, enabling compression by default to speed up file uploads
  - To disable, modify line 238 of the code to ```enableCompression: false```

### 2024-11-01
- Fixed the issue where files could not be loaded after uploading

### 2024-10-19
- Fixed the bug that webp could not be uploaded
- Optimized the database structure, [view migration tutorial](https://github.com/0-RTT/telegraph/releases/tag/v2.0)

### 2024-09-29
- Optimized the cache function, using Cloudflare Cache API cache support

### 2024-09-25
- Fixed the issue of uploading GIF files, thanks to [nodeseek](https://www.nodeseek.com/) user [@Libs](https://www.nodeseek.com/space/7214#/general) for providing the idea
- Telegraph interface moved to the telegraph branch, the main branch is the TG_BOT interface, which can be directly forked to the pages

### 2024-09-23
- Fixed link failures, supports uploading video files

### 2024-09-14
- Files uploaded through the Telegraph interface have **time limits**, it is recommended to use TG_BOT to upload

### 2024-09-13
- Supports uploading to the channel through TG_BOT

### 2024-09-12
- Fixed, can be uploaded to telegraph normally

### 2024-09-06
> ~~From September 6, 2024, telegra.ph has prohibited the uploading of media files, this project is terminated.~~

</details>

## Deployment Steps

### 1. Variable Description
You need to configure the following environment variables in Cloudflare Workers:

| Variable Name | Description | Required | Example |
|---------------|-------------|----------|---------|
| DOMAIN        | Custom domain | Yes      | example.workers.dev |
| DATABASE      | D1 database binding variable name | Yes      | DATABASE |
| TG_BOT_TOKEN  | Telegram Bot Token | Yes      | 123456789:ABCdefGHIjklMNOpqrsTUVwxyz |
| TG_CHAT_ID    | Telegram Channel/Group ID | Yes      | -100xxxxxxxxxx |
| USERNAME      | Administrator username | Yes      | admin |
| PASSWORD      | Administrator password | Yes      | password123 |
| ADMIN_PATH    | Admin backend path | Yes      | admin |
| ENABLE_AUTH   | Visitor authentication (set to true to enable, leave blank or set to false to disable) | No       | false |
| MAX_SIZE_MB   | Maximum single file size supported (unit: MB, default value is 20) | No       | 20 |

### 2. Create a Telegram Bot
1. Find [@BotFather](https://t.me/BotFather) in Telegram
2. Send the `/newbot` command to create a new bot
3. Follow the prompts to set the bot name and username
4. Save the obtained Bot Token (format is `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`)
   - This Token will be used as the environment variable `TG_BOT_TOKEN`

### 3. Create a Telegram Channel or Group
1. Create a new channel or group
2. Add your Bot as an administrator
3. Get the channel/group ID:
   - Send any message within the channel to [@getidsbot](https://t.me/getidsbot)
   - Find the corresponding ID in the Origin chat (format is `-100xxxxxxxxxx`)
   - This ID will be used as the environment variable `TG_CHAT_ID`

### 4. Create a D1 Database
1. Log in to [Cloudflare Dashboard](https://dash.cloudflare.com)
2. Go to `Workers & Pages` → `D1 SQL Databases`
3. Click `Create` to create a database
   - The database name can be customized, such as `images`
   - It is recommended to select the database location as `Asia Pacific` to get better access speed
4. Create a data table:
   - Click the database name to enter the details page
   - Select the `Console` tab
   - Execute the following SQL statement:
```sql
CREATE TABLE media (
    url TEXT PRIMARY KEY,
    fileId TEXT NOT NULL
);
