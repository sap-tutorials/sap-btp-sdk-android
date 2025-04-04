---
parser: v2
author_name: Bruce Meng
author_profile: https://github.com/flyingfish162
auto_validation: true
time: 20
tags: [  tutorial>beginner, operating-system>android, topic>mobile, programming-tool>odata, software-product>sap-btp-sdk-for-android, software-product>sap-business-technology-platform ]
primary_tag: software-product>sap-btp-sdk-for-android
keywords: sdkforandroid
---

# Add New Languages to Your Android Application
<!-- description --> See how the SAP Translation Hub can be used to quickly add support for a new language to your app.

## Prerequisites
- You completed [Try Out SAP BTP SDK Wizard for Android](sdk-android-wizard-app).


## You will learn
- An overview of how the generated app supports different languages
- How to sign up for the SAP Translation Hub
- How to use the SAP Translation Hub from within an Android Studio Project


## Intro
The SAP Translation Hub is currently not available on SAP Cloud Foundry. So if you didn't enable SAP Translation Hub service before, you can skip this tutorial.

---

### Language Support in the Generated App


The wizard generates an Android app that contains translated strings for multiple languages as shown below.

![Languages Localized](localized-strings.png)

The language codes (ar, cs, da, de, en, es, etc.) can be looked up at [Codes for the Representation of Names of Languages](http://www.loc.gov/standards/iso639-2/php/code_list.php).

Strings used within the app are read from the appropriate version of the `strings_localized` or `strings.xml` file.  The `strings.xml` file contains strings that are provided at design time, such as the screen names from `metadata.xml` or the app name. Android Studio provides a Translations Editor that you can use to view the values across the localized strings files. Right-click on the `strings.xml` file and choose **Open Translations Editor**.

>Notice in the screenshot below that the resources that are from the `strings.xml` file, such as **Suppliers**, are not translated. These will get translated in Step 3.

![Translations Editor](translations-editor.png)

The following code in the `WelcomeStepFragment` class, in the `onCreateView` method, is an example of the code used to read from a strings file.

```Kotlin
LaunchScreenSettings.Builder()
                .setHeaderLineLabel(getString(R.string.application_name))
```

The `getString` method determines what the current local language is on the device or emulator and then reads from the matching strings file.

You can set the language on an Android device or emulator using **Settings > System > Languages & input > Languages**. A second language, such as French, can also be added.

![Additional Languages](additional-languages.png)

Select the default language by long pressing on a language and dragging it to the top of the list.

![Default Language](default-language.png)

When the app is restarted, it now shows strings in the preferred language if that language is supported by the app. If the language is not currently supported, the strings will be in English by default.

![French Welcome Screen](welcome-french-screen.png)

For additional information, see:

- [Localize your app](https://developer.android.com/guide/topics/resources/localization)

- [Support different languages and cultures](https://developer.android.com/training/basics/supporting-devices/languages)

- [Localize the UI with Translations Editor](https://developer.android.com/studio/write/translations-editor)



### Sign Up for the SAP Translation Hub


The [SAP Translation Hub](https://help.sap.com/viewer/product/SAP_TRANSLATION_HUB/Cloud/en-US) enables translation of resources to specified languages. SAP Translation Hub is currently not available on SAP Cloud Foundry. Since we cannot enable the SAP Translation Hub on Cloud Foundry yet, we cannot follow Step 3 to use the SAP Translation Hub wizard to generate new languages for the project.


### Add a new Language to Your Project


>If you have enabled SAP Translation Hub service and the service URL is still available, then you can follow the steps to complete this part.

1.  Right-click on a file in the **Project Explorer** and choose **Translate Resources with SAP Translation Hub**.

    ![Translation wizard Context Menu](translation-wizard-context-menu.png)

2.  Fill in the necessary information:

    - **Account Name** – (optional) To save your settings for reuse, provide a name for this profile and select the **Save Account Information** check box. This name is used to identify the profile in the **Accounts List** available from **Accounts**.

    - **URL** – The URL of the SAP Translation Hub service. Enter the URL using the following table as guidance:

        | Global Account | URL | Sample URL |
        |:----|:----|:----|
        | Trial | **`https://saptranslation-<subaccountname>.hanatrial.ondemand.com`** | **`https://saptranslation-s0001001002trial.hanatrial.ondemand.com`** |
        | Enterprise | **`https://sap<provider subaccount name>-<subscription subaccount name>.<region host>`** | **`https://sapa5b6789cd-e77777fg7h.hana.ondemand.com`** |

    - **Username** and **Password** – The credentials of your SAP BTP account.

        ![Translation Wizard Page 1](wiz-page1.png)

3.  Click **Next** and choose **Create New SAP Translation Hub project**.

    ![Translation Wizard Page 2](wiz-page2.png)

4.  Choose the **Domain** of the application, specify the **Source Language** as English, and select the target language to add (in this case, Catalan).

    ![Translation Wizard Page 2](wiz-page3.png)

5.  Select the source file to be used for the translation, either `strings.xml` or `strings_localized.xml`.

    ![Translation Wizard Page 2](wiz-page3b.png)

6.  Press **Finish** and wait for a few moments. A pop-up window will display `Project translated successfully`.

    There should now be an additional file, `strings_localized.xml(ca)` or `strings.xml(ca)`.

    ![Translation Result](translation-result.png)

7.  In the emulator or device, set the preferred language to be Catalan, then run the app and notice that the app now displays Catalan strings.

    ![Translation Result in App](translation-result-in-app.png)

>Some of the strings in the app are part of the foundation or Fiori library and their strings are not easily accessible to be localized.  A couple of examples are the basic authentication screen and the passcode screen.

>For further information on this feature, see [SAP Translation Hub](https://help.sap.com/viewer/product/SAP_TRANSLATION_HUB/Cloud/en-US).

Congratulations! You have learned how to use the SAP Translation Hub to quickly add translations to your application!


---
