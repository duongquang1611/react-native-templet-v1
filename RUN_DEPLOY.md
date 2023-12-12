<h1>Document run and deploy app react native</h1>

**This guide builds for Mac OS, does not support Window OS**
_The running and deploying instructions below are written based on the PIT PORT project, not the sample project._

<h2>Table of Contents</h2>

- [Setup Environment](#setup-environment)
- [Getting Started](#getting-started)
- [Getting Started Android](#getting-started-android)
- [Getting Started iOS](#getting-started-ios)
- [Project's structures](#projects-structures)
    - [Introduction](#introduction)
    - [Main folders, files](#main-folders-files)
- [Deploy](#deploy)
  - [Android](#android)
  - [iOS](#ios)

## Setup Environment

Install the environment (Android, IOS) by following the link: [Setup environment](https://reactnative.dev/docs/environment-setup)

## Getting Started

**The application has 3 environments:**

- **Development**: develop and fix bugs
  Env file: `.env.development`

- **Staging**: environment to test again before pushing to production
  Env file: `.env.staging`

- **Production**: environment users can use
  Env file: `.env.production`

The instructions below will guide you through running each environment in development.
You need to choose the correct environment when running the project and when building to deploy the app.

## Getting Started Android

Run app android debug in emulator (You just need to choose 1 of the commands below to be able to run the app):

- Development:

  ```
  yarn android-run-debug-develop
  ```

- Staging:

  ```
  yarn android-run-debug-staging

  ```

- Production:
  ```
  yarn android-run-debug-product
  ```

The command will execute the application on the android emulator

**Result**

<img title="Result" alt="Result" src="./assets/images/result_android.png" style="display: block; margin-left: auto; margin-right: auto; height: 500px; width: auto">

## Getting Started iOS

Run app iOS debug in simulator

1. Open `ios/PitPort.xcworkspace` by [Xcode](https://xcodereleases.com/)
2. Choose iOS simulator you want to run the app
   <img title="choose-simulator" alt="Choose simulator" src="./assets/images/choose_ios_simulator.png">
3. Choose environment you want to run the app

- Development: **`PitPort DEV`**
- Staging: **`PitPort STG`**
- Production: **`PitPort PROD`**
  <img title="choose-env" alt="Choose Environment" src="./assets/images/choose_env.png">

4. Click icon build
   <img title="choose-simulator" alt="Choose simulator" src="./assets/images/choose_build.png">

5. Result

<img title="Result" alt="Result" src="./assets/images/result_ios.png" style="display: block; margin-left: auto; margin-right: auto; height: 500px; width: auto">

## Project's structures

#### Introduction

- React-native version 0.72.4
- Fully using typescript for typing
- Folder structure using package-by-feature
- Redux-toolkit, redux-saga, redux-persist, redux-logger
- React-navigation v6
- 100% functional component with hooks
- A lot of custom/base components
- i18next for multiple languages
- Custom hooks for share state logic between components
- Eslint with Prettier plugin for checking code convention
- Husky for pre-commit (we check lint to make sure we have no errors first when commit)

#### Main folders, files

- **/patches**: contains patches to customize libraries by [patch-package](https://www.npmjs.com/package/patch-package)
- **.env**: env sample, env includes config app name, package name android, bundle id iOS, version app when build, other config key (codepush, maps, ...), api url, dynamic links.
- **/environment**: contains multiple environments of the project
  - **.env.development**: env development
  - **.env.staging**: env staging
  - **.env.production**: env production
- **/src**: contains the main source of the project

  - **/api**: config request, api
  - **/app-redux**: config redux
  - **/assets**: images, config themes, metrics
  - **/components**: components that share the project
  - **/feature**: main features of the project

    - **/authentication**: feature authentication
    - **/home**: feature map, create reservation
    - **/parking**: start, stop, extend time reservation
    - **/reservation**: reservation history, detail reservation, edit reservation
    - **/setting**: edit phone, edit number plate, list notification, contact, terms, ...

  - **/hooks**: custom hooks shared in the project
  - **/navigation**: config navigation, bottom tabs, ...

  - **/utilities**
    - **/authenticate**: config services authenticate
    - **/notification**: config service notification
    - **/permissions**: config flow request permissions in app
    - **/i18next.ts**: config language in app
    - **/helper.ts**, **/format.ts**, **/validate.ts**, ...: config functions shared in app

## Deploy

### Android

**Reference** See full setup <br>
[![](https://markdown-videos-api.jorgenkh.no/youtube/295bzuj02BI)](https://youtu.be/295bzuj02BI)

1. Edit the android app version you want to push the app to Google Play in env file: `env.production`
   Includes 2 fields: The thought of each school can be read more here [Version App](https://developer.android.com/studio/publish/versioning)

   - **ANDROID_APP_VERSION_NAME**: need to increase 1 version compared to the version already on Google Play.
     Example: `1.0.0` => `1.0.1`
   - **ANDROID_APP_VERSION_CODE**: need to increase 1 version code compared to the version code already on Google Play.
     Example `1.0.0 (1)` => `1.0.0 (2)`

2. Generate keystore if you do not have a release keystore file in the `android` folder. [Follow this steps](https://docs.google.com/document/d/1sPg4N7iXEgD_NzbXBRD_SzHPo4p48uJIgG_fC9hK48s/edit#heading=h.gco98qoy1a9b)

3. Build `aab` file

   - Checkout branch `production`
     ```
     git checkout production
     ```
   - Build `aab` file

     ```
     yarn android-build-aab-product
     ```

4. Open the folder containing the file `aab` just built

   ```
   open-folder-aab
   ```

5. Sign in [Google Play Console](https://play.google.com/console/u/0/developers) and open PIT PORT app.
   _In this step you need to have a google account added in the google developer group of your company (Example: Amela Technology)_

![Open app in Google Play Console](./assets/images/open_app_gg.png)

1. Upload **Internal testing**: Test again before pushing to Google Play Store

   - Click **Internal testing**
   - Click **Create new release**
   - Upload the `aab` file you just built in the previous part
   - Enter **Release name** and **Release notes** (Release Note is the update information in this release)
   - Click **Review release**
   - Click **Start rollout to Internal Testing**
   - Add testers to test again before uploading to Store: Tester can find internal testing app in Google Play
     ![Add tester](./assets/images/add_tester_google.png)
   - Tester accept invite: click **Copy Link** and Send to tester is added
     ![Accept Invite](./assets/images/accept_invite_google.png)

2. Upload app to Google Play Store

   - Click **Production**
     ![Click Production](./assets/images/internal_to_production.png)
   - Review Release: Update info release => Click **Review Release**
     ![Review release](./assets/images/edit_release.png)
   - Click **Start Rollout**
     ![Start Rollout](./assets/images/start_rollout.png)
   - Waiting for Google to review the app
     ![Waiting Review](./assets/images/waiting_review.png)
   - After Google reviews the app, if it is not rejected, you can publish the app to Google Play Store under `Publishing Overview` => Click `Publish Changes`
     ![Publish Changes](./assets/images/publish_changes.png)

### iOS

**Reference** See full setup <br>
[![](https://markdown-videos-api.jorgenkh.no/youtube/fXeDe9tafG8)](https://youtu.be/fXeDe9tafG8)

1. Create Apple Certificate, Provisioning in [Apple Developer](https://developer.apple.com/account) by [How to Create Apple Developer Certificate, Provisioning profiles and AppID's?](https://www.youtube.com/watch?v=oK1b-H6rh08)
   _No need to create AppId because the AppId com.pit-port is already created corresponding to the app PIT PORT_

2. Edit the android app version you want to push the app to AppStore in env file: `env.production`
   Includes 2 fields: The thought of each school can be read more here [Version App](https://stackoverflow.com/a/38009895)

   - **IOS_APP_VERSION_CODE**: need to increase 1 version compared to the version already on AppStore.
     Example: `1.0.0` => `1.0.1`
   - **IOS_APP_BUILD_CODE**: If there is an unpublished version 1.0.0(1) on the AppStore, you need to increase the version to 1.0.0(2). If there is a published version 1.0.0(1) on the AppStore, you need to increase the version to 1.0.1(1).

3. Build App Production

   - Checkout branch `production`
   - Select scheme `PitPort PROD` and `Build: Any IOS device` in XCode
     ![Scheme Prod](./assets/images/scheme_prod.png)
   - `Clean Build Folder` => `Archive`
     ![Clean Build](./assets/images/clean_build.png)
     ![Archive](./assets/images/archive_ios.png)
   - Archive Done
     ![Result Archive](./assets/images/result_archive.png)
     _If you don't see the build result screen, you can choose Window => Organizer_
     ![Open Result Archive](./assets/images/open_result_archive.png)
   - Upload app to [TestFlight](https://appstoreconnect.apple.com/apps/1632578357/testflight/ios):<br>
     [![](https://markdown-videos-api.jorgenkh.no/youtube/jscv2fAVWbk)](https://youtu.be/jscv2fAVWbk)

   - Upload done => Waiting Apple Process New Version
     ![Process New Version](./assets/images/process_new_version.png)

4. Upload to AppStore

   - After re-testing the app version on TestFlight, you can get that version of the app to push to the AppStore according to the video:
     [![](https://markdown-videos-api.jorgenkh.no/youtube/wHCSnosyVBg)](https://youtu.be/wHCSnosyVBg)
   - Click `Add for review` to Apple review new version app.
   - After Apple reviews the app, if it is not rejected, you can publish the app to **AppStore**.
