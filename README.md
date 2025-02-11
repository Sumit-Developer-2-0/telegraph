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
```

### 5. Create a Worker
1. Go to `Workers & Pages`.
2. Click `Create`.
3. Select `Create Worker`.
4. Set a name for the Worker.
5. Click `Deploy` to create the Worker.
6. Click "Continue to project".

### 6. Configure Variables and Secrets
1. In the Worker's `Settings` → `Variables and Secrets`.
2. Click `Add` individually to add the following variables as needed:
   - DOMAIN
   - TG_BOT_TOKEN
   - TG_CHAT_ID
   - USERNAME
   - PASSWORD
   - ADMIN_PATH
   - ENABLE_AUTH (Optional)
   - MAX_SIZE_MB (Optional)
3. Click `Deploy`.

### 7. Bind a Database
1. In the Worker settings page, find `Settings` → `Bindings`.
2. Click `Add` to add the following variable name:
   - DATABASE
3. Click `Deploy`.

### 8. Bind a Domain
1. In the Worker's `Settings` → `Domains & Routes`.
2. Click `Add` → `Custom Domain`.
3. Enter the domain you have bound to Cloudflare.
4. Click `Add domain`.
5. Wait for the domain to take effect.

### 9. Deploy the Code
1. Go to your worker project → Click "Edit code".
2. Copy and paste the complete code from `_worker.js` into the editor.
3. Click `Deploy`.

## Deployment Step Reference:

> ⚠️ The images below are for reference only. The Cloudflare panel may be updated.  Please refer to the text tutorial above for specific operations.

> 💡You can also refer to the [graphic tutorial](https://www.nodeseek.com/post-196832-1) by nodeseek user @sdo888.

### Worker Deployment Example

#### 1. Initialize Database
![image](https://kycloud3.koyoo.cn/20241007ae0fa202410070917194587.png)  

![image](https://kycloud3.koyoo.cn/202410074b824202410070851275140.png)  
 
![image](https://kycloud3.koyoo.cn/20241007917fa202410070852019143.png)  
 
![image](https://kycloud3.koyoo.cn/20240829426e2202408291111415611.png)  

![image](https://kycloud3.koyoo.cn/202408290028f20240829111205448.png)  

#### 2. Create Worker
![image](https://kycloud3.koyoo.cn/202408295c74a202408291112222566.png)

![image](https://kycloud3.koyoo.cn/20240829b4a21202408291118209822.png)

#### 3. Set Custom Domain
![image](https://kycloud3.koyoo.cn/20240829d5fe4202408291113048235.png)

![image](https://kycloud3.koyoo.cn/20240829f9ecc202408291113197734.png)

![image](https://kycloud3.koyoo.cn/2024082997a84202408291113394516.png)

![image](https://kycloud3.koyoo.cn/202408294223e202408291114234528.png)

![image](https://kycloud3.koyoo.cn/202408294def5202408291113564340.png)

#### 4. Set Variables
![image](https://kycloud3.koyoo.cn/2024092389dc0202409232021524424.png) 

#### 5. Copy and paste the code from _worker.js into the editor
![image](https://kycloud3.koyoo.cn/202408299f1cf202408291115372291.png)

![image](https://kycloud3.koyoo.cn/2024082995808202408291115555979.png)

#### 6. Click Deploy
![image](https://kycloud3.koyoo.cn/20240829a4d5f202408291117024227.png)

## Pages Deployment Tutorial:

#### 1. Initialize Database
![image](https://kycloud3.koyoo.cn/20241007ae0fa202410070917194587.png)  

![image](https://kycloud3.koyoo.cn/202410074b824202410070851275140.png)  
 
![image](https://kycloud3.koyoo.cn/20241007917fa202410070852019143.png)  
 
![image](https://kycloud3.koyoo.cn/20240829426e2202408291111415611.png)  

![image](https://kycloud3.koyoo.cn/202408290028f20240829111205448.png)  

#### 2. Deploy to Pages

![image](https://kycloud3.koyoo.cn/20241007f786a202410070857578208.png)

- 2.1 Download _worker.js, package it into a zip file, and upload it to Pages.

![image](https://kycloud3.koyoo.cn/2024100790232202410070900405992.png)

- 2.2 Deploy to Pages by forking this repository.
![image](https://kycloud3.koyoo.cn/20241007d7bf6202410070902287155.png)
![image](https://kycloud3.koyoo.cn/20241007a4b2f202410070902288891.png)

#### 3. Set Variables
![image](https://kycloud3.koyoo.cn/2024092389dc0202409232021524424.png) 

#### 4. Set Custom Domain.
![image](https://kycloud3.koyoo.cn/202409068f76a202409061718122696.png)  

![image](https://kycloud3.koyoo.cn/20240906b79a6202409061719043430.png)  

![image](https://kycloud3.koyoo.cn/20240906188f8202409061720032928.png)  

#### 5. Re-deploy to apply the configured custom domain and variables.

![image](https://kycloud3.koyoo.cn/202409066761e202409061721281588.png)  

![image](https://kycloud3.koyoo.cn/2024090677f2320240906172317323.png)  

![image](https://kycloud3.koyoo.cn/202409065c29920240906172451915.png)  

## Open Source License

MIT License

## Acknowledgements

- [Cloudflare](https://www.cloudflare.com/)
- [Telegram](https://telegram.org/)
