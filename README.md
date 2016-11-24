# Ampiri Unity Plugin Integration Guide

Table of contents
=================

* [Ampiri Support](#ampiri-support)
* [Getting Started](#getting-started)
* [Integrating Banners](#integrating-banners)
* [Integrating Interstitials](#integrating-interstitials)
* [Integrating Videos](#integrating-video)
* [Integrating Third Party SDKs](#integrating-third-party-sdks)
* [Platform Specific Steps](#platform-specific-steps)

Ampiri Support
=================

Additional documentation for integrating the Ampiri SDK with your iOS/ Android app can be found by clicking the links. 

- [Ampiri.com Tutorials](http://www.ampiri.com/tutorials/) - Tutorials and Sign Up to Ampiri
- [Publisher's Self-Serve UI User Guide](https://ampiri.zendesk.com/hc/en-us/articles/213857245-Publisher-s-Self-Serve-UI-User-Guide) - Publisher's Interface
- Additional Documentation Coming Soon!

Getting Started
=================
1. Open your project in the Unity editor and select __Assets > Import Package > Custom Package__ and find the downloaded  _AmpiriPlugin.unitypackage_ file
2. Make sure that all of the files are selected and click __Import__
3. Under __Assets > Ampiri > Prefabs__, drag Ampiri.prefabs into your scene.
4. Android/iOS Ad Unit IDs must be entered in the fields that appear. Ad Unit IDs are available from the Ampiri Admin page

To see the sample, please import AmpiriUnityDemo.package. There you should see the __DemoApp__ scene in the scenes folder and inside the __Scripts__ folder are some example of Ampiri usage scripts

Initialization
===================
You should only call this **once** during the app lifecycle:
```
ampiriSDK.sdk.Init("https://api.ampiri.com", true);
```

Integrating Banners
===================
Banners ads can only be shown on the top or the bottom of the app's screen.  
To show banner:

Available banner sizes:
* 320x50 (recommended)
* 728x90

Available gravity(alignment):
* Center
* Left
* Right

Auto Update advertisement:
* True
* False

```
ampiriSDK.sdk.ShowTopBanner(size, gravity, true);
ampiriSDK.sdk.ShowBottomBanner(size, gravity, true);
```

To hide banner(s):

```
ampiriSDK.sdk.HideTopBanner();
ampiriSDK.sdk.HideBottomBanner();
```

# Banner Callback
```
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

/// <summary>
/// User Example Class on Banners
/// </summary>
public class BannerCallBack : MonoBehaviour {

    [SerializeField]
    Text log;

    void Log(string Text)
    {
        log.text = Text;
    }

    #region Mono
    void OnEnable()
    {
        // Add Standard Banner CallBacks
        AmpiriEvents.Banner_OnLoaded += OnBannerLoaded;
        AmpiriEvents.Banner_OnLoadError += OnBannerLoadError;
        AmpiriEvents.Banner_OnClick += OnBannerClicked;
        AmpiriEvents.Banner_OnNoAd += OnBannerNoAd;
    }
    void OnDisable()
    {
        // Remove Standard Banner CallBacks
        AmpiriEvents.Banner_OnLoaded -= OnBannerLoaded;
        AmpiriEvents.Banner_OnLoadError -= OnBannerLoadError;
        AmpiriEvents.Banner_OnClick -= OnBannerClicked;
        AmpiriEvents.Banner_OnNoAd -= OnBannerNoAd;
    }
    #endregion

    #region Banner CallBacks

	// Callback triggers when banner has successfully loaded
    public void OnBannerLoaded()
    {
        Log("Banner_OnLoaded");
    }
	// Callback triggers when banner has been clicked
    void OnBannerClicked()
    {
        Log("Fullscreen_OnBannerClick");
    }
    // Callback triggers when loading of banner returns error
    void OnBannerLoadError()
    {
        Log("Banner_OnLoadError");
    }
    // Callback triggers when loading of banner returns no advertisements
    void OnBannerNoAd()
    {
        Log("Banner_OnNoAd");
    }
    #endregion
}
```

Integrating Interstitials
==========================
Banner size is defined automatically, depending on the screen size.

To *load* an interstitial banner:
```
ampiriSDK.sdk.LoadFullscreen();
```
To *show* interstitial banner:
```
ampiriSDK.sdk.ShowFullscreen();
```
To *load* and *show* interstitial banners:
```
ampiriSDK.sdk.LoadAndShowFullscreen();
```
To *load* and *show* interstitial banners with delay:
```
ampiriSDK.sdk.LoadAndShowWithDelayFullscreen();
```
# Interstitial Callback Example
```
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

/// <summary>
/// Interstitial callbacks example
/// </summary>
public class InterstitialCallbacks : MonoBehaviour {

    [SerializeField]
    Text statusText;

    [SerializeField]
    FunctionalTestingExample fntest;

    [SerializeField]
    Button showButton;

    [SerializeField]
    Button loadnshowButton;

    [SerializeField]
    Button loadnshowDelayButton;

    void Log(string text)
    {
        statusText.text = "Current status: " + text;
    }

    void OnEnable()
    {
        AmpiriEvents.Interstitial_OnBannerLoaded += OnFullScreenBannerLoaded;
        AmpiriEvents.Interstitial_OnBannerLoadError += OnFullScreenBannerLoadError;
        AmpiriEvents.Interstitial_OnBannerClick += OnFullScreenBannerClicked;
        AmpiriEvents.Interstitial_OnBannerClose += OnFullScreenBannerClose;
        AmpiriEvents.Interstitial_OnBannerNoAd += OnFullScreenBannerNoAd;
    }

    void OnDisable()
    {

        AmpiriEvents.Interstitial_OnBannerLoaded -= OnFullScreenBannerLoaded;
        AmpiriEvents.Interstitial_OnBannerLoadError -= OnFullScreenBannerLoadError;
        AmpiriEvents.Interstitial_OnBannerClick -= OnFullScreenBannerClicked;
        AmpiriEvents.Interstitial_OnBannerClose -= OnFullScreenBannerClose;
        AmpiriEvents.Interstitial_OnBannerNoAd -= OnFullScreenBannerNoAd;

    }

	// Callback triggers when Interstitial has successfully loaded
    void OnFullScreenBannerLoaded()
    {
        Log("FullScreen_OnBannerLoaded");
        showButton.interactable = true;
        loadnshowButton.interactable = loadnshowDelayButton.interactable = false;
    }
	// Callback triggers when Interstitial has closed
    void OnFullScreenBannerClose()
    {
        Log("Fullscreen_OnBannerClose");
        fntest.ExitTransition();
        showButton.interactable = false;
        loadnshowButton.interactable = loadnshowDelayButton.interactable = true;
    }
	// Callback triggers when Interstitial has been clicked
    void OnFullScreenBannerClicked()
    {
        Log("Fullscreen_OnBannerClick");
        fntest.ExitTransition();
        showButton.interactable = false;
        loadnshowButton.interactable = loadnshowDelayButton.interactable = true;
    }
	// Callback triggers when Interstitial returns and error while loading
    void OnFullScreenBannerLoadError()
    {
        Log("Fullscreen_OnBannerLoadError");
        fntest.ExitTransition();
        showButton.interactable = false;
        loadnshowButton.interactable = loadnshowDelayButton.interactable = true;
    }
	// Callback triggers when Interstitial has no advertisments
    void OnFullScreenBannerNoAd()
    {
        Log("Fullscreen_OnBannerNoAd");
        fntest.ExitTransition();
        showButton.interactable = false;
        loadnshowButton.interactable = loadnshowDelayButton.interactable = true;
    }

    void Awake()
    {
        showButton.interactable = false;
    }

}

```

Integrating Video
===================
To load video:
```
ampiriSDK.sdk.LoadVideo();
```
Once a video has been successfully loaded, you can use the callback AmpiriEvents.video_OnBannerLoaded
to show video:

```
if (ampiriSDK.sdk.VideoIsReady())
{
    Log("Video_OnBannerLoaded ready YES");
    ampiriSDK.sdk.ShowVideo();
}
else

    Log("Video_OnBannerLoaded ready NO");
}
```

# Video Callback Example
```
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

/// <summary>
/// Class handles or video callbacks
/// </summary>
public class VideoCallBack : MonoBehaviour {

    [SerializeField]
    AdsSdk ampiriSDK;

    [SerializeField]
    FunctionalTestingExample fntest; 

    [SerializeField]
    Text text;

    void OnEnable()
    {
        AmpiriEvents.video_OnBannerLoaded += OnVideoBannerLoaded;
        AmpiriEvents.video_OnBannerOpen += OnVideoBannerOpen;
        AmpiriEvents.video_OnVideoStarted += OnVideoStarted;
        AmpiriEvents.video_OnBannerLoadError += OnVideoBannerLoadError;
        AmpiriEvents.video_OnBannerClose += OnVideoBannerClose;
        AmpiriEvents.video_OnVideoCompleted += OnVideoCompleted;
        AmpiriEvents.video_OnBannerNoAd += OnVideoNoAd;
    }

    void OnDisable()
    {
        AmpiriEvents.video_OnBannerLoaded -= OnVideoBannerLoaded;
        AmpiriEvents.video_OnBannerOpen -= OnVideoBannerOpen;
        AmpiriEvents.video_OnVideoStarted -= OnVideoStarted;
        AmpiriEvents.video_OnBannerLoadError -= OnVideoBannerLoadError;
        AmpiriEvents.video_OnBannerClose -= OnVideoBannerClose;
        AmpiriEvents.video_OnVideoCompleted -= OnVideoCompleted;
        AmpiriEvents.video_OnBannerNoAd -= OnVideoNoAd;
    }


    void Log(string Text)
    {
        if(text != null)
            text.text = Text;
    }

    // Callback triggers when video has loaded
    void OnVideoBannerLoaded()
    {
        if (ampiriSDK.sdk.VideoIsReady())
        {
            Log("Video_OnBannerLoaded ready YES");
            ampiriSDK.sdk.ShowVideo();
        }
        else
       
            Log("Video_OnBannerLoaded ready NO");
        }
    }
	// Callback triggers when video has opened
    void OnVideoBannerOpen()
    {
        Log("Video_OnBannerOpen");
    }
    // Callback triggers when video closes
    void OnVideoBannerClose()
    {
        Log("Video_OnBannerClose");
    }
	// // Callback triggers when loading of video has returned an error
    void OnVideoBannerLoadError()
    {
        Log("Video_OnBannerLoadError");

        fntest.ExitTransition();
    }
	// // Callback triggers when video has completed
    void OnVideoCompleted()
    {
        Log("Video_OnVideoCompleted");
        fntest.ExitTransition();
    }
	// Callback triggers when video has started
    void OnVideoStarted()
    {
        Log("Video_OnVideoStarted");
    }

	// Callback triggers when video has no advertisments
    void OnVideoNoAd()
    {
        Log("Video_OnVideoNoAd");
    }

}
```
Integrating Third Party SDKs
=============================
#### Android
For integrating third party SDKs with Android, please follow the steps in [Platform Specific Steps](#platform-specific-steps) below.

#### iOS
1. Please download Unity packages from the third-party folder.
2. After the download is complete, double click &lt;network&gt;.unitypackage and import all files.

**Note: You cannot use Baidu and MoPub both in one app as this results in a compile error**


Platform-Specific Steps
=======================

#### Android
* To run your Unity App follow the instructions in the section **Building and Running Android Ampiri SDK**. In point 2 drag the scenes of your app to the __Scenes In Build__ dialog.

#### iOS

1. Go to __File > Build Settings__, drag and drop the scenes you want to have included in the build to the __Scenes in Build__.  
2. In the  __Platform__ section select  __iOS__. Configure the build to the right of the __Platform__ section, and select __Build__ in the menu.  
> **Note: You may be asked to download and install the __iOSSupport Installer__.**
3. Select a folder where you want to build a project, click __Save__.
4. Open the exported project in Xcode.

## Building and running Android Ampiri SDK

To run the Ampiri SDK in the Android version of your Unity-application, follow these steps:

1. Click __File > Build Settings__.
2. Select __Android__ in the __Platforms__ list and select the __Google Android Project__ checkbox. Drag all the required scenes from your app to __Scenes In Build__. 
3. Click on __Player Settings__ and configure the __Bundle Identifier__.
4. Click the __Export__ button and select a location where you want to export the project.
5. Download and install [Android Studio](https://developer.android.com/studio/index.html) and configure an Android development environment if you have not done so already.
6. In Android Studio click __File > New > Import Project__. Choose the location of the exported project you specified in step 4.
7. Configure the project according to the [Ampiri SDK Instructions](https://github.com/ampiri/ampiri-android-sdk).

## Building and running iOS Ampiri SDK 

To run the Ampiri SDK in the iOS version of your Unity-application, follow these steps:

1. Select __File > Build Settings__.
2. Select __iOS__ in the __Platforms__ list. Drag all the required scenes to __Scenes In Build__. 
3. Select on __Player Settings__ and configure __Bundle Identifier__.
4. Look for __Target minimum iOS Version__ and select __8.0__.
5. Select the __Export__ button and choose the location where you want to export the project.
6. Open the exported project in Xcode.
7. Select the project and under the __General Tab__, configure __signing__.
8. Scroll down and look for __Embeddded Binaries__, select the __"+"__ button and select __Plugins > iOS > AMPVastLib.Framework__.
9. On the top Xcode menu, select __Product > Build__.

