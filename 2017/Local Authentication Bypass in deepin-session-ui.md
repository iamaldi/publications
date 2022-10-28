# Local Authentication Bypass in deepin-session-ui 4.0.6

## Overview

`deepin-session-ui` is an Arch Linux [community package](https://archlinux.org/packages/community/x86_64/deepin-session-ui/) developed by `Wuhan Deepin Technology Co.,Ltd.` as part of the Deepin Desktop Environment (DDE) and at the time of the discovery of this vulnerability it was used by the [Manjaro Linux](https://manjaro.org/) distribution. This module handles the session user interface and some of the functionality it is responsible for example, is to handle user login, logout and account locking.

## Description
The vulnerability manifests in the `LockManager::onUnlockFinished(QDBusPendingCallWatcher *w)` function implemented at `dde-session-ui/dde-lock/lockmanager.cpp`. Specifically, as there is no restriction in the amount of asynchronous calls to `onUnlockFinished`, multiple consequent failed login attempts on the lock screen will cause D-Bus to timeout after a period of 30 seconds causing a crash of the `deepin-session-ui` module. As a result, when `deepin-session-ui` recovers from the crash it will initialize the user's desktop thus bypassing the authentication mechanism altogether.

## Impact
An adversary with physical access to the affected machine could leverage this vulnerability to bypass the authentication mechanism and gain access to local user accounts.

## Mitigation
The maintainer of this package mitigated this issue by restricting the amount of asynchronous calls to the `onUnlockFinished` function. The related commits can be located at https://github.com/linuxdeepin/dde-session-ui/commit/28e0c04d652b5f0a492b1716d7bde2065557dcbf.

## Timeline
- May 9, 2017 - Initial report to the package maintainer (https://github.com/linuxdeepin/developer-center/issues/286)
- May 9, 2017 - Vulnerability was aknowledged and triaged
- May 13, 2017 - A patch was implemented (https://github.com/linuxdeepin/dde-session-ui/commit/28e0c04d652b5f0a492b1716d7bde2065557dcbf)
- May 24, 2017 - The patch was backported to the Arch Linux `deepin-session-ui 4.0.6-2` package and to Manjaro unstable and testing branches.
