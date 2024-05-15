---
author_name: Haoyue Feng
author_profile: https://github.com/Fenghaoyue
title: Create Your Own Flow
description: Learn how to create your own flow with the framework provided by Jetpack Compose Flows component of SAP BTP SDK for Android.
auto_validation: true
time: 20
tags: [ tutorial>intermediate, operating-system>android, topic>mobile, products>sap-business-technology-platform]
primary_tag: products>sap-btp-sdk-for-android
---

## Prerequisites
- You have [Set Up a BTP Account for Tutorials](group.btp-setup). Follow the instructions to get an account, and then to set up entitlements and service instances for the following BTP services.
    - **SAP Mobile Services**
- You completed [Get Familiar with Jetpack Compose Flows Component by a Wizard Generated Application](sdk-android-flowsjc-wizard).
- You completed [Customize the Jetpack Compose Onboarding Flow](sdk-android-flowsjc-onboarding).
- You completed [Handle Passcode with JetPack Compose Flows Component](sdk-android-flowsjc-passcode).
- You completed [Restore, Reset and Logout Applications Using JetPack Flows Component](sdk-android-flowsjc-restore).

## Details
### You will learn
  - How to create your own flow
  - How to add flow steps to your own flow

Besides the pre-defined flows described in the previous tutorials, the flows component also provides a framework for the client code to create its own flow. Typically, a flow consists of several flow steps, and also it might also include other flows as sub-flows. The Flows framework provides flexible ways for the client code to create a customized flow:

- The client code can create its own flow steps and then create a flow with the combination of the steps.
- The client code can create a flow with some single flow steps or combine sub-flows to create a customized flow.
- The client code can define the navigation logic for a customized flow.

---

[ACCORDION-BEGIN [Step 1: ](Write your own flow)]

Extend your flow class to the **`com.sap.cloud.mobile.flows.compose.flows.BaseFlow`** class, and implement the **`initialize`** function to add flow steps.

1. Create a flow class named **`TestFlow`**:

    ```Kotlin
    class TestFlow(context: Context) : BaseFlow(context, "TestFlow") {
            
    }
    ```

2. Implement the **`initialize`** function to create and add a customized flow step to your own flow:

    ```Kotlin
    override fun initialize() {
        val step1 = SingleStep(context, "step_1") {
            EulaScreen(
                eulaContentLocale = localeState.value,
                eulaScreenSettings = EulaScreenSettings(
                    eulaUrl = "file:///android_res/raw/custom_eula.html",
                    agreeButtonCaption = android.R.string.ok,
                    disagreeButtonCaption = android.R.string.cancel
                ),
                agreeViewClickListener = {
                    flowDone("step_1")
                },
                disagreeViewClickListener = {
                    terminateFlowWithMessage("Custom flow cancelled.")
                },
                webViewClient = object : WebViewClient() {
                    override fun shouldOverrideUrlLoading(
                        view: WebView?,
                        request: WebResourceRequest?
                    ): Boolean =
                        request?.url?.let { url ->
                            SDKCustomTabsLauncher.launchCustomTabs(context, url.toString())
                            true
                        } ?: false
                }
            )
        }

        addSingleStep(step1)
    }
    ```
[ACCORDION-END]

[ACCORDION-BEGIN [Step 2: ](Add flow steps to your flow)]

1. You can either add a single step or a nested flow with following methods:

    **`fun addSingleStep(step: SingleStep, secure: Boolean = false)`**
    **`fun addNestedFlow(flow: BaseFlow)`**

    In the flow created in the first section, we implemented a customized flow step "step1" and added the step with **`addSingleStep`** function. Now we can create another flow named "SubFlow" and add it as a sub-flow for "TestFlow" that we created in the first section:

    ```Kotlin
    class SubFlow(context: Context) : BaseFlow(context, "SubFlow") {
        override fun initialize() {
            val step1 = SingleStep(context, "step_1") {
                ConsentScreen(
                    consentType = "CustomConsent",
                    consentScreenSettings = ConsentScreenSettings(
                        consentType = "CustomConsent",
                        description = R.string.demo_custom_consent_description,
                        agreeButtonCaption = android.R.string.ok,
                        disagreeButtonCaption = android.R.string.cancel
                    ),
                    agreeButtonClickListener = {
                        flowDone("step1_SubFlow")
                    },
                    disagreeButtonClickListener = {
                        terminateFlowWithMessage("Cancel the custom flow.")
                    }
                )
            }

            addSingleStep(step1)
        }
    }
    class TestFlow(context: Context) : BaseFlow(context, "TestFlow") {
        override fun initialize() {
            val step1 = SingleStep(context, "step_1") {
                ...
            }

            addSingleStep(step1)
            addNestedFlow(SubFlow(context = context))
        }
    }
    ```

2. When a flow starts, by default the app will navigate to the first step added in the **`initialize`** function. If the first step should be determined at runtime, you can override the **`start`** function of **`BaseFlow`** and define your own logic for the flow start step. To navigate among your steps, you can use the **`navigateTo`** function.

    There are two steps in the flow created in the first section. The first step includes a EULA screen, and the second step is a sub-flow with consent screen. If the first step must be excluded, you can use the following code to navigate to the sub-flow when the first step with EULA screen is excluded: 

    ```Kotlin
    override fun start(popRoute: String?) {
        if(eulaExcluded) {
            navigateTo("SubFlow", popRoute)
        }     
    }
    ```

For more information, refer to [Write Your Own Flow](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/features/onboarding/android/compose-flows/CustomFlow.html).

Congratulations! You have learned how to create your own flow using the Jetpack Compose Flows component!

[ACCORDION-END]