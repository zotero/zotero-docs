---
tags:
  - kb
  - basics
---

# How Do I Install Zotero on a Chromebook?

#### Step 1: Set up Linux on Chrome OS

Following the steps from Google to [Set up Linux on your Chromebook](https://support.google.com/chromebook/answer/9145439?hl=en).

#### Step 2: Open Terminal

1.  After Linux is installed, you will notice a new app in your overflow menu (where all your app icons live) called Terminal.
2.  Wait for the Terminal app to open. This might take a few minutes the first time.

#### Step 3: Install Zotero

Enter these commands in Terminal to install a packaged version of Zotero [maintained by a community member](https://github.com/retorquere/zotero-deb):

    curl -sL https://raw.githubusercontent.com/retorquere/zotero-deb/master/install.sh | sudo bash
    sudo apt update
    sudo apt install zotero

(If you prefer, you can install the [official tarball](/download), but you will have to perform some [setup steps](installation#linux) manually.)

Once these finish, you can close the Terminal and go back to your overflow (apps) menus. You will now see an icon for Zotero, and clicking on it will open the app. You can then pin the app to your Chrome Launcher.
