# Using GitHub Actions for browser build

These instructions allow you to build AndroidAPS with a browser.


## Setup Github AndroidAPS repository

1. Fork https://github.com/nightscout/AndroidAPS into your account. If you already have a fork of AndroidAPS in GitHub, you can't make another one. You can continue to work with your existing fork, or delete that from GitHub and then fork https://github.com/nightscout/AndroidAPS.


## Prepare your existing or new Android Studio signing keystore

1. Base64 encoding of the keystore file (on a Windows PC):\
   certutil -encode keystore.jks SIGNING_KEY.txt
2. Base64 encoding of the keystore file (on other plattforms with openssl):\
   openssl base64 < keystore.jks | tr -d '\n' | tee SIGNING_KEY.txt
3. Base64 encoding of the keystore file (on macOS):\
   base64 keystore.jks > SIGNING_KEY.txt


## Setup Github secrets for the keystore

1. In the forked AndroidAPS repository, go to Settings -> Secrets and variables -> Actions.
1. For each of the following secrets, tap on "New repository secret", then add the name of the secret, along with the value you defined during keystore creation time. As value for the secret SIGNING_KEY use the text out of the file SIGNING_KEY.txt, which is stored between the two lines --BEGIN CERTIFICATE-- and --END CERTIFICATE--.  
    * `KEY_STORE_PASSWORD`
    * `KEY_ALIAS`
    * `KEY_PASSWORD`
    * `SIGNING_KEY`


### Set up Google API access and download the JSON file
1. Go to the [Gooogle Cloud Console] (https://console.cloud.google.com)
1. Create a Google Cloud project (Project Selection Menu/New Project) with name "Google Drive API") and click Create.
2. Enable the Google Drive API [API & Services -> Library] (https://console.cloud.google.com/apis/library), search for "Google Drive API" and click Enable.
3. Create Credentials for OAuth 2.0 Client IDs. Go to [API & Services -> Credentials] (https://console.cloud.google.com/apis/credentials), click "Create Credentials" -> "OAuth Client ID". If prompted, configure the OAuth Consent Screen
 1 Choose "External" (for personal use
 2 Enter an App Name and your email address
 3 Click "Save & Continue" until you reach the last page
 4 Click "Back to Dashboard"
4. Now create your OAuth credentials:
5. Download the client_secret.json file

## Prepare your new Google API key

1. Base64 encoding of the Google API JSON key file (on a Windows PC):\
   certutil -encode client_secret.json GOOGLE_CLIENT_SECRET_JSON.txt
2. Base64 encoding of the Google API JSON key file (on other plattforms with openssl):\
   openssl base64 < client_secret.json | tr -d '\n' | tee GOOGLE_CLIENT_SECRET_JSON.txt
3. Base64 encoding of the Google API JSON key file (on macOS):\
   base64 client_secret.json > GOOGLE_CLIENT_SECRET_JSON.txt

## Setup Github secrets for the Google Drive API
1. For the Google Drive upload of the build AndroidAPS app, the following secrets have to be defined:
    * `GOOGLE_DRIVE_FOLDER_ID`
    * `GOOGLE_CLIENT_ID`
    * `GOOGLE_CLIENT_SECRET`
    * `GOOGLE_CLIENT_SECRET_JSON`
    * `GOOGLE_REFRESH_TOKEN`

1. In the forked AndroidAPS repository, go to Settings -> Secrets and variables -> Actions.
1. Add the new repository secret GOOGLE_CLIENT_SECRET_JSON.\
   As value for the secret GOOGLE_CLIENT_SECRET_JSON use the text out of the file GOOGLE_CLIENT_SECRET_JSON.txt,\
   which is stored between the two lines --BEGIN CERTIFICATE-- and --END CERTIFICATE--.  


### Setup Github secret for the upload folder on Google Drive

Go to your **Google Drive** and find the folder you want your files to be uploaded to and take note of **the folder's ID**, the long set of characters after the last `/` in your browsers address bar. Store it as new Github secret named GOOGLE_DRIVE_FOLDER_ID. If you only want to store it in the root of your Google Drive filesystem, then enter the value "root" (without quotation marks) as value for the secret GOOGLE_DRIVE_FOLDER_ID.

## Build AndroidAPS
1. On your forked AndroidAPS repository, go to Actions.
2. If not already done, activate workflow on your repository.
3. On the left side, select the workflow "Build app version to Google Drive".
4. On the right side, click on the drop down menue "Run workflow" and select "Branch: master" which is the default value.
5. Then click on "Run workflow".


## Upload the build and signed app to Google Drive (direct through the workflow)
1. When the workflow (build, sign, upload to Google Drive) is completed,
   look into your Google Drive App and see the build and signed AAPS apk file.
