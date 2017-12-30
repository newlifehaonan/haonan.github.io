---
layout: post
title: "How to reset launchpad in mac"
date: 2017-12-27 11:30:20
tag: MacOX
description: Remove Sticky App icon and empty file in launchpad and reorganize launchpad.
comments: true
---
# How to Reset or remove Sticky icon in launchpad on Mac

**_When user deleted apps that were download from any resource other than app store, it will sometimes left its icon in launchpad. The regular way to remove or uninstall the app on MacOX doesn't work for those apps sometimes, I give the following methods to remove the sticky icon or file in launch pad_**

<hr />

## 1.Check Applications file in finder.
* Open `Applications` in `Finder` to find is there any application that has same name as the sticky icon in your launchpad?

* If so, drag the app to trashcan and check launchpad.

## 2. Reset the launchpad to default order
**_Warning! The following command will erase all icon arrangement you set before and set the order to the point when you first get your mac_**
* open the terminal and type the following command
`defaults write com.apple.dock ResetLaunchPad -bool true; killall Dock`

* wait for the dock to relaunch and Launchpad to reset

* check launchpad to see whether the problem are fixed

## 3. Delete app records from database

**_Find the database that store the launchpad icon and apply delete query to delete record from database_**

* Find `com.apple.dock.launchpad` file

  * open finder type `command` + `shift` + `G`

  * enter `/private/var/folders`

  * In folders dictionary try to find a file called `com.apple.dock.launchpad` within which find `db` file.

* DDL db file

  * cd in the path of `com.apple.dock.launchpad`

  * `sqlite3 db`

  * `delete from apps where title='appname';`

* Restart your computer

* The problem should be fixed

<hr>

_CopyRight &copy;Newlifehaonan.com_
