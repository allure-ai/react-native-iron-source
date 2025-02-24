
# @allure-ai/react-native-iron-source

[Iron Source SDK](https://developers.ironsrc.com/) React Native bridge. 
Supports all ad units (Rewarded Video, Interstitial, Banner, Offerwall).

Many thanks to all contributors of [squaretwo/react-native-iron-source](https://github.com/squaretwo/react-native-iron-source) and the fork wowmaking/react-native-iron-source](https://github.com/wowmaking/react-native-iron-source). 

The fork includes following improvements:
* Banner implementation
* Bug fixes for Offerwall, Interstitial, Rewarded
* Documentation
* Installation using CocoaPods
* Easier installation on android 
* [Validate integration option](https://developers.ironsrc.com/ironsource-mobile/ios/integration-helper-ios/)
* `IronSource.setConsent` method
* Working Example App
* etc

## Compatibility table

| @allure-ai/react-native-iron-source | Xcode  | IronSourceSDK |
| ----------------------------------- | ------ | ------------- |
| 7.0.0                               | <= 11  | <= 7.0.1      |
| \>= 7.0.3                           | \>= 12 | \>= 7.0.3     |

## Getting started

`npm install @allure-ai/react-native-iron-source --save`

You can find available versions [here](https://github.com/allure-ai/react-native-iron-source/releases).

### RN >= 0.60

#### iOS
Add `SKAdNetworkIdentifier` to your Info.plist
```xml
<key>SKAdNetworkItems</key>
<array>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>SU67R6K2V3.skadnetwork</string>
    </dict>
</array>
```
 
#### Android
 Add a repo to your `android/app/build.gradle` file 
 ```
 allprojects {
     repositories {
         // Existing repos here
         // ...
         
         maven { url 'https://android-sdk.is.com/' }
     }
 }
 ```


<details>
  <summary>
    expand/collapse
    <h3>RN < 0.60</h3>
  </summary>

  #### Mostly automatic installation
  
  `$ react-native link @allure-ai/react-native-iron-source`
  
  #### Manual installation
  
  
  ##### iOS
  
  1. In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`
  2. Go to `node_modules` ➜ `@allure-ai/react-native-iron-source` and add `RNIronSource.xcodeproj`
  3. In XCode, in the project navigator, select your project. Add `libRNIronSource.a` to your project's `Build Phases` ➜ `Link Binary With Libraries`
  4. Run your project (`Cmd+R`)
  
  ##### iOS CocoaPods
  1. Add `pod 'RNIronSource', :path => '../node_modules/@allure-ai/react-native-iron-source'` to your `ios/Podfile`
  2. Run `pod install` while in `ios` directory
  
  #### Allow iOS Static Frameworks
  If you are using Static Frameworks on iOS, you need to manually enable this for the project. To enable Static Framework support, add the following global to the top of your /ios/Podfile file:
  `$RNIronSourceAsStaticFramework = true`

  ##### Android
  
  1. Open up `android/app/src/main/java/[...]/MainApplication.java`
    - Add `import com.reactlibrary.RNIronSourcePackage;` to the imports at the top of the file
    - Add `new RNIronSourcePackage()` to the list returned by the `getPackages()` method
  2. Append the following lines to `android/settings.gradle`:
    	```
    	include ':@allure-ai_react-native-iron-source'
    	project(':@allure-ai_react-native-iron-source').projectDir = new File(rootProject.projectDir, 	'../node_modules/@allure-ai/react-native-iron-source/android')
    	```
  3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
    	```
      implementation project(':@allure-ai_react-native-iron-source')
    	```
  
  ### Manual Setup
  
  #### IronSource iOS SDK
  
  ##### For projects with CocoaPods
  
  Do nothing.
  
  ##### For other projects
  
  1. Download the iOS SDK from [Ironsrc.com](http://developers.ironsrc.com/ironsource-mobile/ios/ios-sdk/)
  2. Unzip and rename the directory to `IronSourceSDK`
  3. Copy the SDK to `~/Documents/IronSourceSDK`
  4. Drag the `IronSource.framework` to your react native target build phases from the `~/Documents/IronSourceSDK` directory
  5. Add `~/Documents/IronSourceSDK` to your target's Framework Search Paths in Build Settings
  
  
  #### Android
  Add a repo to your `app/build.gradle` file 
  ```
  repositories {
      maven { url "https://dl.bintray.com/ironsource-mobile/android-sdk" }
  }
  ```
</details>

## Mediation Setup

**WARNING!** Make sure you following the doc carefully. If you miss something in the mediation setup process for some network it may not work partially or entirely. Follow guide for each mediation. Add your devices and simulators to test device list in ironsource admin panel.

Official doc:
- [Android](https://developers.ironsrc.com/ironsource-mobile/android/mediation-networks-android/#step-1).
- [iOS](https://developers.ironsrc.com/ironsource-mobile/ios/mediation-networks-ios/#step-1).

<details>
 <summary>Optional syntax (not recommended)</summary>
 
__Warning:__ Using this syntax means that you lock down your iOS CocoaPods dependencies to versions that we currently use in our organization. We don't recommend doing this because you might want another version at some point.

```
pod 'RNIronSource', :path => '../node_modules/@allure-ai/react-native-iron-source', :subspecs => [
    'Core', # required
    'AdColony',
    'Admob',
    'Amazon',
    'AppLovin',
    'Chartboost',
    'Facebook',
    'HyprMX',
    'InMobi',
    'Maio',
    'MediaBrix',
    'Tapjoy',
    'UnityAds',
    'Vungle'
]
```
</details>

## Usage

You can use [example app](https://github.com/allure-ai/react-native-iron-source/tree/master/ExampleApp) as a reference.

There is also an older example app for RN <= 0.59.x [in this branch](https://github.com/allure-ai/react-native-iron-source/tree/example-app-59/ExampleApp).

### Initialization

<details>
 <summary>1. (Optional) Obtain user's consent to share user data with ad network publishers.</summary>
 
 ```javascript
 import { IronSource } from '@allure-ai/react-native-iron-source';

 // After user granted consent

 IronSource.setConsent(true);
 ```
</details> 

2. Initialize IronSource SDK

```javascript
import { IronSource } from '@allure-ai/react-native-iron-source';

IronSource.initializeIronSource('8a19a09d', 'userId', {
  validateIntegration: true,
}).then(() => {
  console.warn('Init finished');
});
```

### Interstitial

```javascript
import { IronSourceInterstitials } from '@allure-ai/react-native-iron-source';

IronSourceInterstitials.loadInterstitial();
IronSourceInterstitials.addEventListener('interstitialDidLoad', () => {
  IronSourceInterstitials.showInterstitial();
});
```
### Rewarded Video

```javascript
import { IronSourceRewardedVideo } from '@allure-ai/react-native-iron-source';


IronSourceRewardedVideo.initializeRewardedVideo();
IronSourceRewardedVideo.addEventListener('ironSourceRewardedVideoAdRewarded', res => {
  console.warn('Rewarded!', res)
});

// or use IronSourceRewardedVideo.addEventListener('ironSourceRewardedVideoAvailable') 
// to get video status
IronSourceRewardedVideo.isRewardedVideoAvailable().then((available) => {
  if (available) {
    IronSourceRewardedVideo.showRewardedVideo();
  } else {
    throw new Error('No video available');
  }
})
```
### Banner

```javascript
import { IronSourceBanner } from '@allure-ai/react-native-iron-source';

IronSourceBanner.loadBanner('LARGE');
IronSourceBanner.addEventListener('ironSourceBannerDidLoad', () => {
  console.warn('Iron Source banner loaded');
  IronSourceBanner.showBanner();
});
```
### Offerwall

```javascript
import { IronSourceOfferwall } from '@allure-ai/react-native-iron-source';

IronSourceOfferwall.showOfferwall();
IronSourceOfferwall.addEventListener('ironSourceOfferwallReceivedCredits', res => {
  console.warn('Got credits', res)
});
```


## API (Incomplete)

### IronSource.initializeIronSource(ironSourceAppKey, userId, options)
Initializes IronSource SDK. 

 `validateIntegration` provides an easy way to verify 
that you’ve successfully integrated the ironSource 
SDK and any additional adapters; it also makes sure all
 required dependencies and frameworks were added for 
 the various mediated ad networks. It doesn't validate Amazon adapter in current version.
 See official docs for [Android](https://developers.ironsrc.com/ironsource-mobile/android/integration-helper-android/), 
 [iOS](https://developers.ironsrc.com/ironsource-mobile/ios/integration-helper-ios/).
  There's known issue in ios 12. See known issue section.
#### Parameter(s)
* **ironSourceAppKey:** String. Can be found in Iron Source administrator's interface
* **userId:** String. Any unique user id.
* **options:** Object (optional)
    * **validateIntegration:** Boolean. Default: false
#### Returns Promise

```javascript
IronSource.initializeIronSource('8a19a09d', 'userId', {
  validateIntegration: true
})
  .then(() => {
    console.warn('Init finished');
  });
```

### IronSourceBanner.loadBanner(options)
Loads IronSource banner. Returns a promise that will be resolved when banner loads successfully and rejected when it fails.

#### Parameter(s)
* **size**: String. Supported values: `"BANNER"`, `"LARGE"`, `"RECTANGLE"`, `"SMART"`
* **options:** Object (optional)
    * **position:** String. Supported values: `"top"` or `"bottom"`. Default: `"bottom"`.
    * **scaleToFitWidth:** Boolean. Default: false
#### Returns Promise of
* **response:** Object
    * **width:** Number
    * **height:** Number

```javascript
IronSourceBanner.loadBanner('BANNER', {
  position: 'top',
  scaleToFitWidth: true,
})
  .then(response => {
    console.warn(`width: ${response.width}, height: ${response.height}`);
  })
  .catch(err => {
    console.warn(err.message);
  });
```

### IronSourceBanner.hideBanner()
```javascript
IronSourceBanner.hideBanner();
```

### IronSourceSegment constructor
Creates a segment manager
```javascript
import { IronSourceSegment } from '@allure-ai/react-native-iron-source';

const segment = new IronSourceSegment();
```

### IronSourceSegment.setCustomValue
Sets custom user property
```javascript
segment.setCustomValue('VALUE').forKey('KEY');
```

### IronSourceSegment.setSegmentName
Sets custom segment name

```javascript
segment.setSegmentName('NAME');
```

### IronSourceSegment.activate
Makes currently existing segment active. Once it's active IronSource SDK will use it to serve your ad units based on segments to tailor the user’s ad experience.

```javascript
segment.activate();
```

### IronSourceRewardedVideo.isRewardedVideoAvailable
Check if a reward video is available to be shown.
Return a Promise resolving a boolean.

```javascript
IronSourceRewardedVideo.isRewardedVideoAvailable().then((available) => {
  if (available) {
    // Video available
  } else {
    // Video not available
  }
});
```

## Events

### Banner events
| Name | Description |
| ---- | ----------- |
| ironSourceBannerDidLoad | Called after a banner ad has been successfully loaded |
| ironSourceBannerDidFailToLoadWithError | Called after a banner has attempted to load an ad but failed |
| ironSourceBannerDidDismissScreen | Called after a full screen content has been dismissed |
| ironSourceBannerWillLeaveApplication | Called when a user would be taken out of the application context |
| ironSourceBannerWillPresentScreen | Called when a banner is about to present a full screen content |
| ironSourceDidClickBanner | Called after a banner has been clicked |

### Rewarded video events
| Name | Description |
| ---- | ----------- |
| ironSourceRewardedVideoAvailable | Called after a rewarded video has changed its availability to YES. It means that now you can show the ad |
| ironSourceRewardedVideoUnavailable | Called after a rewarded video has changed its availability to NO |
| ironSourceRewardedVideoClosedByUser | Called after a rewarded video has been dismissed |
| ironSourceRewardedVideoDidStart | Called after a rewarded video has started playing. Note: this event is not available for all supported rewarded video ad networks. Check which events are available per ad network you choose |
| ironSourceRewardedVideoClosedByError | Called after a rewarded video has attempted to show but failed |
| ironSourceRewardedVideoDidOpen | Called after a rewarded video has been opened |
| ironSourceRewardedVideoAdStarted | Called after a rewarded video has been opened |
| ironSourceRewardedVideoAdEnded | Called after a rewarded video has finished playing. Note: this event is not available for all supported rewarded video ad networks. Check which events are available per ad network you choose |
| ironSourceRewardedVideoAdRewarded | Invoked when the user completed the video and should be rewarded |

### Interstitial events
| Name | Description |
| ---- | ----------- |
| interstitialDidLoad | Invoked when Interstitial Ad is ready to be shown after load function was called |
| interstitialDidShow | Called each time the Interstitial window has opened successfully |
| interstitialDidFailToShowWithError | Called if showing the Interstitial for the user has failed |
| didClickInterstitial | Called each time the end user has clicked on the Interstitial ad |
| interstitialDidClose | Called each time the Interstitial window is about to close |
| interstitialDidOpen | Called each time the Interstitial window is about to open |
| interstitialDidFailToLoadWithError | Invoked when there is no Interstitial Ad available after calling load function |


### Offerwall events
| Name | Description |
| ---- | ----------- |
| ironSourceOfferwallAvailable | Invoked when there is a change in the Offerwall availability status to YES |
| ironSourceOfferwallUnavailable | Invoked when there is a change in the Offerwall availability status to NO |
| ironSourceOfferwallDidShow | Called each time the Offerwall successfully loads for the user |
| ironSourceOfferwallClosedByUser | Called when the user closes the Offerwall |
| ironSourceOfferwallClosedByError | Called each time the Offerwall fails to show |
| ironSourceOfferwallReceivedCredits | Called each time the user completes an offer |
| ironSourceOfferwallFailedToReceiveCreditsByError | Called when failed to retrieve the users credit balance info |

You can find out more about events in the official doc. Start [here](https://developers.ironsrc.com/ironsource-mobile/ios/rewarded-video-integration-ios/) if you wish.

## Known issues

Ads may stop loading properly when "Reload" option (or CMD+R) in your React Native app was used. You have to restart the app completely if you want to check that ads load and display correctly.

---


