# Uploading to TestFlight

In this lab we'll walk through how to upload an app to TestFlight and invite others to test.

## Create an App Id

[Apple Developer Website](https://developer.apple.com/account/)

![screen shot 2018-02-18 at 8 49 12 am](https://user-images.githubusercontent.com/206423/36352560-c39ad6d0-1488-11e8-9758-78dc8ef6c02c.png)


Select *Certificates, IDs & Profiles* -> *App IDs* -> [Plus Icon in upper right]

Provide an App ID Description.
![screen shot 2018-02-18 at 9 03 36 am](https://user-images.githubusercontent.com/206423/36352689-c36af328-148a-11e8-931c-197f7238b6cc.png)


Provide a Bundle ID. 

Confirm and Register.
![screen shot 2018-02-18 at 9 03 50 am](https://user-images.githubusercontent.com/206423/36352693-d1bc38f6-148a-11e8-9b85-0b9dcb038c38.png)


![screen shot 2018-02-18 at 9 07 57 am](https://user-images.githubusercontent.com/206423/36352721-3bb2f8b2-148b-11e8-8989-1e7456937fb5.png)

## Create a New App on iTunes Connect

Login into [iTunes Connect](https://itunesconnect.apple.com/)
![screen shot 2018-02-18 at 9 20 47 am](https://user-images.githubusercontent.com/206423/36352868-07766e9c-148d-11e8-9fcb-ab30246a1ed9.png)

Select *My Apps*
![screen shot 2018-02-18 at 9 22 54 am](https://user-images.githubusercontent.com/206423/36352893-52cd09be-148d-11e8-8367-f995818cd850.png)


Create a new app and associate Bundle ID.

The SKU can be anything.
![screen shot 2018-02-18 at 9 24 13 am](https://user-images.githubusercontent.com/206423/36352911-827172fe-148d-11e8-93eb-a4addeea59bc.png)


## Upload app to iTunes Connect

Add this key to the *info.plist*

Make sure to **SAVE** your project after update.

![screen shot 2018-02-18 at 9 26 40 am](https://user-images.githubusercontent.com/206423/36352952-fd35b2de-148d-11e8-9d1b-49847acc7d3d.png)



Set project Bundle ID and make sure the correct Team is set.
![screen shot 2018-02-18 at 9 09 07 am](https://user-images.githubusercontent.com/206423/36352742-9ae7d0b4-148b-11e8-851a-25aac125f26d.png)


Set scheme to "Generic iOS Device"
![screen shot 2018-02-18 at 9 13 35 am](https://user-images.githubusercontent.com/206423/36352782-25e9ef6c-148c-11e8-85af-853eb643883c.png)


Archive the project.

![screen shot 2018-02-18 at 9 16 36 am](https://user-images.githubusercontent.com/206423/36352809-760ca296-148c-11e8-990f-c78a0a240e17.png)


The *Organizer* window will open. 

Select *Upload to App Store*
![screen shot 2018-02-18 at 9 17 36 am](https://user-images.githubusercontent.com/206423/36352814-9a11973c-148c-11e8-9d41-e7e83d54b5eb.png)

Select all the default options. This will take a few minutes. 

If you are prompted to input the system password, select "Always Allow"
![screen shot 2018-02-18 at 9 28 49 am](https://user-images.githubusercontent.com/206423/36353012-cdec4258-148e-11e8-8516-3711e60a1b98.png)


Upload successful!
![screen shot 2018-02-18 at 9 32 55 am](https://user-images.githubusercontent.com/206423/36353007-b8a5a2fe-148e-11e8-803c-05c98adb686e.png)


## Check app upload on iTunes Connect.

Select *Activity* tab for app.

You'll get an email when it's done processing.
![screen shot 2018-02-18 at 9 34 49 am](https://user-images.githubusercontent.com/206423/36353024-fd748d32-148e-11e8-96bf-99f1ebe48c4e.png)


If you get a message saying *Missing Compliance*, click on the icon and select *No*.

Setting the flag in the *info.plist* should prevent from having to complete this step.
![screen shot 2018-02-18 at 9 45 14 am](https://user-images.githubusercontent.com/206423/36353266-4c8bd378-1492-11e8-9739-75a191edbab9.png)

![screen shot 2018-02-18 at 9 48 04 am](https://user-images.githubusercontent.com/206423/36353273-6096f316-1492-11e8-9ecd-ab0c128cd728.png)



## Add users for testing.

Select *Users and Roles* from the main menu and add a new user.

![screen shot 2018-02-18 at 9 39 10 am](https://user-images.githubusercontent.com/206423/36353065-97c9b07e-148f-11e8-95ef-7d2bc364b50f.png)


Set the Role for *Marketer* and select your app. You can also give access to *All* apps for the user.

Follow the prompts and use the default settings.

Do this for all your testers. You'll only have todo this once.
![screen shot 2018-02-18 at 9 39 48 am](https://user-images.githubusercontent.com/206423/36353070-b14c8e22-148f-11e8-893e-48aa8650f485.png)


Select the app for testing and then select the *iTunes Connect Users* option on the left side.

Select the users you wish to test the app.
![screen shot 2018-02-18 at 9 49 38 am](https://user-images.githubusercontent.com/206423/36353281-7a59d278-1492-11e8-81a7-03250db5a25e.png)


The users will get an email message with instructions.

They will need to download the TestFlight app from the app store and enter a *Redeem* code.

![img_2626](https://user-images.githubusercontent.com/206423/36353306-cd5d9c20-1492-11e8-8a04-c5c7c7405823.png)




<!--
  Image references
-->