<p align="center" >
  <img src="https://raw.githubusercontent.com/wiki/blinkid/blinkid-ios/Images/logo-microblink.png" alt="MicroBlink" title="MicroBlink">
</p>

# PhotoPay SDK for payment slips scanning

PhotoPay SDK is a delightful component for quick and easy scanning of payment slips and payment barcodes. The SDK is powered with [MicroBlink's](http://www.microblink.com) industry-proven and world leading OCR and barcode scanning technology, and offers:

- integrated camera management
- layered API, allowing everything from simple integration to complex UX customizations.
- lightweight and no internet connection required
- enteprise-level security standards
- data parsing from payment slips and barcode standards

PhotoPay is a part of family of SDKs developed by [MicroBlink](http://www.microblink.com) for optical text recognition, barcode scanning, ID document scanning, payment slips and barcodes and many others.

## SDK download

To download the SDK, please contact us at sales@microblink.com.

# Table of contents

* [Requirements](#requirements)
* [Quick Start](#quickStart)
* [Advanced BlinkInput integration instructions](#advancedIntegration)
    * [UI customizations of built-in `MBOverlayViewControllers` and `MBOverlaySubviews`](#uiCustomizations)
        * [Built-in overlay view controllers and overlay subviews](#builtInUIComponents)
    * [Using `MBBarcodeOverlayViewController`](#mbBarcodeOverlayViewcontroller)
    * [Using `MBDocumentOverlayViewController`](#mbDocumentOverlayViewcontroller)
    * [Using `MBDocumentVerificationOverlayViewController`](#mbDocumentVerificationOverlayViewcontroller)
    * [Using `MBPhotopayOverlayViewController`](#mbPhotopayOverlayViewController)
    * [Using `MBFieldOfViewOverlayViewController`](#mbFieldOfViewOverlayViewController)
    * [Custom overlay view controller](#recognizerRunnerViewController)
    * [Direct processing API](#directAPI)
        * [Using Direct API for `NSString` recognition (parsing)](#directAPI_strings)
* [`MBRecognizer` and available recognizers](#availableRecognizers)
    * [The `MBRecognizer` concept](#recognizerConcept)
    * [`MBRecognizerCollection` concept](#recognizerBCollection)
    * [List of available recognizers](#recognizerList)
        * [Frame Grabber Recognizer](#frameGrabberRecognizer)
        * [Success Frame Grabber Recognizer](#successFrameGrabberRecognizer)
        * [PDF417 recognizer](#pdf417Recognizer)
        * [Barcode recognizer](#barcodeRecognizer)
        * [BlinkInput recognizer](#blinkInputRecognizer)
        * [Detector recognizer](#detectorRecognizer)
        * [BlinkID recognizers by countries](#blinkIdRecognizersByCountry)
        * [PhotoPay recognizers by countries](#photopayRecognizersByCountry)
    * [`Field by field` feature](#fieldByFieldFeature)
* [`MBProcessor` and `MBParser`](#processorsAndParsers)
    * [The `MBProcessor` concept](#processorConcept)
    * [List of available processors](#processorList)
        * [Image Return Processor](#imageReturnProcessor)
        * [Parser Group Processor](#parserGroupProcessor)
    * [The `MBParser` concept](#parserConcept)
    * [List of available parsers](#parserList)
        * [Amount Parser](#amountParser)
        * [Date Parser](#dateParser)
        * [Email Parser](#emailParser)
        * [IBAN Parser](#ibanParser)
        * [License Plates Parser](#licensePlatesParser)
        * [Raw Parser](#rawParser)
        * [Regex Parser](#regexParser)
        * [TopUp Parser](#topUpParser)
        * [VIN (*Vehicle Identification Number*) Parser](#vinParser)
        * [List of available parsers by country](#parserListByCountry)
* [Scanning generic documents with Templating API](#detectorTemplating)
        * [The `MBProcessorGroup` component](#processorGroup)
        * [List of available dewarp policies](#dewarpPolicyList)
        * [The `MBTemplatingClass` component](#templatingClass)
        * [Implementing the `MBTemplatingClassifier`](#implementingTemplatingClassifier)
* [The `MBDetector` concept](#detectorConcept)
    * [List of available detectors](#detectorList)
        * [Document Detector](#documentDetector)
        * [MRTD Detector](#mrtdDetector)
* [Troubleshooting](#troubleshoot)
    * [Integration problems](#integrationTroubleshoot)
    * [SDK problems](#sdkTroubleshoot)
    * [Frequently asked questions and known problems](#faq)
* [Additional info](#info)

# <a name="requirements"></a> Requirements

SDK package contains Microblink framework and one or more sample apps which demonstrate framework integration. The framework can be deployed in iOS 8.0 or later, iPhone 4S or newer and iPad 2 or newer.

SDK performs significantly better when the images obtained from the camera are focused. Because of that, the SDK can have lower performance on iPad 2 and iPod Touch 4th gen devices, which [don't have camera with autofocus](http://www.adweek.com/socialtimes/ipad-2-rear-camera-has-tap-for-auto-exposure-not-auto-focus/12536). 



# <a name="quickStart"></a> Quick Start

## Getting started with BlinkID SDK

This Quick Start guide will get you up and performing OCR scanning as quickly as possible. All steps described in this guide are required for the integration.

This guide sets up basic PhotoPay slips and barcode scanning. It closely follows the PhotoPay-sample app. We highly recommend you try to run the sample app. The sample app should compile and run on your device, and in the iOS Simulator. 

The source code of the sample app can be used as the reference during the integration.

### 1. Initial integration steps

- Get access to PhotoPay SDK by contacting our team on [microblink.com](https://microblink.com/). They will give you information on how to download the SDK to your filesystem. Perform the download.
        
- Copy MicroBlink.framework and MicroBlink.bundle to your project folder.

- In your Xcode project, open the Project navigator. Drag the MicroBlink.framework and MicroBlink.bundle to your project, ideally in the Frameworks group, together with other frameworks you're using. When asked, choose "Create groups", instead of the "Create folder references" option.

![Adding MicroBlink.framework to your project](https://raw.githubusercontent.com/wiki/photopay/photopay-ios/Images/01%20-%20Add%20Framework.jpg)

- Since Microblink.framework is a dynamic framework, you also need to add it to embedded binaries section in General settings of your target.

![Adding MicroBlink.framework to embedded binaries](https://raw.githubusercontent.com/wiki/photopay/photopay-ios/Images/03%20-%20Embed%20Binaries.png)

- Include the additional frameworks and libraries into your project in the "Linked frameworks and libraries" section of your target settings. 
    
    - libc++.tbd
    - libz.tbd
    - libiconv.tbd
    - AVFoundation.framework
    - AudioToolbox.framework
    - CoreMedia.framework
    - AssetsLibrary.framework
    - Accelerate.framework

![Adding Apple frameworks to your project](https://raw.githubusercontent.com/wiki/blinkid/blinkid-ios/Images/02%20-%20Add%20Libraries.jpg)
    
### 2. Referencing header file
    
In files in which you want to use scanning functionality place import directive.

Swift

```swift
import MicroBlink
```

Objective-C

```objective-c
#import <MicroBlink/MicroBlink.h>
```
    
### 3. Initiating the scanning process
    
To initiate the scanning process, first decide where in your app you want to add scanning functionality. Usually, users of the scanning library have a button which, when tapped, starts the scanning process. Initialization code is then placed in touch handler for that button. Here we're listing the initialization code as it looks in a touch handler method.

Also, for initialization purposes, the ViewController which initiates the scan have private prperties for [`MBRawParser`](http://blinkid.github.io/blinkid-ios/Classes/MBRawParser.html), [`MBParserGroupProcessor`](http://blinkid.github.io/blinkid-ios/Classes/MBParserGroupProcessor.html) and [`MBBlinkInputRecognizer`](http://blinkid.github.io/blinkid-ios/Classes/MBBlinkInputRecognizer.html), so we know how to obtain result.

```swift
class ViewController: UIViewController, MBPhotopayOverlayViewControllerDelegate  {
    
    var austriaSlipRecognizer : MBAustriaSlipRecognizer?

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    @IBAction func didTapScan(_ sender: AnyObject) {
        
        // To specify we want to perform Austrian Slip recognition, initialize the MBAustriaSlipRecognizer settings
        self.austriaSlipRecognizer = MBAustriaSlipRecognizer()
        
        /** Create barcode settings */
        let settings : MBPhotopayOverlaySettings = MBPhotopayOverlaySettings()
        
        /** Crate recognizer collection */
        let recognizerList = [self.austriaSlipRecognizer!]
        let recognizerCollection : MBRecognizerCollection = MBRecognizerCollection(recognizers: recognizerList)
        
        /** Create your overlay view controller */
        let photopayOverlayViewController : MBPhotopayOverlayViewController = MBPhotopayOverlayViewController(settings: settings, recognizerCollection: recognizerCollection, delegate: self)
        
        /** Create recognizer view controller with wanted overlay view controller */
        let recognizerRunneViewController : UIViewController = MBViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: photopayOverlayViewController)
        
        /** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
        self.present(recognizerRunneViewController, animated: true, completion: nil)
    }
}
```


```objective-c
@interface ViewController () <MBPhotopayOverlayViewControllerDelegate>

@property (nonatomic, strong) MBAustriaSlipRecognizer *austriaSlipRecognizer;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
}


- (IBAction)didTapScan:(id)sender {
    
    MBPhotopayOverlaySettings* settings = [[MBPhotopayOverlaySettings alloc] init];

    self.austriaSlipRecognizer = [[MBAustriaSlipRecognizer alloc] init];

    /** Create recognizer collection */
    MBRecognizerCollection *recognizerCollection = [[MBRecognizerCollection alloc] initWithRecognizers:@[self.austriaSlipRecognizer]];
    
    MBPhotopayOverlayViewController *overlayVC = [[MBPhotopayOverlayViewController alloc] initWithSettings:settings recognizerCollection:recognizerCollection delegate:self];
    UIViewController<MBRecognizerRunnerViewController>* recognizerRunnerViewController = [MBViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];
    
    /** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
    [self presentViewController:recognizerRunnerViewController animated:YES completion:nil];

}

@end
```
    
### 4. Registering for scanning events
    
In the previous step, you instantiated [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html) object with a delegate object. This object gets notified on certain events in scanning lifecycle. In this example we set it to `self`. The protocol which the delegate has to implement is [`MBPhotopayOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPhotopayOverlayViewControllerDelegate.html) protocol. It is necessary to conform to that protocol. We will discuss more about protocols in [Advanced integration section](#advancedIntegration). You can use the following default implementation of the protocol to get you started.

```swift
func photopayOverlayViewControllerDidFinishScanning(_ photopayOverlayViewController: MBPhotopayOverlayViewController, state: MBRecognizerResultState) {

    // this is done on background thread
    // check for valid state
    if state == MBRecognizerResultState.valid {

        // first, pause scanning until we process all the results
        photopayOverlayViewController.recognizerRunnerViewController?.pauseScanning()

        DispatchQueue.main.async(execute: {() -> Void in
            // All UI interaction needs to be done on main thread
        })
    }
}

func photopayOverlayViewControllerDidTapClose(_ photopayOverlayViewController: MBPhotopayOverlayViewController) {
    // Your action on cancel 
}
```
    
```objective-c  
- (void)photopayOverlayViewControllerDidFinishScanning:(nonnull MBPhotopayOverlayViewController *)photopayOverlayViewController state:(MBRecognizerResultState)state {
    
    // this is done on background thread
    // check for valid state
    if (state == MBRecognizerResultStateValid) {
        
        // first, pause scanning until we process all the results
        [photopayOverlayViewController.recognizerRunnerViewController pauseScanning];
        
        dispatch_async(dispatch_get_main_queue(), ^{
            // All UI interaction needs to be done on main thread
        });
    }
}

- (void)photopayOverlayViewControllerDidTapClose:(nonnull MBPhotopayOverlayViewController *)photopayOverlayViewController {
    // Your action on cancel 
}
```


# <a name="advancedIntegration"></a> Advanced BlinkInput integration instructions
This section covers more advanced details of BlinkInput integration.

1. [First part](#uiCustomizations) will cover the possible customizations when using UI provided by the SDK.
2. [Second part](#recognizerRunnerViewController) will describe how to embed [`MBRecognizerRunnerViewController's delegates`](http://photopay.github.io/photopay-ios/Protocols.html) into your `UIViewController` with the goal of creating a custom UI for scanning, while still using camera management capabilites of the SDK.
3. [Third part](#directAPI) will describe how to use the [`MBRecognizerRunner`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerRunner.html) (Direct API) for recognition directly from `UIImage` without the need of camera or to recognize camera frames that are obtained by custom camera management.
4. [Fourth part](#availableRecognizers) will describe recognizer concept and available recognizers.

## <a name="uiCustomizations"></a> UI customizations of built-in `MBOverlayViewControllers` and `MBOverlaySubviews`

### <a name="builtInUIComponents"></a> Built-in overlay view controllers and overlay subviews

Within BlinkID SDK there are several built-in overlay view controllers and scanning subview overlays that you can use to perform scanning.


#### `MBBarcodeOverlayViewController`

[`MBBarcodeOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBBarcodeOverlayViewController.html) is overlay view controller best suited for performing scanning of various barcodes. It has [`MBBarcodeOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBBarcodeOverlayViewControllerDelegate.html) delegate which can be used out of the box to perform scanning using the default UI.

#### `MBDocumentOverlayViewController`

[`MBBarcodeOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html) is overlay view controller best suited for performing scanning of various document cards. It has [`MBDocumentOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBDocumentOverlayViewControllerDelegate.html) delegate which can be used out of the box to perform scanning using the default UI.

#### `MBDocumentVerificationOverlayViewController`

[`MBBarcodeOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html) is overlay view controller best suited for performing scanning of various document for both front and back side. It has [`MBDocumentVerificationOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBDocumentVerificationOverlayViewControllerDelegate.html) delegate which can be used out of the box to perform scanning using the default UI.

#### `MBPhotopayOverlayViewController`

[`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html) is overlay view controller best suited for performing scanning of various payment slips and barcodes. It has [`MBPhotopayOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPhotopayOverlayViewControllerDelegate.html) delegate which can be used out of the box to perform scanning using the default UI.

#### `MBFieldOfViewOverlayViewController`

[`MBFieldOfViewOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBFieldOfViewOverlayViewController.html) is overlay view controller best suited for performing scanning of various payment slips and barcodes with field of view. It has [`MBFieldOfViewOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBFieldOfViewOverlayViewControllerDelegate.html) delegate which can be used out of the box to perform scanning using the default UI.


## <a name="mbBarcodeOverlayViewcontroller"></a> Using `MBBarcodeOverlayViewController`

[`MBBarcodeOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBBarcodeOverlayViewController.html) is built-in overlay view controller which is best suited to use while scanning various barcodes. Here is an example how to use and initialize [`MBBarcodeOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBBarcodeOverlayViewController.html):

Swift
```swift
/** Create your overlay view controller */
let barcodeOverlayViewController : MBBarcodeOverlayViewController = MBBarcodeOverlayViewController(settings: barcodeSettings, recognizerCollection: recognizerCollection, delegate: self)

/** Create recognizer view controller with wanted overlay view controller */
let recognizerRunneViewController : UIViewController = MBViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: barcodeOverlayViewController)

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
self.present(recognizerRunneViewController, animated: true, completion: nil)
```

Objective-C
```objective-c
MBBarcodeOverlayViewController *overlayVC = [[MBBarcodeOverlayViewController alloc] initWithSettings:settings recognizerCollection: recognizerCollection delegate:self];
UIViewController<MBRecognizerRunnerViewController>* recognizerRunnerViewController = [MBViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
[self presentViewController:recognizerRunnerViewController animated:YES completion:nil];
```

As you can see, when initializing [`MBBarcodeOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBBarcodeOverlayViewController.html), we are sending delegate property as `self`. To get results, we need to conform to [`MBBarcodeOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBBarcodeOverlayViewControllerDelegate.html) protocol.


## <a name="mbDocumentOverlayViewcontroller"></a> Using `MBDocumentOverlayViewController`

[`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html) is built-in overlay view controller which is best suited to use while scanning one side of a document. As you have seen in [Quick Start](#quickStart), [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html) has [`MBDocumentOverlaySettings`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlaySettings.html). Here is an example how to use and initialize [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html):

Swift
```swift
/** Create your overlay view controller */
let documentOverlayViewController : MBDocumentOverlayViewController = MBDocumentOverlayViewController(settings: documentSettings, recognizerCollection: recognizerCollection, delegate: self)

/** Create recognizer view controller with wanted overlay view controller */
let recognizerRunneViewController : UIViewController = MBViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: documentOverlayViewController)

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
self.present(recognizerRunneViewController, animated: true, completion: nil)
```

Objective-C
```objective-c
MBDocumentOverlayViewController *overlayVC = [[MBDocumentOverlayViewController alloc] initWithSettings:settings recognizerCollection: recognizerCollection delegate:self];
UIViewController<MBRecognizerRunnerViewController>* recognizerRunnerViewController = [MBViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
[self presentViewController:recognizerRunnerViewController animated:YES completion:nil];
```

As you can see, when initializing [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), we are sending delegate property as `self`. To get results, we need to conform to [`MBDocumentOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBDocumentOverlayViewControllerDelegate.html) protocol.


## <a name="mbDocumentVerificationOverlayViewcontroller"></a> Using `MBDocumentVerificationOverlayViewController`

[`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html) is built-in overlay view controller which is best suited to use while scanning both sides of a document. Here is an example how to use and initialize [`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html):

Swift
```swift
/** Create your overlay view controller */
let documentOverlayViewController : MBDocumentVerificationOverlayViewController = MBDocumentVerificationOverlayViewController(settings: documentVerificationSettings, recognizerCollection: recognizerCollection, delegate: self)

/** Create recognizer view controller with wanted overlay view controller */
let recognizerRunneViewController : UIViewController = MBViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: documentOverlayViewController)

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
self.present(recognizerRunneViewController, animated: true, completion: nil)
```

Objective-C
```objective-c
MBDocumentVerificationOverlayViewController *overlayVC = [[MBDocumentVerificationOverlayViewController alloc] initWithSettings:settings recognizerCollection: recognizerCollection delegate:self];
UIViewController<MBRecognizerRunnerViewController>* recognizerRunnerViewController = [MBViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
[self presentViewController:recognizerRunnerViewController animated:YES completion:nil];
```

As you can see, when initializing [`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html), we are sending delegate property as `self`. To get results, we need to conform to [`MBDocumentVerificationOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBDocumentVerificationOverlayViewControllerDelegate.html) protocol.


## <a name="mbPhotopayOverlayViewController"></a> Using `MBPhotopayOverlayViewController`

[`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html) is built-in overlay view controller which is best suited to use while scanning both sides of a document. Here is an example how to use and initialize [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html):

Swift
```swift
/** Create your overlay view controller */
let photopayOverlayViewController : MBPhotopayOverlayViewController = MBPhotopayOverlayViewController(settings: photopayOverlaySttings, recognizerCollection: recognizerCollection, delegate: self)

/** Create recognizer view controller with wanted overlay view controller */
let recognizerRunneViewController : UIViewController = MBViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: photopayOverlayViewController)

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
self.present(recognizerRunneViewController, animated: true, completion: nil)
```

Objective-C
```objective-c
MBPhotopayOverlayViewController *overlayVC = [[MBPhotopayOverlayViewController alloc] initWithSettings:settings recognizerCollection: recognizerCollection delegate:self];
UIViewController<MBRecognizerRunnerViewController>* recognizerRunnerViewController = [MBViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
[self presentViewController:recognizerRunnerViewController animated:YES completion:nil];
```

As you can see, when initializing [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), we are sending delegate property as `self`. To get results, we need to conform to [`MBPhotopayOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPhotopayOverlayViewControllerDelegate.html) protocol.


## <a name="mbFieldOfViewOverlayViewController"></a> Using `MBFieldOfViewOverlayViewController`

[`MBFieldOfViewOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBFieldOfViewOverlayViewController.html) is built-in overlay view controller which is best suited to use while scanning both sides of a document. Here is an example how to use and initialize [`MBFieldOfViewOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBFieldOfViewOverlayViewController.html):

Swift
```swift
/** Create your overlay view controller */
let photopayOverlayViewController : MBFieldOfViewOverlayViewController = MBFieldOfViewOverlayViewController(settings: photopayOverlaySttings, recognizerCollection: recognizerCollection, delegate: self)

/** Create recognizer view controller with wanted overlay view controller */
let recognizerRunneViewController : UIViewController = MBViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: photopayOverlayViewController)

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
self.present(recognizerRunneViewController, animated: true, completion: nil)
```

Objective-C
```objective-c
MBFieldOfViewOverlayViewController *overlayVC = [[MBFieldOfViewOverlayViewController alloc] initWithSettings:settings recognizerCollection: recognizerCollection delegate:self];
UIViewController<MBRecognizerRunnerViewController>* recognizerRunnerViewController = [MBViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
[self presentViewController:recognizerRunnerViewController animated:YES completion:nil];
```

As you can see, when initializing [`MBFieldOfViewOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBFieldOfViewOverlayViewController.html), we are sending delegate property as `self`. To get results, we need to conform to [`MBFieldOfViewOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBFieldOfViewOverlayViewControllerDelegate.html) protocol.


## <a name="recognizerRunnerViewController"></a> Custom overlay view controller

Please check our pdf417-sample-Swift for custom implementation of overlay view controller.

Overlay View Controller is an abstract class for all overlay views.

It's responsibility is to provide meaningful and useful interface for the user to interact with.
 
Typical actions which need to be allowed to the user are:

- intuitive and meaniningful way to guide the user through scanning process. This is usually done by presenting a "viewfinder" in which the user need to place the scanned object
- a way to cancel the scanning, typically with a "cancel" or "back" button
- a way to power on and off the light (i.e. "torch") button
 
BlinkID SDK always provides it's own default implementation of the Overlay View Controller for every specific use. Your implementation should closely mimic the default implementation as it's the result of thorough testing with end users. Also, it closely matches the underlying scanning technology. 

For example, the scanning technology usually gives results very fast after the user places the device's camera in the expected way above the scanned object. This means a progress bar for the scan is not particularly useful to the user. The majority of time the user spends on positioning the device's camera correctly. That's just an example which demonstrates careful decision making behind default camera overlay view.

### 1. Initialization
 
To use your custom overlay with MicroBlink's camera view, you must first subclass [`MBCustomOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBCustomOverlayViewController.html) and implement the overlay behaviour conforming wanted protocols.

### 2. Protocols

There are five [`MBRecognizerRunnerViewController`](http://photopay.github.io/photopay-ios/Protocols/MBRecognizerRunnerViewController.html) protocols and one overlay protocol [`MBOverlayViewControllerInterface`](http://photopay.github.io/photopay-ios/Protocols/MBOverlayViewControllerInterface.html).

Five `RecognizerRunnerView` protocols are:
- [`MBScanningRecognizerRunnerViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBScanningRecognizerRunnerViewControllerDelegate.html)
- [`MBDetectionRecognizerRunnerViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBDetectionRecognizerRunnerViewControllerDelegate.html)
- [`MBOcrRecognizerRunnerViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBOcrRecognizerRunnerViewControllerDelegate.html)
- [`MBDebugRecognizerRunnerViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBDebugRecognizerRunnerViewControllerDelegate.html)
- [`MBRecognizerRunnerViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBRecognizerRunnerViewControllerDelegate.html)

In `viewDidLoad`, other protocol conformation can be done and it's done on `recognizerRunnerViewController` property of [`MBOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBOverlayViewController.html), for example:

Swift and Objective-C
```swift
self.scanningRecognizerRunnerViewControllerDelegate = self;
```

### 3. Overlay subviews
Developer needs to know which subivew is needed for custom view controller. If you want to use built-in implementation we recommend to use [`MBModernViewfinderOverlaySubview`](http://photopay.github.io/photopay-ios/Classes/MBModernViewfinderOverlaySubview.html). In can be initialized in `viewDidLoad` method:

Swift
```swift
viewfinderSubview = MBModernViewfinderOverlaySubview()
viewfinderSubview.moveable = true
view.addSubview(viewfinderSubview)
```
Objective-C
```objective-c
self.viewfinderSubview = [[MBModernViewfinderOverlaySubview alloc] init];
self.viewfinderSubview.delegate = self.overlaySubviewsDelegate;
self.viewfinderSubview.moveable = YES;
[self.view addSubview:self.viewfinderSubview];
```

### 4. Initialization
In [Quick Start](#quickStart) guide it is shown how to use [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html). You can now swap [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html) with `CustomOverlayViewController`

Swift
```swift
let recognizerRunnerViewController : UIViewController = MBViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: CustomOverlayViewController)
```

Objective-C
```objective-c
UIViewController<MBRecognizerRunnerViewController>* recognizerRunnerViewController = [MBViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:CustomOverlayViewController];
```


## <a name="directAPI"></a> Direct processing API

This guide will in short present you how to process UIImage objects with PDF417.mobi SDK, without starting the camera video capture.

With this feature you can solve various use cases like:
	- recognizing text on images in Camera roll
	- taking full resolution photo and sending it to processing
	- scanning barcodes on images in e-mail etc.

DirectAPI-sample demo app here will present UIImagePickerController for taking full resolution photos, and then process it with MicroBlink SDK to get scanning results using Direct processing API.

Direct processing API is handled with [`MBRecognizerRunner`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerRunner.html). That is a class that handles processing of images. It also has protocols as [`MBRecognizerRunnerViewController`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerRunnerViewController.html).
Developer can choose which protocol to conform:

- [`MBScanningRecognizerRunnerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBScanningRecognizerRunnerDelegate.html)
- [`MBDetectionRecognizerRunnerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBDetectionRecognizerRunnerDelegate.html)
- [`MBDebugRecognizerRunnerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBDebugRecognizerRunnerDelegate.html)
- [`MBOcrRecognizerRunnerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBOcrRecognizerRunnerDelegate.html)

In example, we are conforming to [`MBScanningRecognizerRunnerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBScanningRecognizerRunnerDelegate.html) protocol.

To initiate the scanning process, first decide where in your app you want to add scanning functionality. Usually, users of the scanning library have a button which, when tapped, starts the scanning process. Initialization code is then placed in touch handler for that button. Here we're listing the initialization code as it looks in a touch handler method.

Swift
```swift
func setupRecognizerRunner() {
    var recognizers = [MBRecognizer]()
    pdf417Recognizer = MBPdf417Recognizer()
    recognizers.append(pdf417Recognizer!)
    let recognizerCollection = MBRecognizerCollection(recognizers: recognizers)
    recognizerRunner = MBRecognizerRunner(recognizerCollection: recognizerCollection)
    recognizerRunner?.scanningRecognizerRunnerDelegate = self
}

func processImageRunner(_ originalImage: UIImage) {
    var image: MBImage? = nil
    if let anImage = originalImage {
        image = MBImage(uiImage: anImage)
    }
    image?.cameraFrame = true
    image?.orientation = MBProcessingOrientation.left
    let _serialQueue = DispatchQueue(label: "com.microblink.DirectAPI-sample-swift")
    _serialQueue.async(execute: {() -> Void in
        self.recognizerRunner?.processImage(image!)
    })
}

func recognizerRunner(_ recognizerRunner: MBRecognizerRunner, didFinishScanningWith state: MBRecognizerResultState) {
    if blinkInputRecognizer.result.resultState == MBRecognizerResultStateValid {
        // Handle result
    }
}
```

Objective-C
```objective-c
- (void)setupRecognizerRunner {
    NSMutableArray<MBRecognizer *> *recognizers = [[NSMutableArray alloc] init];
    
    self.blinkInputRecognizer = [[MBBlinkInputRecognizer alloc] init];
    
    [recognizers addObject:self.blinkInputRecognizer];
    
    MBRecognizerCollection *recognizerCollection = [[MBRecognizerCollection alloc] initWithRecognizers:recognizers];
    
    self.recognizerRunner = [[MBRecognizerRunner alloc] initWithRecognizerCollection:recognizerCollection];
    self.recognizerRunner.scanningRecognizerRunnerDelegate = self;
}

- (void)processImageRunner:(UIImage *)originalImage {
    MBImage *image = [MBImage imageWithUIImage:originalImage];
    image.cameraFrame = YES;
    image.orientation = MBProcessingOrientationLeft;
    dispatch_queue_t _serialQueue = dispatch_queue_create("com.microblink.DirectAPI-sample", DISPATCH_QUEUE_SERIAL);
    dispatch_async(_serialQueue, ^{
        [self.recognizerRunner processImage:image];
    });
}

#pragma mark - MBScanningRecognizerRunnerDelegate
- (void)recognizerRunner:(nonnull MBRecognizerRunner *)recognizerRunner didFinishScanningWithState:(MBRecognizerResultState)state {
    if (self.blinkInputRecognizer.result.resultState == MBRecognizerResultStateValid) {
        // Handle result
    }
}
```

Now you've seen how to implement the Direct processing API.

In essence, this API consists of two steps:

- Initialization of the scanner.
- Call of `- (void)processImage:(MBImage *)image;` method for each UIImage or CMSampleBufferRef you have.


### <a name="directAPI_strings"></a> Using Direct API for `NSString` recognition (parsing)

Some recognizers support recognition from `NSString`. They can be used through Direct API to parse given `NSString` and return data just like when they are used on an input image. When recognition is performed on `NSString`, there is no need for the OCR. Input `NSString` is used in the same way as the OCR output is used when image is being recognized. 
Recognition from `String` can be performed in the same way as recognition from image. 
The only difference is that user should call `- (void)processString:(NSString *)string;` on [`MBRecognizerRunner`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerRunner.html).

# <a name="availableRecognizers"></a> `MBRecognizer` and available recognizers

## <a name="recognizerConcept"></a> The `MBRecognizer` concept

The [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) is the basic unit of processing within the SDK. Its main purpose is to process the image and extract meaningful information from it. As you will see [later](#recognizerList), the SDK has lots of different [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects that have various purposes.

Each [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) has a [`MBRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerResult.html) object, which contains the data that was extracted from the image. The [`MBRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerResult.html) object is a member of corresponding [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) object its lifetime is bound to the lifetime of its parent [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) object. If you need your `MBRecognizerRecognizer` object to outlive its parent [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) object, you must make a copy of it by calling its method `copy`.

While [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) object works, it changes its internal state and its result. The [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) object's [`MBRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerResult.html) always starts in `Empty` state. When corresponding [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) object performs the recognition of given image, its [`MBRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerResult.html) can either stay in `Empty` state (in case [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html)failed to perform recognition), move to `Uncertain` state (in case [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) performed the recognition, but not all mandatory information was extracted) or move to `Valid` state (in case [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) performed recognition and all mandatory information was successfully extracted from the image).

As soon as one [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) object's [`MBRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerResult.html) within [`MBRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerCollection.html) given to `MBRecognizerRunner` or `MBRecognizerRunnerViewController` changes to `Valid` state, the `onScanningFinished` callback will be invoked on same thread that performs the background processing and you will have the opportunity to inspect each of your [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects' [`MBRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerResult.html) to see which one has moved to `Valid` state.

As soon as `onScanningFinished` method ends, the `MBRecognizerRunnerViewController` will continue processing new camera frames with same [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects, unless `paused`. Continuation of processing or `reset` recognition will modify or reset all [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects's [`MBRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerResult.html). When using built-in activities, as soon as `onScanningFinished` is invoked, built-in activity pauses the `MBRecognizerRunnerViewController` and starts finishing the activity, while saving the [`MBRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerCollection.html) with active [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html).

## <a name="recognizerBCollection"></a> `MBRecognizerCollection` concept

The [`MBRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerCollection.html) is is wrapper around [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects that has array of [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects that can be used to give [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects to `MBRecognizerRunner` or `MBRecognizerRunnerViewController` for processing.

The [`MBRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerCollection.html) is always constructed with array `[[MBRecognizerCollection alloc] initWithRecognizers:recognizers]` of [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects that need to be prepared for recognition (i.e. their properties must be tweaked already). 

The [`MBRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerCollection.html) manages a chain of [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects within the recognition process. When a new image arrives, it is processed by the first [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) in chain, then by the second and so on, iterating until a [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) object's [`MBRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerResult.html) changes its state to `Valid` or all of the [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects in chain were invoked (none getting a `Valid` result state).

You cannot change the order of the [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects within the chain - no matter the order in which you give [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects to [`MBRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerCollection.html), they are internally ordered in a way that provides best possible performance and accuracy. Also, in order for SDK to be able to order [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects in recognition chain in a best way possible, it is not allowed to have multiple instances of [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects of the same type within the chain. Attempting to do so will crash your application.

## <a name="recognizerList"></a> List of available recognizers

This section will give a list of all [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) objects that are available within PDF417.mobi SDK, their purpose and recommendations how they should be used to get best performance and user experience.

### <a name="frameGrabberRecognizer"></a> Frame Grabber Recognizer

The [`MBFrameGrabberRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBFrameGrabberRecognizer.html) is the simplest recognizer in SDK, as it does not perform any processing on the given image, instead it just returns that image back to its `onFrameAvailable`. Its result never changes state from empty.

This recognizer is best for easy capturing of camera frames with `MBRecognizerRunnerViewController`. Note that [`MBImage`](http://photopay.github.io/photopay-ios/Classes/MBImage.html) sent to `onFrameAvailable` are temporary and their internal buffers all valid only until the `onFrameAvailable` method is executing - as soon as method ends, all internal buffers of [`MBImage`](http://photopay.github.io/photopay-ios/Classes/MBImage.html) object are disposed. If you need to store [`MBImage`](http://photopay.github.io/photopay-ios/Classes/MBImage.html) object for later use, you must create a copy of it by calling `copy`.

### <a name="successFrameGrabberRecognizer"></a> Success Frame Grabber Recognizer

The [`MBSuccessFrameGrabberRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSuccessFrameGrabberRecognizer.html) is a special `MBecognizer` that wraps some other [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) and impersonates it while processing the image. However, when the [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) being impersonated changes its [`MBRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerResult.html) into `Valid` state, the [`MBSuccessFrameGrabberRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSuccessFrameGrabberRecognizer.html) captures the image and saves it into its own [`MBSuccessFrameGrabberRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBSuccessFrameGrabberRecognizerResult.html) object.

Since [`MBSuccessFrameGrabberRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSuccessFrameGrabberRecognizer.html)  impersonates its slave [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) object, it is not possible to give both concrete [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) object and `MBSuccessFrameGrabberRecognizer` that wraps it to same `MBRecognizerCollection` - doing so will have the same result as if you have given two instances of same [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) type to the [`MBRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerCollection.html) - it will crash your application.

This recognizer is best for use cases when you need to capture the exact image that was being processed by some other [`MBRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRecognizer.html) object at the time its [`MBRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBRecognizerResult.html) became `Valid`. When that happens, `MBSuccessFrameGrabberRecognizer's` `MBSuccessFrameGrabberRecognizerResult` will also become `Valid` and will contain described image.

### <a name="pdf417Recognizer"></a> PDF417 recognizer

The [`MBPdf417Recognizer`](http://photopay.github.io/photopay-ios/Classes/MBPdf417Recognizer.html) is recognizer specialised for scanning [PDF417 2D barcodes](https://en.wikipedia.org/wiki/PDF417). This recognizer can recognize only PDF417 2D barcodes - for recognition of other barcodes, please refer to [BarcodeRecognizer](#barcodeRecognizer).

This recognizer can be used in any overlay view controller, but it works best with the [`MBBarcodeOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBBarcodeOverlayViewController.html), which has UI best suited for barcode scanning.

### <a name="barcodeRecognizer"></a> Barcode recognizer

The [`MBBarcodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBBarcodeRecognizer.html) is recognizer specialised for scanning various types of barcodes. This recognizer should be your first choice when scanning barcodes as it supports lots of barcode symbologies, including the [PDF417 2D barcodes](https://en.wikipedia.org/wiki/PDF417), thus making [PDF417 recognizer](#pdf417Recognizer) possibly redundant, which was kept only for its simplicity.

You can enable multiple barcode symbologies within this recognizer, however keep in mind that enabling more barcode symbologies affect scanning performance - the more barcode symbologies are enabled, the slower the overall recognition performance. Also, keep in mind that some simple barcode symbologies that lack proper redundancy, such as [Code 39](https://en.wikipedia.org/wiki/Code_39), can be recognized within more complex barcodes, especially 2D barcodes, like [PDF417](https://en.wikipedia.org/wiki/PDF417).

This recognizer can be used in any overlay view controller, but it works best with the [`MBBarcodeOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBBarcodeOverlayViewController.html), which has UI best suited for barcode scanning.

### <a name="blinkInputRecognizer"></a> BlinkInput recognizer

The [`MBBlinkInputRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBBlinkInputRecognizer.html) is generic OCR recognizer used for scanning segments which enables specifying `MBProcessors` that will be used for scanning. Most commonly used `MBProcessor` within this recognizer is [`MBParserGroupProcessor`](http://photopay.github.io/photopay-ios/Classes/MBParserGroupProcessor.html)) that activates all `MBParsers` in the group to extract data of interest from the OCR result.

This recognizer can be used in any context. It is used internally in the implementation of the provided [`MBFieldByFieldOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBFieldByFieldOverlayViewController.html).

`MBProcessors` are explained in [The Processor concept](#processorConcept) section and you can find more about `MBParsers` in [The Parser concept](#parserConcept) section.

### <a name="detectorRecognizer"></a> Detector recognizer

The [`MBDetectorRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBDetectorRecognizer.html) is recognizer for scanning generic documents using custom `MBDetector`. You can find more about `Detector` in [The Detector concept](#detectorConcept) section. `MBDetectorRecognizer` can be used simply for document detection and obtaining its image. The more interesting use case is data extraction from the custom document type. `MBDetectorRecognizer` performs document detection and can be configured to extract fields of interest from the scanned document by using **Templating API**. You can find more about Templating API in [this](#detectorTemplating) section.

#### <a name="documentFaceBlinkId"></a> Document Face

The [`MBDocumentFaceRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBCzechiaIDFrontRecognizer.html) is recognizer specialised scanning Document Face.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentFaceRecognizer.html), which has UI best suited for one side document scanning.

### <a name="blinkIdRecognizersByCountry"></a> BlinkID recognizers by countries

#### <a name="austriaBlinkId"></a> Austria

The [`MBAustriaIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBAustriaIdFrontRecognizer.html) is recognizer specialised for scanning front side of Austrian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBAustriaIdBackRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBAustriaIdBackRecognizer.html) is recognizer specialised for scanning back side of Austrian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBAustriaPassportRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBAustriaPassportRecognizer.html) is recognizer specialised for scanning Austrian Passports.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBAustriaCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBAustriaCombinedRecognizer.html) is recognizer specialised for scanning both front and back side of Austrian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html), which has UI best suited for both side document scanning.

#### <a name="australiaBlinkId"></a> Australia

The [`MBAustraliaDlFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBAustraliaDlFrontRecognizer.html) is recognizer specialised for scanning front side of Australian Driver's License.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBAustraliaDlBackRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBAustraliaDlBackRecognizer.html) is recognizer specialised for scanning back side of Australian Driver's License.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="colombiaBlinkId"></a> Colombia

The [`MBColombiaIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBColombiaIdFrontRecognizer.html) is recognizer specialised for scanning front side of Colombian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBColombiaIdBackRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBColombiaIdBackRecognizer.html) is recognizer specialised for scanning back side of Colombian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="croatiaBlinkId"></a> Croatia

The [`MBCroatiaIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBCroatiaIdFrontRecognizer.html) is recognizer specialised for scanning front side of Croatian ID. It always extracts
identity card number, first and last name of ID holder while extracting other elements is optional.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBCroatiaIdBackRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBCroatiaIdBackRecognizer.html) is recognizer specialised for scanning back side of Croatian ID. It always extracts
MRZ zone and address of ID holder while extracting other elements is optional.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBCroatiaCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBCroatiaCombinedRecognizer.html) is recognizer specialised for scanning both front and back side of Croatian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html), which has UI best suited for both side document scanning.

#### <a name="czechiaBlinkId"></a> Czechia

The [`MBCzechiaIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBCzechiaIdFrontRecognizer.html) is recognizer specialised for scanning front side of Czech ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBCzechiaIdBackRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBBackIdBackRecognizer.html) is recognizer specialised for scanning back side of Czech ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBCzechiaCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBCzechiaCombinedRecognizer.html) is recognizer specialised for scanning both front and back side of Czech ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html), which has UI best suited for both side document scanning.

#### <a name="eudlBlinkId"></a> European Driver License

The [`MBEudlRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBEudlRecognizer.html) is recognizer specialised for scanning EU Driver License. Supported countries are Austria, Germany, United Kingdom and any (generic) EU driver license. List can be found in [`MBEudlCountry`](http://photopay.github.io/photopay-ios/Enums/MBEudlCountry.html)

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="egyptBlinkId"></a> Egypt

The [`MBEgyptIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBEgyptIdFrontRecognizer.html) is recognizer specialised for scanning front side of Egypt ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="croatiaBlinkId"></a> Germany

The [`MBGermanyIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBGermanyIdFrontRecognizer.html) is recognizer specialised for scanning front side of German ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBGermanyIdBackRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBGermanyIdBackRecognizer.html) is recognizer specialised for scanning back side of German ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBGermanyCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBGermanyCombinedRecognizer.html) is recognizer specialised for scanning both front and back side of German ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html), which has UI best suited for both side document scanning.

The [`MBGermanOldIDRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBGermanOldIDRecognizer.html) is recognizer specialised for scanning old German ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBGermanyPassportRecognizer.h`](http://photopay.github.io/photopay-ios/Classes/MBGermanyPassportRecognizer.h.html) is recognizer specialised for scanning German Passports.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="hongkongBlinkId"></a> Hong Kong

The [`MBHongKongIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBHongKongIdFrontRecognizer.html) is recognizer specialised for scanning front side of Hong Kong ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="indonesiaBlinkId"></a> Indonesia

The [`MBIndonesiaIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBIndonesiaIdFrontRecognizer.html) is recognizer specialised for scanning front side of Indonesian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="jordanBlinkId"></a> Jordan

The [`MBJordanIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBJordanIdFrontRecognizer.html) is recognizer specialised for scanning front side of Jordan ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBJordanIdBackRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBJordanIdBackRecognizer.html) is recognizer specialised for scanning back side of Jordan ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBJordanCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBJordanCombinedRecognizer.html) is recognizer specialised for scanning both front and back side of Jordan ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html), which has UI best suited for both side document scanning.

#### <a name="malaysiaBlinkId"></a> Malaysia

The [`MBMalaysiaDLFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBMalaysiaDLFrontRecognizer.html) is recognizer specialised for scanning Malaysian Driver's License.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBMyKadFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBMyKadFrontRecognizer.html) is recognizer specialised for scanning front side of MyKad.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBMyKadBackRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBMyKadBackRecognizer.html) is recognizer specialised for scanning back side of MyKad.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBMyTenteraRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBMyTenteraRecognizer.html) is recognizer specialised for scanning MyTentera.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBiKadRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBiKadRecognizer.html) is recognizer specialised for scanning iKad.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="moroccoBlinkId"></a> Morocco

The [`MBMoroccoIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBMoroccoIdFrontRecognizer.html) is recognizer specialised for scanning front side of Morocco ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="newZealandBlinkId"></a> New Zealand

The [`MBNewZealandDLFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBNewZealandDLFrontRecognizer.html) is recognizer specialised for scanning front side of New Zealand Driver's License.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="polandBlinkId"></a> Poland

The [`MBPolandIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPolandIdFrontRecognizer.html) is recognizer specialised for scanning front side of Polish ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBPolandIdBackRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPolandIdBackRecognizer.html) is recognizer specialised for scanning back side of Polish ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBPolandCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPolandCombinedRecognizer.html) is recognizer specialised for scanning both front and back side of Polish ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html), which has UI best suited for both side document scanning.

#### <a name="romaniaBlinkId"></a> Romania

The [`MBRomaniaIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBRomaniaIdFrontRecognizer.html) is recognizer specialised for scanning front side of Romanian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="serbiaBlinkId"></a> Serbia

The [`MBSerbiaIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSerbiaIdFrontRecognizer.html) is recognizer specialised for scanning front side of Serbian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSerbiaIdBackRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSerbiaIdBackRecognizer.html) is recognizer specialised for scanning back side of Serbian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSerbiaCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSerbiaCombinedRecognizer.html) is recognizer specialised for scanning both front and back side of Serbian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html), which has UI best suited for both side document scanning.

#### <a name="singaporeBlinkId"></a> Singapore

The [`MBSingaporeIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSingaporeIdFrontRecognizer.html) is recognizer specialised for scanning front side of Singapore ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSingaporeIdBackRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSingaporeIdBackRecognizer.html) is recognizer specialised for scanning back side of Singapore ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSingaporeCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSingaporeCombinedRecognizer.html) is recognizer specialised for scanning both front and back side of Singapore ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html), which has UI best suited for both side document scanning.

The [`MBSingaporeDlFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSingaporeDlFrontRecognizer.html) is recognizer specialised for scanning front side of Singapore Driver's License.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="slovakiaBlinkId"></a> Slovakia

The [`MBSlovakiaIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSlovakiaIdFrontRecognizer.html) is recognizer specialised for scanning front side of Slovakian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSlovakiaIdBackRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSlovakiaIdBackRecognizer.html) is recognizer specialised for scanning back side of Slovakian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSlovakiaCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSlovakiaCombinedRecognizer.html) is recognizer specialised for scanning both front and back side of Slovakian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html), which has UI best suited for both side document scanning.

#### <a name="sloveniaBlinkId"></a> Slovenia

The [`MBSloveniaIdFrontRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSloveniaIdFrontRecognizer.html) is recognizer specialised for scanning front side of Slovenian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSloveniaIdBackRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSloveniaIdBackRecognizer.html) is recognizer specialised for scanning back side of Slovenian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSloveniaCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSloveniaCombinedRecognizer.html) is recognizer specialised for scanning both front and back side of Slovenian ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html), which has UI best suited for both side document scanning.

#### <a name="swedenBlinkId"></a> Sweden

The [`MBSwedenDlFrontRecognizer.h`](http://photopay.github.io/photopay-ios/Classes/MBSwedenDlFrontRecognizer.html) is recognizer specialised for scanningSwedish Driver's License.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="switzerlandBlinkId"></a> Switzerland

The [`MBSwitzerlandIdFrontRecognizer.h`](http://photopay.github.io/photopay-ios/Classes/MBSwitzerlandIdFrontRecognizer.html) is recognizer specialised for scanning front side of Swiss ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSwitzerlandIdBackRecognizer.h`](http://photopay.github.io/photopay-ios/Classes/MBSwitzerlandIdBackRecognizer.html) is recognizer specialised for scanning back side of Swiss ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSwitzerlandPassportRecognizer.h.h`](http://photopay.github.io/photopay-ios/Classes/MBSwitzerlandIdBackRecognizer.html) is recognizer specialised for scanning of Swiss Passports.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSwitzerlandDlFrontRecognizer.h`](http://photopay.github.io/photopay-ios/Classes/MBSwitzerlandDlFrontRecognizer.html) is recognizer specialised for scanning front side of Swiss Driver's License.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="uaeBlinkId"></a> United Arab Emirates

The [`MBUnitedArabEmiratesIdFrontRecognizer.h`](http://photopay.github.io/photopay-ios/Classes/MBUnitedArabEmiratesIdFrontRecognizer.html) is recognizer specialised for scanning front side of UAE ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBUnitedArabEmiratesIdBackRecognizer.h`](http://photopay.github.io/photopay-ios/Classes/MBUnitedArabEmiratesIdBackRecognizer.html) is recognizer specialised for scanning back side of UAE ID.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="unitedStatesBlinkId"></a> United States

The [`MBUsdlRecognizer.h`](http://photopay.github.io/photopay-ios/Classes/MBUsdlRecognizer.html) is recognizer specialised for scanning back side of US Driver's License.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBUsdlCombinedRecognizer.h`](http://photopay.github.io/photopay-ios/Classes/MBUsdlCombinedRecognizer.html) is recognizer specialised for scanning both front and back side of US Driver's License.

This recognizer can be used in any overlay view controller, but it works best with the [`MBDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBDocumentVerificationOverlayViewController.html), which has UI best suited for both side document scanning.

### <a name="photopayRecognizersByCountry"></a> PhotoPay recognizers by countries

#### <a name="austriaPhotoPay"></a> Austria

The [`MBAustriaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBAustriaQrCodeRecognizer.html) is recognizer specialised for scanning Austrian payment QR codes, such as Stuzza codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBAustriaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBAustriaSlipRecognizer.html) is recognizer specialised for scanning Austrian payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="belgiumPhotoPay"></a> Belgium

The [`MBBelgiumSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBBelgiumSlipRecognizer.html) is recognizer specialised for scanning Belgian payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="croatiaPhotoPay"></a> Croatia

The [`MBCroatiaPdf417Recognizer`](http://photopay.github.io/photopay-ios/Classes/MBCroatiaPdf417Recognizer.html) is recognizer specialised for scanning Croatia payment Pdf417 codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBCroatiaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBCroatiaQrCodeRecognizer.html) is recognizer specialised for scanning Croatia payment QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBCroatiaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBCroatiaSlipRecognizer.html) is recognizer specialised for scanning Croatia payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for both side document scanning.

#### <a name="czechiaPhotoPay"></a> Czechia

The [`MBCzechiaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBCzechiaQrCodeRecognizer.html) is recognizer specialised for scanning Czech payment QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBCzechiaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBCzechiaSlipRecognizer.html) is recognizer specialised for scanning Czech payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="germanyPhotoPay"></a> Germany

The [`MBGermanyQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBGermanyQrCodeRecognizer.html) is recognizer specialised for scanning German payment QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBGermanySlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBGermanySlipRecognizer.html) is recognizer specialised for scanning German payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="hungaryPhoPay"></a> Hungary

The [`MBHungarySlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBHungarySlipRecognizer.html) is recognizer specialised for scanning Hungarian payment slips - white and yellow.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="kosovoPhotoPay"></a> Kosovo

The [`MBKosovoSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBKosovoSlipRecognizer.html) is recognizer specialised for scanning Kosovo payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBFieldOfViewOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBFieldOfViewOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBKosovoCode128Recognizer`](http://photopay.github.io/photopay-ios/Classes/MBKosovoCode128Recognizer.html) is recognizer specialised for scanning code 128 barcodes found on Kosovo payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBFieldOfViewOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBFieldOfViewOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="netherlandsPhotoPay"></a> Netherlands

The [`MBNetherlandsSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBNetherlandsSlipRecognizer.html) is recognizer specialised for scanning Dutch payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="sepaPhotoPay"></a> SEPA

The [`MBSepaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSepaQrCodeRecognizer.html) is recognizer specialised for scanning SEPA QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="serbiaPhotoPay"></a> Serbia

The [`MBSerbiaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSerbiaQrCodeRecognizer.html) is recognizer specialised for scanning Serbian QR payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSerbiaPdf417Recognizer`](http://photopay.github.io/photopay-ios/Classes/MBSerbiaPdf417Recognizer.html) is recognizer specialised for scanning Serbian PDF 417 payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="slovakiaPhotoPay"></a> Slovakia

The [`MBSlovakiaCode128Recognizer`](http://photopay.github.io/photopay-ios/Classes/MBSlovakiaCode128Recognizer.html) is recognizer specialised for scanning Slovakian CODE 128 payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSlovakiaDataMatrixRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSlovakiaDataMatrixRecognizer.html) is recognizer specialised for scanning Slovakian Data Matrix payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSlovakiaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSlovakiaQrCodeRecognizer.html) is recognizer specialised for scanning Slovakian QR payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for both side document scanning.

The [`MBSlovakiaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSlovakiaSlipRecognizer.html) is recognizer specialised for scanning Slovakian payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for both side document scanning.

#### <a name="sloveniaPhotoPay"></a> Slovenia

The [`MBSloveniaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSloveniaSlipRecognizer.html) is recognizer specialised for scanning Slovenian UPN payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="switzerlandPhotoPay"></a> Switzerland

The [`MBSwitzerlandQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSwitzerlandQrCodeRecognizer.html) is recognizer specialised for scanning Swiss payment QR codes, such as Stuzza codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBSwitzerlandSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBSwitzerlandSlipRecognizer.html) is recognizer specialised for scanning Swiss payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

#### <a name="unitedStatesBlinkId"></a> United Kingdom

The [`MBUnitedKingdomQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBUnitedKingdomQrCodeRecognizer.html) is recognizer specialised for scanning UK payment QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBUnitedKingdomSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBUnitedKingdomSlipRecognizer.html) is recognizer specialised for scanning UK payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPhotopayOverlayViewController.html), which has UI best suited for both side document scanning.# <a name="fieldScan"></a> `Field by field` scanning feature

[`Field by field`](#fieldByFieldFeature) scanning feature is designed for scanning small text fields which are called scan elements. For each scan element, specific [`MBParser`](#parserConcept) that will extract structured data of interest from the OCR result is defined. Focusing on the small text fields which are scanned one by one enables implementing support for the **free-form documents** because field detection is not required. The user is responsible for positioning the field of interest inside the scanning window and the scanning process guides him. When implementing support for the custom document, only fields of interest has to be defined.

[`Field by field`](#fieldByFieldFeature) approaches are described in the following sections.

## <a name="fieldByFieldFeature"></a> `Field by field` feature

`Field by field` feature is designed for scanning small text fields in the predefined order by using [`MBFieldByFieldViewController`](#fieldByFieldUiComponent).

To start with Field by Field feature all you need to do is to initialize MBFieldByFieldOverlayViewController and conform to [`MBFieldByFieldOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBFieldByFieldOverlayViewControllerDelegate.html) (this example follows our FieldByField-sample-Swift project):

Swift
```swift
    // Create MBFieldByFieldOverlaySettings
    let settings = MBFieldByFieldOverlaySettings(scanElements: MBGenericPreset.getPreset()!)
    
    // Create field by field VC
    let fieldByFieldVC = MBFieldByFieldOverlayViewController(settings: settings, delegate: self)
    
    // Create scanning VC
    let recognizerRunnerViewController: (UIViewController & MBRecognizerRunnerViewController)? = MBViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: fieldByFieldVC)
    
    // Present VC
    self.present(recognizerRunnerViewController!, animated: true, completion: nil)


    func field(_ fieldByFieldOverlayViewController: MBFieldByFieldOverlayViewController, didFinishScanningWith scanElements: [MBScanElement]) {
    	// Whatever you want to do with results
    }
```

Objective-C
```objective-c
	// Create MBFieldByFieldOverlaySettings
	MBFieldByFieldOverlaySettings *settings = [[MBFieldByFieldOverlaySettings alloc] initWithScanElements: [MBGenericPreset getPreset] initWithSettings: settings, delegate: self];

	// Create field by field VC
	MBFieldByFieldOverlayViewController *fieldByFieldOverlayViewController =  [[MBFieldByFieldOverlayViewController alloc] initWithSettings:settings delegate: self];

	// Create scanning VC
	UIViewController<MBRecognizerRunnerViewController>* recognizerRunnerViewController = [MBViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:fieldByFieldOverlayViewController];

	/ Present VC
	[self presentViewController:recognizerRunnerViewController animated:YES completion:nil];

	- (void)fieldByFieldOverlayViewController:(MBFieldByFieldOverlayViewController *)fieldByFieldOverlayViewController didFinishScanningWithElements:(NSArray<MBScanElement *> *)scanElements {
		// Whatever you want to do with results
	}
```

# <a name="processorsAndParsers"></a> `MBProcessor` and `MBParser`

The `MBProcessors` and `MBParsers` are standard processing units within *BlinkID* SDK used for data extraction from the input images. Unlike the [`MBRecognizer`](#recognizerConcept), `MBProcessor` and `MBParser` are not stand-alone processing units. `MBProcessor` is always used within `MBRecognizer` and `MBParser` is used within appropriate `MBProcessor` to extract data from the OCR result.

## <a name="processorConcept"></a> The `MBProcessor` concept

`MBProcessor` is a processing unit used within some `Recognizer` which supports processors. It process the input image prepared by the enclosing `Recognizer` in the way that is characteristic to the implementation of the concrete `MBProcessor`.

`MBProcessor` architecture is similar to `MBRecognizer` architecture described in [The Recognizer concept](#recognizerConcept) section. Each instance also has associated inner `MBRecognizerResult` object whose lifetime is bound to the lifetime of its parent `MBProcessor` object and it is updated while `MBProcessor` works. If you need your `MBRecognizerResult` object to outlive its parent `MBProcessor` object, you must make a copy of it by calling its method `copy`.

It also has its internal state and while it is in the *working state* during recognition process, it is not allowed to tweak `MBProcessor` object's properties.

To support common use cases, there are several different `MBProcessor` implementations available. They are listed in the next section.

## <a name="processorList"></a> List of available processors

This section will give a list of `MBProcessor` types that are available within *BlinkID* SDK and their purpose.

### <a name="imageReturnProcessor"></a> Image Return Processor

The [`MBImageReturnProcessor`](http://photopay.github.io/photopay-ios/Classes/MBImageReturnProcessor.html) is used for obtaining input images. It simply saves the input image and makes it available after the scanning is done.

The appearance of the input image depends on the context in which `MBImageReturnProcessor` is used. For example, when it is used within [`MBBlinkInputRecognizer`](#blinkInputRecognizer), simply the raw image of the scanning region is processed. When it is used within the [`Templating API`](#detectorTemplating), input image is dewarped (cropped and rotated).
 
The image is returned as the raw [`MBImage`](http://photopay.github.io/photopay-ios/Classes/MBImage.html) type. Also, processor can be configured to [encode saved image to JPEG](http://photopay.github.io/photopay-ios/Classes/MBImageReturnProcessor.html).

### <a name="parserGroupProcessor"></a> Parser Group Processor


The [`MBParserGroupProcessor`](http://photopay.github.io/photopay-ios/Classes/MBParserGroupProcessor.html) is the type of the processor that performs the OCR (*Optical Character Recognition*) on the input image and lets all the parsers within the group to extract data from the OCR result. The concept of `MBParser` is described in [the next](#parserConcept) section.

Before performing the OCR, the best possible OCR engine options are calculated by combining engine options needed by each `MBParser` from the group. For example, if one parser expects and produces result from uppercase characters and other parser extracts data from digits, both uppercase characters and digits must be added to the list of allowed characters that can appear in the OCR result. This is a simplified explanation because OCR engine options contain many parameters which are combined by the `MBParserGroupProcessor`.

Because of that, if multiple parsers and multiple parser group processors are used during the scan, it is very important to group parsers carefully.

Let's see this on an example: assume that we have two parsers at our disposal: `MBAmountParser` and `MBEmailParser`. `MBAmountParser` knows how to extract amount's from OCR result and requires from OCR only to recognize digits, periods and commas and ignore letters. On the other hand, `MBEmailParser` knows how to extract e-mails from OCR result and requires from OCR to recognize letters, digits, '@' characters and periods, but not commas. 

If we put both `MBAmountParser` and `MBEmailParser` into the same `MBParserGroupProcessor`, the merged OCR engine settings will require recognition of all letters, all digits, '@' character, both period and comma. Such OCR result will contain all characters for `MBEmailParser` to properly parse e-mail, but might confuse `MBAmountParser` if OCR misclassifies some characters into digits.

If we put `MBAmountParser` in one `MBParserGroupProcessor` and `MBEmailParser` in another `MBParserGroupProcessor`, OCR will be performed for each parser group independently, thus preventing the `MBAmountParser` confusion, but two OCR passes of the image will be performed, which can have a performance impact.

`MBParserGroupProcessor` is most commonly used `MBProcessor`. It is used whenever the OCR is needed. After the OCR is performed and all parsers are run, parsed results can be obtained through parser objects that are enclosed in the group. `MBParserGroupProcessor` instance also has associated inner `MBParserGroupProcessorResult` whose state is updated during processing and its property [`ocrLayout`](http://photopay.github.io/photopay-ios/Classes/MBParserGroupProcessor.html) can be used to obtain the raw [`MBOcrLayout`](http://photopay.github.io/photopay-ios/Classes/MBOcrLayout.html) that was used for parsing data.

Take note that `MBOcrLayout` is available only if it is allowed by the *BlinkID* SDK license key. `MBOcrLayout` structure contains information about all recognized characters and their positions on the image. To prevent someone to abuse that, obtaining of the `MBOcrLayout` structure is allowed only by the premium license keys.

## <a name="parserConcept"></a> The `MBParser` concept

`MBParser` is a class of objects that are used to extract structured data from the raw OCR result. It must be used within `MBParserGroupProcessor` which is responsible for performing the OCR, so `MBParser` is not stand-alone processing unit.

Like [`MBRecognizer`](#recognizerConcept) and all other processing units, each `MBParser` instance has associated inner `MBRecognizerResult` object whose lifetime is bound to the lifetime of its parent `MBParser` object and it is updated while `MBParser` works. When parsing is done `MBParserResult` can be used for obtaining extracted data. If you need your `MBParserResult` object to outlive its parent `MBParser` object, you must make a copy of it by calling its method `copy`.

It also has its internal state and while it is in the *working state* during recognition process, it is not allowed to tweak `MBParser` object's properties.

There are a lot of different `MBParsers` for extracting most common fields which appear on various documents. Also, most of them can be adjusted for specific use cases. For all other custom data fields, there is `RegexParser` available which can be configured with the arbitrary regular expression.

## <a name="parserList"></a> List of available parsers

### <a name="amountParser"></a> Amount Parser

[`MBAmountParser`](http://photopay.github.io/photopay-ios/Classes/MBAmountParser.html) is used for extracting amounts from the OCR result.

### <a name="dateParser"></a> Date Parser

[`MBDateParser`](http://photopay.github.io/photopay-ios/Classes/MBDateParser.html) is used for extracting dates in various formats from the OCR result.

### <a name="emailParser"></a> Email Parser

[`MBEmailParser`](http://photopay.github.io/photopay-ios/Classes/MBEmailParser.html) is used for extracting e-mail addresses from the OCR result.

### <a name="ibanParser"></a> IBAN Parser

[`MBIbanParser`](http://photopay.github.io/photopay-ios/Classes/MBIbanParser.html) is used for extracting IBAN (*International Bank Account Number*) from the OCR result.

### <a name="licensePlatesParser"></a> License Plates Parser

[`MBLicensePlatesParser`](http://photopay.github.io/photopay-ios/Classes/MBLicensePlatesParser.html) is used for extracting license plate content from the OCR result.

### <a name="rawParser"></a> Raw Parser

[`MBRawParser`](http://photopay.github.io/photopay-ios/Classes/MBRawParser.html) is used for obtaining string version of raw OCR result, without performing any smart parsing operations.

### <a name="regexParser"></a> Regex Parser

[`MBRegexParser`](http://photopay.github.io/photopay-ios/Classes/MBRegexParser.html) is used for extracting OCR result content which is in accordance with the given regular expression. Regular expression parsing is not performed with java's regex engine. Instead, it is performed with custom regular expression engine.

### <a name="topUpParser"></a> TopUp Parser

[`MBTopUpParser`](http://photopay.github.io/photopay-ios/Classes/MBTopUpParser.html) is used for extracting TopUp (mobile phone coupon) codes from the OCR result. There exists [`TopUpPreset`](http://photopay.github.io/photopay-ios/Enums/MBTopUpPreset.html) enum with presets for most common vendors. Method `- (void)setTopUpPreset:(MBTopUpPreset)topUpPreset` can be used to configure parser to only return codes with the appropriate format defined by the used preset. 

### <a name="vinParser"></a> VIN (*Vehicle Identification Number*) Parser

[`MBVinParser`](http://photopay.github.io/photopay-ios/Classes/MBVinParser.html) is used for extracting VIN (*Vehicle Identification Number*) from the OCR result.

### <a name="parserListByCountry"></a> List of available parsers by country

#### <a name="australiaParser"></a> Australia

[`MBAustraliaAbnParser`](http://photopay.github.io/photopay-ios/Classes/MBAustraliaAbnParser.html) is used for extracting ABN number from the OCR result.

[`MBAustraliaBillerParser`](http://photopay.github.io/photopay-ios/Classes/MBAustraliaBillerParser.html) is used for extracting biller number from the OCR result.

[`MBAustraliaReferenceParser`](http://photopay.github.io/photopay-ios/Classes/MBAustraliaReferenceParser.html) is used for extracting reference number from the OCR result.

[`MBAustraliaAccountParser`](http://photopay.github.io/photopay-ios/Classes/MBAustraliaAccountParser.html) is used for extracting account number from the OCR result.

[`MBAustraliaBsbParser`](http://photopay.github.io/photopay-ios/Classes/MBAustraliaBsbParser.html) is used for extracting BSB number from the OCR result.

#### <a name="austriaParser"></a> Austria

[`MBAustriaReferenceParser`](http://photopay.github.io/photopay-ios/Classes/MBAustriaReferenceParser.html) is used for extracting reference number from the OCR result.

#### <a name="bihParser"></a> Bosnia and Herzegovina

[`MBBosniaAndHerzegovinaAccountParser`](http://photopay.github.io/photopay-ios/Classes/MBBosniaAndHerzegovinaAccountParser.html) is used for extracting account from the OCR result.

[`MBBosniaAndHerzegovinaReferenceParser`](http://photopay.github.io/photopay-ios/Classes/MBBosniaAndHerzegovinaReferenceParser.html) is used for extracting reference number from the OCR result.

#### <a name="croatiaParser"></a> Croatia

[`MBCroatiaAmountParser`](http://photopay.github.io/photopay-ios/Classes/MBCroatiaAmountParser.html) is used for extracting amounts from the OCR result.

[`MBCroatiaReferenceParser`](http://photopay.github.io/photopay-ios/Classes/MBCroatiaReferenceParser.html) is used for extracting reference number from the OCR result.

#### <a name="czechiaParser"></a> Czechia

[`MBCzechiaAccountParser`](http://photopay.github.io/photopay-ios/Classes/MBCzechiaAccountParser.html) is used for extracting account from the OCR result.

[`MBCzechiaVariabilniSymbolParser`](http://photopay.github.io/photopay-ios/Classes/MBCzechiaVariabilniSymbolParser.html) is used for extracting variabilni symbol from the OCR result.

#### <a name="germanyParser"></a> Germany

[`MBGermanyReferenceParser`](http://photopay.github.io/photopay-ios/Classes/MBGermanyReferenceParser.html) is used for extracting refrence number from the OCR result.

#### <a name="hungaryParser"></a> Hungary

[`MBHungaryAccountParser`](http://photopay.github.io/photopay-ios/Classes/MBHungaryAccountParser.html) is used for extracting account number from the OCR result.

[`MBHungaryPayerIdParser`](http://photopay.github.io/photopay-ios/Classes/MBHungaryPayerIdParser.html) is used for extracting payer ID from the OCR result.

#### <a name="macedoniaParser"></a> Macedonia

[`MBMacedoniaAccountParser`](http://photopay.github.io/photopay-ios/Classes/MBMacedoniaAccountParser.html) is used for extracting account number from the OCR result.

[`MBMacedoniaReferenceParser`](http://photopay.github.io/photopay-ios/Classes/MBMacedoniaReferenceParser.html) is used for extracting reference from the OCR result.

#### <a name="montenegroParser"></a> Montenegro

[`MBMontenegroAccountParser`](http://photopay.github.io/photopay-ios/Classes/MBMontenegroAccountParser.html) is used for extracting account number from the OCR result.

[`MBMontenegroReferenceParser`](http://photopay.github.io/photopay-ios/Classes/MBMontenegroReferenceParser.html) is used for extracting reference from the OCR result.

#### <a name="serbiaParser"></a> Serbia

[`MBSerbiaAccountParser`](http://photopay.github.io/photopay-ios/Classes/MBSerbiaAccountParser.html) is used for extracting account number from the OCR result.

[`MBSerbiaReferenceParser`](http://photopay.github.io/photopay-ios/Classes/MBSerbiaReferenceParser.html) is used for extracting reference from the OCR result.

#### <a name="sloveniaParser"></a> Slovenia

[`MBSloveniaReferenceParser`](http://photopay.github.io/photopay-ios/Classes/MBSloveniaReferenceParser.html) is used for extracting reference from the OCR result.

#### <a name="swedenParser"></a> Sweden

[`MBSwedenAmountParser`](http://photopay.github.io/photopay-ios/Classes/MBSwedenAmountParser.html) is used for extracting amount from the OCR result.

[`MBSwedenGiroNumberParser`](http://photopay.github.io/photopay-ios/Classes/MBSwedenGiroNumberParser.html) is used for extracting giro number from the OCR result.

[`MBSwedenSlipCodeParser`](http://photopay.github.io/photopay-ios/Classes/MBSwedenSlipCodeParser.html) is used for extracting slip code from the OCR result.

[`MBSwedenReferenceParser`](http://photopay.github.io/photopay-ios/Classes/MBSwedenReferenceParser.html) is used for extracting reference from the OCR result.


# <a name="detectorTemplating"></a> Scanning generic documents with Templating API

This section discusses the setting up of `MBDetectorRecognizer` for scanning templated documents. Please check `Templating-sample` sample app for source code examples.

Templated document is any document which is defined by its template. Template contains the information about how the document should be detected, i.e. found on the camera scene and information about which part of the document contains which useful information.

## Defining how document should be detected

Before performing OCR of the document, _BlinkID_ first needs to find its location on a camera scene. In order to perform detection, you need to define [MBDetector](#detectorConcept). 

You have to set concrete `MBDetector` when instantiating the `MBDetectorRecognizer` as a parameter to its constructor.

You can find out more information about detectors that can be used in section [List of available detectors](#detectorList). The most commonly used detector is [`MBDocumentDetector`](#documentDetector).

## Defining how fields of interest should be extracted

`MBDetector` produces its result which contains document location. After the document has been detected, all further processing is done on the detected part of the input image.

There may be one or more variants of the same document type, for example for some document there may be old and new version and both of them must be supported. Because of that, for implementing support for each document, one or multiple templating classes are used. `MBTemplatingClass` is described in [The Templating Class component](#templatingClass) section.

`MBTemplatingClass` holds all needed information and components for processing its class of documents. Templating classes are processed in chain, one by one. On first class for which the data is successfully extracted, the chain is terminated and recognition results are returned. For each input image processing is done in the following way:

1. Classification `MBProcessorGroups` are run on the defined locations to extract data. `MBProcessorGroup` is used to define the location of interest on the detected document and `MBProcessors` that will extract data from that location. You can find more about `MBProcessorGroup` in the [next section](#processorGroup).

2. `MBTemplatingClassifier` is run with the data extracted by the classification processor groups to decide whether the currently scanned document belongs to the current class or not. Its [classify](http://photopay.github.io/photopay-ios/Protocols/MBTemplatingClassifier.html) method  simply returns `YES/true` or `NO/false`. If the classifier returns `NO/false`, recognition is moved to the next class in the chain, if it exists. You can find more about `MBTemplatingClassifier` in [this](#implementingTemplatingClassifier) section.

3. If the `MBTemplatingClassifier` has decided that currently scanned document belongs to the current class, non-classification `MBProcessorGroups` are run to extract other fields of interest.

### <a name="processorGroup"></a> The `MBProcessorGroup` component

In templating API [`MBProcessorGroup`](http://photopay.github.io/photopay-ios/Classes/MBProcessorGroup.html) is used to define the location of the field of interest on the detected document and how that location should be processed by setting following parameters in its constructor:

1. Location coordinates relative to document detection which are passed as [`Rectangle`] object.

2. `MBDewarpPolicy` which determines the resulting image chunk for processing. You can find a description of each `MBDewarpPolicy`, its purpose and recommendations when it should be used to get the best results in [List of available dewarp policies](#dewarpPolicyList) section.

3. Collection of processors that will be executed on the prepared chunk of the image for current document location. You can find more information about processors in [The Processor concept](#processorConcept) section.

### <a name="dewarpPolicyList"></a> List of available dewarp policies

Concrete `MBDewarpPolicy` defines how specific location of interest should be dewarped (cropped and rotated). It determines the height and width of the resulting dewarped image in pixels. Here is the list of available dewarp policies with linked doc for more information:

- [`MBFixedDewarpPolicy`](http://photopay.github.io/photopay-ios/Classes/MBFixedDewarpPolicy.html)
    - defines the exact height of the dewarped image in pixels
    - **usually the best policy for processor groups that use a legacy OCR engine**

- [`MBDPIBasedDewarpPolicy`](http://photopay.github.io/photopay-ios/Classes/MBDPIBasedDewarpPolicy.html):
    - defines the desired DPI (*Dots Per Inch*)
    - the height of the dewarped image will be calculated based on the actual physical size of the document provided by the used detector and chosen DPI
    - **usually the best policy for processor groups that prepare location's raw image for output**
 
- [`MBNoUpScalingDewarpPolicy`](http://photopay.github.io/photopay-ios/Classes/MBNoUpScalingDewarpPolicy.html): 
    - defines the maximal allowed height of the dewarped image in pixels
    - the height of the dewarped image will be calculated in a way that no part of the image will be up-scaled
    - if the height of the resulting image is larger than maximal allowed, then the maximal allowed height will be used as actual height, which effectively scales down the image
    - **usually the best policy for processors that use neural networks, for example,  DEEP OCR, hologram detection or NN-based classification**

### <a name="templatingClass"></a> The `MBTemplatingClass` component

[`MBTemplatingClass`](http://photopay.github.io/photopay-ios/Classes/MBTemplatingClass.html) enables implementing support for a specific class of documents that should be scanned with templating API. Final implementation of the templating recognizer consists of one or more templating classes, one class for each version of the document.

`MBTemplatingClass` contains two collections of `MBProcessorGroups` and a `MBTemplatingClassifier`.

The two collections of processor groups within `MBTemplatingClass` are:

1. The classification processor groups which are set by using the [`- (void)setClassificationProcessorGroups:(nonnull NSArray<__kindof MBProcessorGroup *> *)processorGroups`] method. `MBProcessorGroups` from this collection will be executed before classification, which means that they are always executed when processing comes to this class.

2. The non-classification processor groups which are set by using the [`- (void)setNonClassificationProcessorGroups:(nonnull NSArray<__kindof MBProcessorGroup *> *)processorGroups`]method. `MBProcessorGroups` from this collection will be executed after classification if the classification has been positive.

A component which decides whether the scanned document belongs to the current class is [`MBTemplatingClass`](http://photopay.github.io/photopay-ios/Classes/MBTemplatingClass.html). It can be set by using the `- (void)setTemplatingClassifier:(nullable id<MBTemplatingClassifier>)templatingClassifier` method. If it is not set, non-classification processor groups will not be executed. Instructions for implementing the `MBTemplatingClassifier` are given in the [next section](#implementingTemplatingClassifier).

### <a name="implementingTemplatingClassifier"></a> Implementing the `MBTemplatingClassifier`

Each concrete templating classifier implements the [`MBTemplatingClassifier`](http://photopay.github.io/photopay-ios/Protocols/MBTemplatingClassifier.html) interface, which requires to implement its `classify` method that is invoked while evaluating associated `MBTemplatingClass`.

Classification decision should be made based on the processing result which is returned by one or more processing units contained in the collection of the classification processor groups. As described in [The ProcessorGroup component](#processorGroup) section, each processor group contains one or more `MBProcessors`. [There are different `MBProcessors`](#processorList) which may enclose smaller processing units, for example, [`MBParserGroupProcessor`](#parserGroupProcessor) maintains the group of [`MBParsers`](#parserConcept). Result from each of the processing units in that hierarchy can be used for classification. In most cases `MBParser` result is used to determine whether some data in the expected format exists on the specified location.

To be able to retrieve results from the various processing units that are needed for classification, their instances must be available when `classify` method is called.

## Obtaining recognition results

When recognition is done, results can be obtained through processing units instances, such as: `MBProcessors`, `MBParsers`, etc. which are used for configuring the `MBTemplatingRecognizer` and later for processing the input image.

# <a name="detectorConcept"></a> The `MBDetector` concept

[`MBDetector`](http://photopay.github.io/photopay-ios/Classes/MBDetector.html) is a processing unit used within some `MBRecognizer` which supports detectors, such as [`MBDetectorRecognizer`](#detectorRecognizer). Concrete `MBDetector` knows how to find the certain object on the input image. `MBRecognizer` can use it to perform object detection prior to performing further recognition of detected object's contents.

`MBDetector` architecture is similar to `MBRecognizer` architecture described in [The Recognizer concept](#recognizerConcept) section. Each instance also has associated inner `MBRecognizerResult` object whose lifetime is bound to the lifetime of its parent `MBDetector` object and it is updated while `MBDetector` works. If you need your `MBRecognizerResult` object to outlive its parent `MBDetector` object, you must make a copy of it by calling its `copy` method.

It also has its internal state and while it is in the *working state* during recognition process, it is not allowed to tweak `MBDetector` object's properties.

When detection is performed on the input image, each `MBDetector` in its associated `MBDetectorResult` object holds the following information:

- [`MBDetectionCode`](http://photopay.github.io/photopay-ios/Enums/MBDetectionCode.html) that indicates the type of the detection.

- [`MBDetectionStatus`](http://photopay.github.io/photopay-ios/Enums/MBDetectionStatus.html) that represents the status of the detection.

- each concrete detector returns additional information specific to the detector type


To support common use cases, there are several different `MBDetector` implementations available. They are listed in the next section.

## <a name="detectorList"></a> List of available detectors

### <a name="documentDetector"></a> Document Detector

[`MBDocumentDetector`](http://photopay.github.io/photopay-ios/Classes/MBDocumentDetector.html) is used to detect card documents, cheques, A4-sized documents, receipts and much more.

It accepts one or more `MBDocumentSpecifications`. [`MBDocumentSpecification`](http://photopay.github.io/photopay-ios/Classes/MBDocumentSpecification.html) represents a specification of the document that should be detected by using edge detection algorithm and predefined aspect ratio.

For the most commonly used document formats, there is a helper method  `+ (instancetype)createFromPreset:(MBDocumentSpecificationPreset)preset` which creates and initializes the document specification based on the given [`MBDocumentSpecificationPreset`](http://photopay.github.io/photopay-ios/Enums/MBDocumentSpecificationPreset.html).

For the list of all available configuration methods see [`MBDocumentDetector`](http://photopay.github.io/photopay-ios/Classes/MBDocumentDetector.html) doc, and for available result content see [`MBDocumentDetectorResult`](http://photopay.github.io/photopay-ios/Classes/MBDocumentDetectorResult.html) doc.


### <a name="mrtdDetector"></a> MRTD Detector

[`MBMrtdDetector`](http://photopay.github.io/photopay-ios/Classes/MBMrtdDetector.html) is used to perform detection of *Machine Readable Travel Documents (MRTD)*.

Method `- (void)setMrtdSpecifications:(NSArray<__kindof MBMrtdSpecification *> *)mrtdSpecifications` can be used to define which MRTD documents should be detectable. It accepts the array of `MBMrtdSpecification`. [`MBMrtdSpecification`](http://photopay.github.io/photopay-ios/Classes/MBMrtdSpecification.html) represents specification of MRTD that should be detected. It can be created from the [`MBMrtdSpecificationPreset`](http://photopay.github.io/photopay-ios/Enums/MBMrtdSpecificationPreset.html) by using `+ (instancetype)createFromPreset:(MBMrtdSpecificationPreset)preset` method.

If `MBMrtdSpecifications` are not set, all supported MRTD formats will be detectable.

For the list of all available configuration methods see [`MBMrtdDetector`](http://photopay.github.io/photopay-ios/Classes/MBMrtdDetector.html) doc, and for available result content see [`MBMrtdDetectorResult`](http://photopay.github.io/photopay-ios/Classes/MBMrtdDetectorResult.html) doc.

# <a name="troubleshoot"></a> Troubleshooting

## <a name="integrationTroubleshoot"></a> Integration problems

In case of problems with integration of the SDK, first make sure that you have tried integrating it into XCode by following [integration instructions](#quickStart).

If you have followed [XCode integration instructions](#quickStart) and are still having integration problems, please contact us at [help.microblink.com](http://help.microblink.com).

## <a name="sdkTroubleshoot"></a> SDK problems

In case of problems with using the SDK, you should do as follows:

### Licencing problems

If you are getting "invalid licence key" error or having other licence-related problems (e.g. some feature is not enabled that should be or there is a watermark on top of camera), first check the console. All licence-related problems are logged to error log so it is easy to determine what went wrong.

When you have determine what is the licence-relate problem or you simply do not understand the log, you should contact us [help.microblink.com](http://help.microblink.com). When contacting us, please make sure you provide following information:

* exact Bundle ID of your app (from your `info.plist` file)
* licence that is causing problems
* please stress out that you are reporting problem related to iOS version of PDF417.mobi SDK
* if unsure about the problem, you should also provide excerpt from console containing licence error

### Other problems

If you are having problems with scanning certain items, undesired behaviour on specific device(s), crashes inside PDF417.mobi SDK or anything unmentioned, please do as follows:
	
* Contact us at [help.microblink.com](http://help.microblink.com) describing your problem and provide following information:
	* log file obtained in previous step
	* high resolution scan/photo of the item that you are trying to scan
	* information about device that you are using
	* please stress out that you are reporting problem related to iOS version of PDF417.mobi SDK

## <a name="faq"></a> Frequently asked questions and known problems
Here is a list of frequently asked questions and solutions for them and also a list of known problems in the SDK and how to work around them.

#### <a name="featureNotSupportedByLicenseKey"></a> In demo everything worked, but after switching to production license I get `NSError` with `MBMicroblinkSDKRecognizerErrorDomain` and `MBRecognizerFailedToInitalize` code as soon as I construct specific [`MBRecognizer`](http://photopay.github.io/photopay-ios/docs/Classes/MBRecognizer.html) object

Each license key contains information about which features are allowed to use and which are not. This `NSError` indicates that your production license does not allow using of specific `MBRecognizer` object. You should contact [support](http://help.microblink.com) to check if provided licence is OK and that it really contains all features that you have purchased.

#### <a name="invalidLicenseKey"></a> I get `NSError` with `MBMicroblinkSDKRecognizerErrorDomain` and `MBRecognizerFailedToInitalize` code with trial license key

Whenever you construct any [`MBRecognizer`](http://photopay.github.io/photopay-ios/docs/Classes/MBRecognizer.html) object or, a check whether license allows using that object will be performed. If license is not set prior constructing that object, you will get `NSError` with `MBMicroblinkSDKRecognizerErrorDomain` and `MBRecognizerFailedToInitalize` code. We recommend setting license as early as possible in your app.

#### <a name="undefinedSymbols"></a> Undefined Symbols on Architecture armv7

Make sure you link your app with iconv and Accelerate frameworks as shown in [Quick start](#quickStart). 
If you are using Cocoapods, please be sure that you've installed `git-lfs` prior to installing pods. If you are still getting this error, go to project folder and execute command `git-lfs pull`.

#### <a name="statefulRecognizer"></a> In my `didFinish` callback I have the result inside my `MBRecognizer`, but when scanning activity finishes, the result is gone

This usually happens when using [`MBRecognizerRunnerViewController`](http://photopay.github.io/photopay-ios/docs/Classes/MBRecognizerRunnerViewController.html) and forgetting to pause the [`MBRecognizerRunnerViewController`](http://photopay.github.io/photopay-ios/docs/Classes/MBRecognizerRunnerViewController.html) in your `didFinish` callback. Then, as soon as `didFinish` happens, the result is mutated or reset by additional processing that `MBRecognizer` performs in the time between end of your `didFinish` callback and actual finishing of the scanning activity. For more information about statefulness of the `MBRecognizer` objects, check [this section](#recognizerConcept).

#### <a name="unsupportedArchitecture"></a> Unsupported architectures when submitting app to App Store

Microblink.framework is a dynamic framework which contains slices for all architectures - device and simulator. If you intend to extract .ipa file for ad hoc distribution, you'll need to preprocess the framework to remove simulator architectures. 

Ideal solution is to add a build phase after embed frameworks build phase, which strips unused slices from embedded frameworks.

Build step is based on the one provided here: http://ikennd.ac/blog/2015/02/stripping-unwanted-architectures-from-dynamic-libraries-in-xcode/

```shell
APP_PATH="${TARGET_BUILD_DIR}/${WRAPPER_NAME}"

# This script loops through the frameworks embedded in the application and
# removes unused architectures.
find "$APP_PATH" -name '*.framework' -type d | while read -r FRAMEWORK
do
FRAMEWORK_EXECUTABLE_NAME=$(defaults read "$FRAMEWORK/Info.plist" CFBundleExecutable)
FRAMEWORK_EXECUTABLE_PATH="$FRAMEWORK/$FRAMEWORK_EXECUTABLE_NAME"
echo "Executable is $FRAMEWORK_EXECUTABLE_PATH"

EXTRACTED_ARCHS=()

for ARCH in $ARCHS
do
echo "Extracting $ARCH from $FRAMEWORK_EXECUTABLE_NAME"
lipo -extract "$ARCH" "$FRAMEWORK_EXECUTABLE_PATH" -o "$FRAMEWORK_EXECUTABLE_PATH-$ARCH"
EXTRACTED_ARCHS+=("$FRAMEWORK_EXECUTABLE_PATH-$ARCH")
done

echo "Merging extracted architectures: ${ARCHS}"
lipo -o "$FRAMEWORK_EXECUTABLE_PATH-merged" -create "${EXTRACTED_ARCHS[@]}"
rm "${EXTRACTED_ARCHS[@]}"

echo "Replacing original executable with thinned version"
rm "$FRAMEWORK_EXECUTABLE_PATH"
mv "$FRAMEWORK_EXECUTABLE_PATH-merged" "$FRAMEWORK_EXECUTABLE_PATH"

done
```

# <a name="info"></a> Additional info

Complete API reference can be found [here](http://photopay.github.io/photopay-ios/docs/index.html). 

For any other questions, feel free to contact us at [help.microblink.com](http://help.microblink.com).
