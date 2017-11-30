# HDX SDK for Citrix Receiver for Chrome 2.6

Citrix Receiver for Chrome provides the HDX SDK as a JavaScript file that you can include in third-party Chrome apps. The HDX SDK provides an API to launch and interact with the XenApp and XenDesktop sessions from third-party Chrome apps.

The HDX SDK for Chrome provides the following capabilities:

* Embed Receiver sessions in third-party apps that are running in kiosk mode on Google Chromebooks.
* Launch an HDX SDK session in regular mode (opens the session in a new window) or in embedded mode inside third-party Chrome apps (using appview).
* Receive events from a session on connect, disconnect, and errors.
* Change the resolution of the launched session dynamically.
* Send special key combinations in an active session, - for example, Ctrl+Alt+Del, Ctrl+Shift+Esc.
* Show or hide a launched session.
* Send Disconnect /Log off commands from a session.

For the latest HDX SDK APIs and examples, see the [download](https://www.citrix.com/downloads/citrix-receiver/html5.html?_ga=1.58227882.1697021148.1491559074) page.

For HDX SDK API documentation for Chrome, see [Capabilities](./capabilities.md).
 
## Prerequisites 

Citrix Receiver for Chrome supports only the whitelisted third-party Chrome apps. You can whitelist a third-party Chrome app by adding the policy file for Citrix Receiver for Chrome using Chrome management settings

To whitelist a third-party Chrome app, do the following: 

1.	Install the latest version of Citrix Receiver for Chrome. See Citrix downloads page for details.

2.	Whitelist the third-party Chrome app by adding the policy file for Citrix Receiver for Chrome using Chrome management settings.

The sample policy.txt file to whitelist the third-party Chrome app is as below:

```json
{
	"settings": {

		 "Value": {

			 "settings_version": "1.0",

			 "store_settings": {

					"externalApps": [“<3rdParty_App1_ExtnID>”,“<3rdParty_App2_ExtnID>”]


            }


        }


    }


}
```

!!!tip "Note"
		&lt;3rdParty_App1_ExtnID&gt; is used as an example for the name of externalApps and can send messages to Citrix Receiver for Chrome. Get your appid from the `chrome://extensions` site.

## Embedding Receiver for Chrome sessions within a third-party app in kiosk mode

To embed a Receiver session within a third-party app that is running in kiosk mode, perform the following steps:

&#49;. Add the Citrix Receiver ID to the manifest.json file of the third-party Chrome app. Example:

```json
"kiosk_enabled": true,
   "kiosk_secondary_apps": [
   {
      "id": "<receiver_for_chrome_id>"
    }
]
```
&#50;. Enable the Auto-Launch Kiosk App for the third-party Chrome app in the Google Admin console.For more information, see [https://support.google.com/chrome/a/answer/3316168?hl=en](https://support.google.com/chrome/a/answer/3316168?hl=en)

!!!tip "Note"
		In this scenario, Receiver policies that are set in the Google Admin console will not be pushed to the device.

&#51; Custom configurations for each Citrix Receiver session that you set by way of Citrix Receiver policies in the Google Admin Console are not being pushed to the user device. Use the SDK to set custom configurations for each Citrix Receiver session. 
For more information, see [Namespace: receiver](./namespace-receiver.md)

## Virtual Channel SDK in kiosk mode

The Virtual Channel SDK for Citrix Receiver for Chrome supports custom virtual channels in kiosk mode. After you configure the Virtual Channel SDK to support kiosk mode, you must repackage and publish Citrix Receiver for Chrome.

To enable custom the Virtual Channel SDK for Citrix Receiver in kiosk mode, perform the following steps:

&#49;. Add the Virtual Channel App ID to the manifest.json file of Citrix Receiver in the third-party Chrome app and repackage and publish Citrix Receiver. Example:

```json
"kiosk_enabled": true,
"kiosk_secondary_apps": [
	{"id": "<VC_APP_ID>"}
]
```

&#50;. Add `"kiosk_enabled": true` to the manifest file of the Virtual Channel app.

&#51;. Publish the Virtual Channel app through the developer dashboard and enable kiosk mode. For more information, see [https://support.google.com/chrome/a/answer/3316168?hl=en](https://support.google.com/chrome/a/answer/3316168?hl=en).

## Integrate HDX SDK and Virtual Channel SDK

You can now integrate both the HDX SDK and the VC SDK in a single Chrome app or in separate apps. This feature is supported in both user or public session, and in kiosk mode.

### Single app for both SDKs

You can include the custom Virtual Channel code within the third-party Chrome app integrating the HDX SDK to function as a single app for both SDKs. For sample examples, see [Downloads](https://www.citrix.com/downloads/citrix-receiver/chrome/receiver-for-chrome-sdk-latest.html).

#### User or public session mode

In this mode, a third-party Chrome app can embed Citrix Receiver sessions or launch in a new window

To launch sessions, perform the following steps:

1. Whitelist the third-party Chrome app ID using the Citrix Receiver policy.
2. Add a custom Virtual Channel configuration using the Citrix Receiver policy or the SDK API.

!!!tip "Note"
		The ID of the Virtual Channel app is the third-party Chrome app ID itself. For more information, see [https://docs.citrix.com/en-us/receiver/chrome/current-release/sdk-api.html](https://docs.citrix.com/en-us/receiver/chrome/current-release/sdk-api.html).
#### Kiosk mode

In this mode, a third-party Chrome app can only embed Citrix Receiver sessions.
To launch sessions, perform the following steps:
1.	Add the Citrix Receiver ID to the manifest.json file of the third-party Chrome app.

Example:

```json
"kiosk_enabled": true,
   "kiosk_secondary_apps": [
   {
      "id": "<receiver_for_chrome_id>"
    }
]
```
1.	Publish both Citrix Receiver and the third-party Chrome app through the developer dashboard and enable kiosk mode.
2.	Enable Auto-Launch Kiosk App for the third-party Chrome app in the Google Admin console.
3.	Custom configurations for each Citrix Receiver session that you set by way of Citrix Receiver policies in the Google Admin Console are not being pushed to the user device. Use the SDK to set custom configurations for each Citrix Receiver session.

!!!tip "Note"
		The ID of the Virtual Channel app is the third-party Chrome app ID itself.

### Separate apps for each SDK

You can have individual third-party Chrome apps for the HDX SDK for Chrome and for the Virtual Channel SDK. In this case, the custom Virtual Channel can be made available in both kiosk, and user or public session mode.

#### User or public session mode

In this mode, a third-party Chrome app can embed Citrix Receiver sessions or launch in a new window
To launch sessions, perform the following steps:

1.	Whitelist the third-party Chrome app ID using the Citrix Receiver policy.
2.	Add a custom Virtual Channel configuration using the Citrix Receiver policy or the SDK API. For more information, see [https://docs.citrix.com/en-us/receiver/chrome/current-release/sdk-api.html](https://docs.citrix.com/en-us/receiver/chrome/current-release/sdk-api.html).

#### Kiosk mode

In this mode, only those Citrix Receiver sessions launched in embedded mode using the HDX SDK for Chrome is supported.

To launch sessions, perform the following steps:

&#49;. Add both the Citrix Receiver for Chrome ID and the Virtual Channel App ID to the manifest.json file of the third-party Chrome app. Example:

```json
"kiosk_enabled":true,
"kiosk_secondary_apps": [
	{"id": "<Receiver_for_chrome_id>"} ,
	{ "id": "<vc_app_id>" }
]
```
&#50;. Add `"kiosk_enabled": true` to the manifest file of the Virtual Channel app.

&#51;. Publish the Virtual Channel app and the third-party Chrome app through the developer dashboard and enable kiosk mode. For more information, see [https://support.google.com/chrome/a/answer/3316168?hl=en](https://support.google.com/chrome/a/answer/3316168?hl=en).

&#52;. Enable Auto-Launch Kiosk App for the third-party Chrome app in the Google Admin console.

&#53;. Custom configurations for each Citrix Receiver session that you set by way of Citrix Receiver policies in the **Google Admin Console** are not being pushed to the user device. Use the SDK to set custom configurations for each Citrix Receiver session.

## Additional references

Citrix Receiver for Chrome uses message communication provided by Chrome OS. For more details, see the following links:

1. [https://developer.chrome.com/apps/tags/appview](https://developer.chrome.com/apps/tags/appview)
1. [https://developer.chrome.com/extensions/runtime#event-onMessageExternal](https://developer.chrome.com/extensions/runtime#event-onMessageExternal) 
1. [https://developer.chrome.com/extensions/runtime#method-sendMessage](https://developer.chrome.com/extensions/runtime#method-sendMessage)
1. For more details, see Manage Chrome Apps by organizational unit on Google support.
1. For more information on whitelisting, see [https://support.google.com/chrome/a/answer/6177431?hl=en](https://support.google.com/chrome/a/answer/6177431?hl=en)