Plugin Name: kiwiirc-plugin-avatar-upload
SSH Connection and Installation Steps (Run in the root directory):
bash
Kopyala
Düzenle
git clone https://github.com/ItsOnlyBinary/kiwiirc-plugin-avatar-upload.git
After running this command, a directory named /kiwiirc-plugin-avatar-upload will be created.

Now, enter the directory:

bash
Kopyala
Düzenle
cd /kiwiirc-plugin-avatar-upload
Then, run the following commands to install dependencies and build the plugin:

bash
Kopyala
Düzenle
yarn
yarn build
Once the installation is complete, a folder named dist/ will be created inside /kiwiirc-plugin-avatar-upload.

Copy all files inside /dist to /user/share/kiwiirc/plugins:

bash
Kopyala
Düzenle
cp -r /kiwiirc-plugin-avatar-upload/dist/* /user/share/kiwiirc/plugins/
UnrealIRCd Configuration (for Unreal 4-6, Inspircd may have different settings)
Edit unrealircd.conf and add the following lines:

bash
Kopyala
Düzenle
loadmodule "extjwt";
extjwt {
    method "HS256";
    expire-after 30;
    secret "j1kQiXVUngKUGH3sadsdpkR59wYOqNWB3Egtur8=";
}
Generating the "secret code":
Run this command in SSH to generate a secure key:

bash
Kopyala
Düzenle
openssl rand -base64 32
Replace the "secret" field in the configuration with the generated key.

This module will allow the web avatars to interact with the IRC daemon.

To verify if the module is loaded, run:

bash
Kopyala
Düzenle
/EXTJWT
If successful, you will receive the response "EXTJWT Not enough parameters".
Alternatively, you can check with:

bash
Kopyala
Düzenle
/module -all
If properly loaded, you should see:

bash
Kopyala
Düzenle
*** extjwt - Command /EXTJWT (web service authorization) - by UnrealIRCd Team [3RD]
KiwiIRC Plugin Configuration
Assuming you already have a plugin list, add the following lines:

Add this line to the plugin list:

json
Kopyala
Düzenle
{"name": "avatar-upload", "url": "/static/plugins/plugin-avatar-upload.js"}
Configure the plugin:

json
Kopyala
Düzenle
"plugin-avatar-upload" : {
    "api_url": "https://kiwi.zchat.org/avatars/api.php",
    "avatars_url": "https://kiwi.zchat.org/avatars/",
    "preload_avatars": false,
    "set_avatars": true
}
Important part:
The API file should be accessible at:

https://kiwi.zchat.org/avatars/api.php

The avatars directory should be at: https://kiwi.zchat.org/avatars/

API File:
You can download and save the API file from the following link:
👉 https://paste.nginx.org/8f

Save this as api.php.

For better organization, place it inside:

bash
Kopyala
Düzenle
/var/www/html/avatars/
Which means it will be accessible at:
https://kiwi.zchat.org/avatars/

Avoid unnecessary directories to keep things simple!

For easier DNS management, consider using a subdomain such as:
chat.zchat.org instead of chat.zchat.org

This will make it easier to configure Nginx settings.

Additional Requirements:
Download and upload the following files to your server:
👉 https://github.com/ItsOnlyBinary/kiwiirc-plugin-avatar-upload/tree/main/server-php

Inside this folder, ensure you have the vendor directory, as api.php requires it:

php
Kopyala
Düzenle
require 'vendor/autoload.php';
Nginx Configuration for KiwiIRC and Avatars Directory
These settings allow avatar uploads under the subdomain kiwi.zchat.org.

💡 Make sure you have an SSL certificate for kiwi.zchat.org!

After setting everything up, restart the services:

bash
Kopyala
Düzenle
systemctl restart kiwiirc
systemctl restart nginx
Final Notes:
The service is now live at:
👉 https://web.zchat.org

If you encounter errors, open Chrome, press F12, go to the Console tab, and check for issues.

You can share any errors you find, and we will help you troubleshoot them. 🚀
