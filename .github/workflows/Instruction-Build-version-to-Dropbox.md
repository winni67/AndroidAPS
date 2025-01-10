# Using GitHub Actions for browser build

These instructions allow you to build AndroidAPS with a browser.


## Setup Github AndroidAPS repository

1. Fork https://github.com/nightscout/AndroidAPS into your account. If you already have a fork of AndroidAPS in GitHub, you can't make another one. You can continue to work with your existing fork, or delete that from GitHub and then fork https://github.com/nightscout/AndroidAPS.


## Prepare your existing or new Android Studio signing keystore

1. Base64 encoding of the keystore file (on a Windows PC):\
   certutil keystore.jks SIGNING_KEY.txt
2. Base64 encoding of the keystore file (on other plattforms with openssl):\
   openssl base64 < keystore.jks | tr -d '\n' | tee SIGNING_KEY.txt


## Setup Github secrets for the keystore and the GPG encryption

1. In the forked AndroidAPS repository, go to Settings -> Secrets and variables -> Actions.
1. For each of the following secrets, tap on "New repository secret", then add the name of the secret, along with the value you defined during keystore creation time. As value for the secret SIGNING_KEY use the text out of the file SIGNING_KEY.txt, which is stored between the two lines --BEGIN CERTIFICATE-- and --END CERTIFICATE--.  
    * `KEY_STORE_PASSWORD`
    * `KEY_ALIAS`
    * `KEY_PASSWORD`
    * `SIGNING_KEY`
1. For the GPG encryption of the build AndroidAPS app, define a passphrase, which is stored in the secret GPG_PASSPHRASE
    * `GPG_PASSPHRASE`


## Build AndroidAPS
1. On your forked AndroidAPS repository, go to Actions.
2. If not already done, activate workflow on your repository.
3. On the left side, select the workflow "Build encrypted app version".
4. On the right side, click on the drop down menue "Run workflow" and select "Branch: master" which is the default value.
5. Then click on "Run workflow".


## Download the build, signed and encrypted app
1. When the workflow (build, sign, encryption) is completed, click on "Build encrypted app version" at the right side.\
   There should be a green checkmark in front of this text when the workflow was sucessfull.
2. Scroll down to the block "Annotations".
3. There, click on the download link behind the ZIP file, which contains the GPG encrypted app.
4. After the download completes, delete the ZIP file on Github:\
   click at the trash behind the GPG encrypted ZIP file.\
   This saves space at your Github account and protects your data.
6. Uncompress the ZIP file. It contains the GPG encrypted file, named app-full-release-unsigned-signed.apk.gpg
7. Decrypt the file app-full-release-unsigned-signed.apk.gpg:\
   gpg --decrypt --passphrase "value of GPG_PASSPHRASE" app-full-release-unsigned-signed.apk.gpg
8. Rename your app file if necessary.
