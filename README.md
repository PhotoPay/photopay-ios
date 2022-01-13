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
# Table of contents

- [Requirements](#requirements)
- [Quick Start](#quick-start)
- [Advanced PhotoPay integration instructions](#advanced-integration)
	- [Built-in overlay view controllers and overlay subviews](#ui-customizations)
		- [Using `MBPBarcodeOverlayViewController`](#using-pdf417-overlay-viewcontroller)
		- [Using `MBPFieldByFieldOverlayViewController`](#using-fieldbyfield-overlay-viewcontroller)
		- [Using `MBPDocumentCaptureOverlayViewController`](#using-documentcapture-overlay-viewcontroller)
		- [Using `MBPBlinkCardOverlayViewController`](#using-blinkcard-overlay-viewcontroller)
		- [Using `MBPBlinkIdOverlayViewController`](#using-blinkid-overlay-viewcontroller)
		- [Using `MBPPhotopayOverlayViewController`](#using-photopay-overlay-viewcontroller)
		- [Custom overlay view controller](#using-custom-overlay-viewcontroller)
	- [Direct processing API](#direct-api-processing)
		- [Using Direct API for `NSString` recognition (parsing)](#direct-api-string-processing)
- [`MBPRecognizer` and available recognizers](#recognizer)
- [List of available recognizers](#available-recognizers)
	- [Frame Grabber Recognizer](#frame-grabber-recognizer)
	- [Success Frame Grabber Recognizer](#success-frame-grabber-recognizer)
	- [PDF417 recognizer](#pdf417-recognizer)
	- [Barcode recognizer](#barcode-recognizer)
	- [BlinkInput recognizer](#blinkinput-recognizer)
	- [Detector recognizer](#detector-recognizer)
	- [Document Capture recognizer](#document-capture-recognizer)
	- [BlinkCard recognizers](#blinkcard-recognizers)
		- [MBPBlinkCardRecognizer](#blink-card-recognizer)
		- [MBPLegacyBlinkCardRecognizer (deprecated)](#payment-card-recognizers)
		- [MBPLegacyBlinkCardEliteRecognizer (deprecated)](#elite-payment-card-recognizers)
	- [BlinkID recognizers](#blinkid-recognizers)
		- [Machine Readable Travel Document recognizer](#mrtd-recognizer)
		- [Passport recognizer](#passport-recognizer)
		- [Visa recognizer](#visa-recognizer)
		- [ID barcode recognizer](#id-barcode-recognizer)
		- [Document face recognizer](#document-face-recognizers)
		- [BlinkID Recognizer](#blink-id-recognizers)
		- [BlinkID Combined Recognizer](#blink-id-combined-recognizers)
	- [PhotoPay recognizers by countries](#photopay-recognizers)
		- [Austria](#photopay-austria)
		- [Belgium](#photopay-belgium)
		- [Croatia](#photopay-croatia)
		- [Czechia](#photopay-czechia)
		- [Germany](#photopay-germany)
		- [Hungary](#photopay-hungary)
		- [Kosovo](#photopay-kosovo)
		- [Netherlands](#photopay-netherlands)
		- [SEPA](#photopay-sepa)
		- [Serbia](#photopay-serbia)
		- [Slovakia](#photopay-slovakia)
		- [Slovenia](#photopay-slovenia)
		- [Switzerland](#photopay-switzerland)
		- [United Kingdom](#photopay-uk)
- [`MBPProcessor` and `MBPParser`](#processors-and-parsers)
	- [The `MBPProcessor` concept](#processor-concept)
		- [Image Return Processor](#image-processors)
		- [Parser Group Processor](#parser-group-processor)
	- [The `MBPParser` concept](#parser-concept)
		- [Amount Parser](#amount-parser)
		- [Date Parser](#date-parser)
		- [Email Parser](#email-parser)
		- [IBAN Parser](#iban-parser)
		- [License Plates Parser](#license-plate-parser)
		- [Raw Parser](#raw-parser)
		- [Regex Parser](#regex-parser)
		- [TopUp Parser](#topup-parser)
		- [VIN (*Vehicle Identification Number*) Parser](#vin-parser)
- [Scanning generic documents with Templating API](#templating-api)
	- [Defining how document should be detected](#defining-document-detection)
	- [Defining how fields of interest should be extracted](#defining-field-extraction)
		- [The `MBPProcessorGroup` component](#processor-group)
		- [List of available dewarp policies](#dewarp-policy-list)
		- [The `MBPTemplatingClass` component](#templating-class)
		- [Implementing the `MBPTemplatingClassifier`](#implementing-templating-classifier)
- [The `MBPDetector` concept](#detector-concept)
	- [List of available detectors](#detector-list)
		- [Document Detector](#document-detector)
		- [MRTD Detector](#mrtd-detector)
- [Localization](#localization)
- [Troubleshooting](#troubleshooting)
	- [Integration problems](#troubleshooting-integration-problems)
	- [SDK problems](#troubleshooting-sdk-problems)
		- [Licencing problems](#troubleshooting-licensing-problems)
		- [Other problems](#troubleshooting-other-problems)
	- [Frequently asked questions and known problems](#troubleshooting-faq)
- [Additional info](#info)


# <a name="requirements"></a> Requirements

SDK package contains PhotoPay framework and one or more sample apps which demonstrate framework integration. The framework can be deployed in **iOS 9.0 or later**.

SDK performs significantly better when the images obtained from the camera are focused. Because of that, the SDK can have lower performance on iPad 2 and iPod Touch 4th gen devices, which [don't have camera with autofocus](http://www.adweek.com/socialtimes/ipad-2-rear-camera-has-tap-for-auto-exposure-not-auto-focus/12536). 
# <a name="quick-start"></a> Quick Start

## Getting started with PhotoPay SDK

This Quick Start guide will get you up and performing OCR scanning as quickly as possible. All steps described in this guide are required for the integration.

This guide sets up basic Raw OCR parsing and price parsing at the same time. It closely follows the BlinkOCR-sample app. We highly recommend you try to run the sample app. The sample app should compile and run on your device, and in the iOS Simulator.

The source code of the sample app can be used as the reference during the integration.

### 1. Initial integration steps


-[Download](https://github.com/PhotoPay/photopay-ios/releases) latest release (Download .zip or .tar.gz file starting with PhotoPay. DO NOT download Source Code as GitHub does not fully support Git LFS)

OR

Clone this git repository:

- Since the libraries are stored on [Git Large File Storage](https://git-lfs.github.com), you need to install git-lfs by running these commands:
```shell
brew install git-lfs
git lfs install
```

- **Be sure to restart your console after installing Git LFS**

- To clone, run the following shell command:

```shell
git clone git@github.com:PhotoPay/photopay-ios.git
```

- Copy PhotoPay.xcframework to your project folder.

- In your Xcode project, open the Project navigator. Drag the PhotoPay.xcframework file to your project, ideally in the Frameworks group, together with other frameworks you're using. When asked, choose "Create groups", instead of the "Create folder references" option.

![Adding PhotoPay.xcframework to your project](https://user-images.githubusercontent.com/1635933/89505694-535a1680-d7ca-11ea-8c65-678f158acae9.png)

- Since PhotoPay.xcframework is a dynamic framework, you also need to add it to embedded binaries section in General settings of your target.

![Adding PhotoPay.xcframework to embedded binaries](https://user-images.githubusercontent.com/1635933/89793425-238e7400-db26-11ea-9556-6eedeb6dcc95.png)

- Include the additional frameworks and libraries into your project in the "Linked frameworks and libraries" section of your target settings.

    - libc++.tbd
    - libiconv.tbd
    - libz.tbd

![Adding Apple frameworks to your project](https://raw.githubusercontent.com/wiki/blinkocr/blinkocr-ios/Images/02%20-%20Add%20Libraries.png)

### 2. Referencing header file

In files in which you want to use scanning functionality place import directive.

Swift

```swift
import PhotoPay
```

Objective-C

```objective-c
#import <PhotoPay/PhotoPay.h>
```

### 3. Initiating the scanning process

To initiate the scanning process, first decide where in your app you want to add scanning functionality. Usually, users of the scanning library have a button which, when tapped, starts the scanning process. Initialization code is then placed in touch handler for that button. Here we're listing the initialization code as it looks in a touch handler method.

Also, for initialization purposes, the ViewController which initiates the scan have private properties for [`MBPRawParser`](http://photopay.github.io/photopay-ios/Classes/MBPRawParser.html), [`MBPParserGroupProcessor`](http://photopay.github.io/photopay-ios//Classes/MBPParserGroupProcessor.html) and [`MBPBlinkInputRecognizer`](http://photopay.github.io/photopay-ios//Classes/MBPBlinkInputRecognizer.html), so we know how to obtain result.

Swift

```swift
class ViewController: UIViewController, MBPDocumentOverlayViewControllerDelegate  {

    var rawParser: MBPRawParser?
    var parserGroupProcessor: MBPParserGroupProcessor?
    var blinkInputRecognizer: MBPBlinkInputRecognizer?

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    @IBAction func didTapScan(_ sender: AnyObject) {

        let settings = MBPDocumentOverlaySettings()
        rawParser = MBPRawParser()
        parserGroupProcessor = MBPParserGroupProcessor(parsers: [rawParser!])
        blinkInputRecognizer = MBPBlinkInputRecognizer(processors: [parserGroupProcessor!])

        let recognizerList = [self.blinkInputRecognizer!]
        let recognizerCollection = MBPRecognizerCollection(recognizers: recognizerList)

        /** Create your overlay view controller */
        let documentOverlayViewController = MBPDocumentOverlayViewController(settings: settings, recognizerCollection: recognizerCollection, delegate: self)

        /** Create recognizer view controller with wanted overlay view controller */
        let recognizerRunnerViewController: UIViewController = MBPViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: documentOverlayViewController)

        /** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
        present(recognizerRunnerViewController!, animated: true, completion: nil)
    }
}
```

Objective-C

```objective-c
@interface ViewController () <MBPDocumentOverlayViewControllerDelegate>

@property (nonatomic, strong) MBPRawParser *rawParser;
@property (nonatomic, strong) MBPParserGroupProcessor *parserGroupProcessor;
@property (nonatomic, strong) MBPBlinkInputRecognizer *blinkInputRecognizer;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
}


- (IBAction)didTapScan:(id)sender {

    MBPDocumentOverlaySettings* settings = [[MBPDocumentOverlaySettings alloc] init];

    self.rawParser = [[MBPRawParser alloc] init];
    self.parserGroupProcessor = [[MBPParserGroupProcessor alloc] initWithParsers:@[self.rawParser]];
    self.blinkInputRecognizer = [[MBPBlinkInputRecognizer alloc] initWithProcessors:@[self.parserGroupProcessor]];

    /** Create recognizer collection */
    MBPRecognizerCollection *recognizerCollection = [[MBPRecognizerCollection alloc] initWithRecognizers:@[self.blinkInputRecognizer]];

    MBPDocumentOverlayViewController *overlayVC = [[MBPDocumentOverlayViewController alloc] initWithSettings:settings recognizerCollection:recognizerCollection delegate:self];
    UIViewController<MBPRecognizerRunnerViewController>* recognizerRunnerViewController = [MBPViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];

    /** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
    [self presentViewController:recognizerRunnerViewController animated:YES completion:nil];

}

@end
```

### 4. License key

A valid license key is required to initalize scanning. You can generate a free trial license key, after you register, at [Microblink developer dashboard](https://microblink.com/login).

You can include the license key in your app by passing a string or a file with license key.
**Note** that you need to set the license key before intializing scanning. Ideally in `AppDelegate` or `viewDidLoad` before initializing any recognizers.

#### License key as string
You can pass the license key as a string, the following way:

Swift

```swift
MBPMicroblinkSDK.shared().setLicenseKey("LICENSE-KEY", errorCallback: block)
```

Objective-C

```objective-c
[[MBPMicroblinkSDK sharedInstance] setLicenseKey:@"LICENSE-KEY" errorCallback:block];
```

#### License key as file
Or you can include the license key, with the code below. Please make sure that the file that contains the license key is included in your project and is copied during **Copy Bundle Resources** build phase.

Swift

```swift
MBPMicroblinkSDK.shared().setLicenseResource("license-key-file", withExtension: "txt", inSubdirectory: "directory-to-license-key", for: Bundle.main, errorCallback: block)
```

Objective-C

```objective-c
[[MBPMicroblinkSDK sharedInstance] setLicenseResource:@"license-key-file" withExtension:@"txt" inSubdirectory:@"" forBundle:[NSBundle mainBundle] errorCallback: block];
```

If the licence is invalid or expired then the methods above will throw an **exception**.

### 5. Registering for scanning events

In the previous step, you instantiated [`MBPDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPDocumentOverlayViewController.html) object with a delegate object. This object gets notified on certain events in scanning lifecycle. In this example we set it to `self`. The protocol which the delegate has to implement is [`MBPDocumentOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios//Protocols/MBPDocumentOverlayViewControllerDelegate.html) protocol. It is necessary to conform to that protocol. We will discuss more about protocols in [Advanced integration section](#advanced-integration). You can use the following default implementation of the protocol to get you started.

Swift

```swift
func documentOverlayViewControllerDidFinishScanning(_ documentOverlayViewController: MBPDocumentOverlayViewController, state: MBPRecognizerResultState) {

    // this is done on background thread
    // check for valid state
    if state == .valid {

        // first, pause scanning until we process all the results
        documentOverlayViewController.recognizerRunnerViewController?.pauseScanning()

        DispatchQueue.main.async(execute: {() -> Void in
            // All UI interaction needs to be done on main thread
        })
    }
}

func documentOverlayViewControllerDidTapClose(_ documentOverlayViewController: MBPDocumentOverlayViewController) {
    // Your action on cancel
}
```

Objective-C

```objective-c
- (void)documentOverlayViewControllerDidFinishScanning:(MBPDocumentOverlayViewController *)documentOverlayViewController state:(MBPRecognizerResultState)state {

    // this is done on background thread
    // check for valid state
    if (state == MBPRecognizerResultStateValid) {

        // first, pause scanning until we process all the results
        [documentOverlayViewController.recognizerRunnerViewController pauseScanning];

        dispatch_async(dispatch_get_main_queue(), ^{
            // All UI interaction needs to be done on main thread
        });
    }
}

- (void)documentOverlayViewControllerDidTapClose:(MBPDocumentOverlayViewController *)documentOverlayViewController {
    // Your action on cancel
}
```

# <a name="advanced-integration"></a> Advanced PhotoPay integration instructions
This section covers more advanced details of PhotoPay integration.

1. [First part](#ui-customizations) will cover the possible customizations when using UI provided by the SDK.
2. [Second part](#using-document-overlay-viewcontroller) will describe how to embed [`MBPRecognizerRunnerViewController's delegates`](http://photopay.github.io/photopay-ios/Protocols.html) into your `UIViewController` with the goal of creating a custom UI for scanning, while still using camera management capabilites of the SDK.
3. [Third part](#direct-api-processing) will describe how to use the [`MBPRecognizerRunner`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerRunner.html) (Direct API) for recognition directly from `UIImage` without the need of camera or to recognize camera frames that are obtained by custom camera management.
4. [Fourth part](#recognizer) will describe recognizer concept and available recognizers.


## <a name="ui-customizations"></a> Built-in overlay view controllers and overlay subviews

Within PhotoPay SDK there are several built-in overlay view controllers and scanning subview overlays that you can use to perform scanning. 
### <a name="using-pdf417-overlay-viewcontroller"></a> Using `MBPBarcodeOverlayViewController`

[`MBPBarcodeOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPBarcodeOverlayViewController.html) is overlay view controller best suited for performing scanning of various barcodes. It has [`MBPBarcodeOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPBarcodeOverlayViewControllerDelegate.html) delegate which can be used out-of-the-box to perform scanning using the default UI. Here is an example how to use and initialize [`MBPBarcodeOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPBarcodeOverlayViewController.html):

Swift
```swift
/** Create your overlay view controller */
let barcodeOverlayViewController : MBPBarcodeOverlayViewController = MBPBarcodeOverlayViewController(settings: barcodeSettings, recognizerCollection: recognizerCollection, delegate: self)

/** Create recognizer view controller with wanted overlay view controller */
let recognizerRunneViewController : UIViewController = MBPViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: barcodeOverlayViewController)

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
self.present(recognizerRunneViewController, animated: true, completion: nil)
```

Objective-C
```objective-c
MBPBarcodeOverlayViewController *overlayVC = [[MBPBarcodeOverlayViewController alloc] initWithSettings:settings recognizerCollection: recognizerCollection delegate:self];
UIViewController<MBPRecognizerRunnerViewController>* recognizerRunnerViewController = [MBPViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
[self presentViewController:recognizerRunnerViewController animated:YES completion:nil];
```

As you can see, when initializing [`MBPBarcodeOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPBarcodeOverlayViewController.html), we are sending delegate property as `self`. To get results, we need to conform to [`MBPBarcodeOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPBarcodeOverlayViewControllerDelegate.html) protocol.
### <a name="using-fieldbyfield-overlay-viewcontroller"></a> Using `MBPFieldByFieldOverlayViewController`

[`MBPFieldByFieldOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPFieldByFieldOverlayViewController.html) is overlay view controller best suited for performing scanning of various payment slips and barcodes with field of view. It has [`MBPFieldByFieldOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPFieldByFieldOverlayViewControllerDelegate.html) delegate which can be used out-of-the-box to perform scanning using the default UI. Here is an example how to use and initialize [`MBPFieldByFieldOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPFieldByFieldOverlayViewController.html):

Swift
```swift
/** Create your overlay view controller */
let fieldByFieldOverlayViewController : MBPFieldByFieldOverlayViewController = MBPFieldByFieldOverlayViewController(settings: fieldByFieldOverlaySettings, recognizerCollection: recognizerCollection, delegate: self)

/** Create recognizer view controller with wanted overlay view controller */
let recognizerRunneViewController : UIViewController = MBPViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: fieldByFieldOverlayViewController)

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
self.present(recognizerRunneViewController, animated: true, completion: nil)
```

Objective-C
```objective-c
MBPFieldByFieldOverlayViewController *overlayVC = [[MBPFieldByFieldOverlayViewController alloc] initWithSettings:settings recognizerCollection: recognizerCollection delegate:self];
UIViewController<MBPRecognizerRunnerViewController>* recognizerRunnerViewController = [MBPViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
[self presentViewController:recognizerRunnerViewController animated:YES completion:nil];
```

As you can see, when initializing [`MBPFieldByFieldOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPFieldByFieldOverlayViewController.html), we are sending delegate property as `self`. To get results, we need to conform to [`MBPFieldByFieldOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPFieldByFieldOverlayViewControllerDelegate.html) protocol.


### <a name="using-documentcapture-overlay-viewcontroller"></a> Using `MBPDocumentCaptureOverlayViewController`

[`MBPDocumentCaptureOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPDocumentCaptureOverlayViewController.html) is overlay view controller best suited for performing captureing cropped document images. It has [`MBPDocumentCaptureOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPDocumentCaptureOverlayViewControllerDelegate.html) delegate which can be used out-of-the-box to perform scanning using the default UI. Here is an example how to use and initialize [`MBPDocumentCaptureOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPFieldByFieldOverlayViewController.html):

Swift
```swift
/** Create your overlay view controller */
let documentCaptureOverlayViewController : MBPDocumentCaptureOverlayViewController = MBPDocumentCaptureOverlayViewController(settings: settings, recognizer: documentCaptureRecognizer, delegate: self)

/** Create recognizer view controller with wanted overlay view controller */
let recognizerRunneViewController : UIViewController = MBPViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: documentCaptureOverlayViewController)

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
self.present(recognizerRunneViewController, animated: true, completion: nil)
```

Objective-C
```objective-c
MBPDocumentCaptureOverlayViewController *overlayVC = [[MBPDocumentCaptureOverlayViewController alloc] initWithSettings:settings recognizer: documentCaptureRecognizer delegate:self];
UIViewController<MBPRecognizerRunnerViewController>* recognizerRunnerViewController = [MBPViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
[self presentViewController:recognizerRunnerViewController animated:YES completion:nil];
```

As you can see, when initializing [`MBPDocumentCaptureOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPDocumentCaptureOverlayViewController.html), we are sending delegate property as `self`. To get results, we need to conform to [`MBPDocumentCaptureOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPDocumentCaptureOverlayViewControllerDelegate.html) protocol.
### <a name="using-blinkcard-overlay-viewcontroller"></a> Using `MBPBlinkCardOverlayViewController`

[`MBPBlinkCardOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkCardOverlayViewController.html) is overlay view controller best suited for performing scanning of payment cards for both front and back side. It has [`MBPBlinkCardOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPBlinkCardOverlayViewControllerDelegate.html) delegate which can be used out-of-the-box to perform scanning using the default UI. Here is an example how to use and initialize [`MBPBlinkCardOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkCardOverlayViewController.html):

Swift

```swift
/** Create your overlay view controller */
let blinkCardViewController : MBPBlinkCardOverlayViewController = MBPBlinkCardOverlayViewController(settings: blinkCardSettings, recognizerCollection: recognizerCollection, delegate: self)

/** Create recognizer view controller with wanted overlay view controller */
let recognizerRunneViewController : UIViewController = MBPViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: blinkCardViewController)

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
self.present(recognizerRunneViewController, animated: true, completion: nil)
```

Objective-C

```objective-c
MBPDocumentVerificationOverlayViewController *overlayVC = [[MBPBlinkCardOverlayViewController alloc] initWithSettings:settings recognizerCollection: recognizerCollection delegate:self];
UIViewController<MBPRecognizerRunnerViewController>* recognizerRunnerViewController = [MBPViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
[self presentViewController:recognizerRunnerViewController animated:YES completion:nil];
```

As you can see, when initializing [`MBPDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPDocumentVerificationOverlayViewController.html), we are sending delegate property as `self`. To get results, we need to conform to [`MBPDocumentVerificationOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPDocumentVerificationOverlayViewControllerDelegate.html) protocol.

### Edit results screen

SDK also provides an overlay view controller that allows users to edit scanned results and input data that wasn't scanned. Note that this view controller works only with `MBPBlinkCardRecognizer`.

Enable edit screen by setting property `enableEditScreen = YES/true` on `MBPBlinkCardOverlaySettings`. It is enabled by default.

If edit screen is enabled, you must implement `blinkCardOverlayViewControllerDidFinishEditing` delegate method from `MBPBlinkCardOverlayViewControllerDelegate` protocol to get edited results. It returns `MBPBlinkCardOverlayViewController` and `MBPBlinkCardEditResult` object. You can still get original results and images from `MBPBlinkCardRecognizerResult`.

Edit results view controller can be customised in several ways:

- to configure which fields should be displayed use `fieldConfiguration` property of type [`MBPBlinkCardEditFieldConfiguration`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkCardEditFieldConfiguration.html)
- set your custom theme with [`MBPBlinkCardEditOverlayTheme`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkCardEditOverlayTheme.html)
- for setting custom strings, please check out our [Localization guide](#localization)

#### Edit results screen in Custom UI

SDK also provides options to use `MBPBlinkCardEditViewController` with Custom UI. Initalize it and, add it to `{ class_prefix }}BlinkCardEditNavigationController` and present it.

```swift
let blinkCardEditViewController = MBPBlinkCardEditViewController(delegate: self)
let navigationController = MBPBlinkCardEditNavigationController(rootViewController: blinkCardEditViewController)
```

```objective-c
self.blinkCardEditViewController = [[MBPBlinkCardEditViewController alloc] initWithDelegate:self];
self.navigationController = [[MBPBlinkCardEditNavigationController alloc] initWithRootViewController:self.blinkCardEditViewController];
```
### <a name="using-blinkid-overlay-viewcontroller"></a> Using `MBPBlinkIdOverlayViewController`

[`MBPBlinkIdOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkIdOverlayViewController.html) implements new UI for scanning identity documents, which is optimally designed to be used with new [`MBPBlinkIdRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkIdRecognizer.html) and [`MBPBlinkIdCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkIdCombinedRecognizer.html). The new [`MBPBlinkIdOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkIdOverlayViewController.html) implements several new features:
* clear indication for searching phase, when BlinkID is searching for an ID document
* clear progress indication, when BlinkID is busy with OCR and data extraction
* clear message when the document is not supported
* visual indications when the user needs to place the document closer to the camera
* when [`MBPBlinkIdCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkIdCombinedRecognizer.html) is used, visual indication that the data from the front side of the document doesn't match the data on the back side of the document.

The new UI allows the user to scan the document at an any angle, in any orientation. We recommend forcing landscape orientation if you scan barcodes on the back side, because in that orientation success rate will be higher.
To force the UI in landscape mode, use the following instructions:

Swift
```swift
let settings = MBPBlinkIdOverlaySettings()
settings.autorotateOverlay = true
settings.supportedOrientations = UIInterfaceOrientationMask.landscape
```

Objective-C
```objective-c
MBPBlinkIdOverlaySettings *settings = [[MBPBlinkIdOverlaySettings alloc] init];
settings.autorotateOverlay = YES;
settings.supportedOrientations = UIInterfaceOrientationMaskLandscape;
```

It has [`MBPBlinkIdOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPBlinkIdOverlayViewControllerDelegate.html) delegate which can be used out-of-the-box to perform scanning using the default UI. Here is an example how to use and initialize [`MBPBlinkIdOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkIdOverlayViewController.html):

Swift
```swift
/** Create your overlay view controller */
let blinkIdOverlayViewController : MBPBlinkIdOverlayViewController = MBPBlinkIdOverlayViewController(settings: blinkIdSettings, recognizerCollection: recognizerCollection, delegate: self)

/** Create recognizer view controller with wanted overlay view controller */
let recognizerRunneViewController : UIViewController = MBPViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: blinkIdOverlayViewController)

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
self.present(recognizerRunneViewController, animated: true, completion: nil)
```

Objective-C
```objective-c
MBPBlinkIdOverlayViewController *overlayVC = [[MBPBlinkIdOverlayViewController alloc] initWithSettings:settings recognizerCollection: recognizerCollection delegate:self];
UIViewController<MBPRecognizerRunnerViewController>* recognizerRunnerViewController = [MBPViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
[self presentViewController:recognizerRunnerViewController animated:YES completion:nil];
```

As you can see, when initializing [`MBPBlinkIdOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkIdOverlayViewController.html), we are sending delegate property as `self`. To get results, we need to conform to [`MBPBlinkIdOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPBlinkIdOverlayViewControllerDelegate.html) protocol.

### Customizing the look

The SDK comes with the ability to customize some aspects of the UI by using the UI theming. The screens can be customized to fit your app’s look and feel by defining themes in your application that override themes from the SDK. Each theme must extend the corresponding base theme from the SDK, as described in the following sections.

#### BlinkID Overlay Theme

![BlinkIDOverlayTheme](https://user-images.githubusercontent.com/26868155/101787592-8c4f2280-3aff-11eb-8ecf-41b49e83a163.png)

![BlinkIDOverlayThemeSuccess](https://user-images.githubusercontent.com/26868155/101787959-f1a31380-3aff-11eb-9d5c-05e46e347159.png)

To customize `MBPBlinkIdOverlayViewController`, use `MBPBlinkIdOverlayTheme` class to customize your look. You can customise elements labeled on the screenshot above by providing wanted properties to `MBPBlinkIdOverlayTheme`:

- **reticle**
	- reticleErrorColor - change custom error UIColor

- **instructions**
	- instructionsFont - set custom UIFont
	- instructionsTextColor - set custom UIColor
	- instructionsCornerRadius - set custom corner radius

- **flashlightWarning**
	- flashlightWarningFont - set custom UIFont
	- flashlightWarningBackgroundColor - set custom background UIColor
	- flashlightWarningTextColor - set custom text UIColor
	- flashlightWarningCornerRadius - set custom corner radius

- **cardIcon**
	- frontCardImage - change front card image on flip
	- backCardImage - change back card image on flip

- **successIcon**
	- successScanningImage - change success scan image
	- successFlashColor - change flash color on success scanning
### <a name="using-photopay-overlay-viewcontroller"></a> Using `MBPPhotopayOverlayViewController`

[`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html) is overlay view controller best suited for performing scanning of various payment slips and barcodes. It has [`MBPPhotopayOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPPhotopayOverlayViewControllerDelegate.html) delegate which can be used out-of-the-box to perform scanning using the default UI. Here is an example how to use and initialize [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html):

Swift
```swift
/** Create your overlay view controller */
let photopayOverlayViewController : MBPPhotopayOverlayViewController = MBPPhotopayOverlayViewController(settings: photopayOverlaySttings, recognizerCollection: recognizerCollection, delegate: self)

/** Create recognizer view controller with wanted overlay view controller */
let recognizerRunneViewController : UIViewController = MBPViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: photopayOverlayViewController)

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
self.present(recognizerRunneViewController, animated: true, completion: nil)
```

Objective-C
```objective-c
MBPPhotopayOverlayViewController *overlayVC = [[MBPPhotopayOverlayViewController alloc] initWithSettings:settings recognizerCollection: recognizerCollection delegate:self];
UIViewController<MBPRecognizerRunnerViewController>* recognizerRunnerViewController = [MBPViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];

/** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
[self presentViewController:recognizerRunnerViewController animated:YES completion:nil];
```

As you can see, when initializing [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), we are sending delegate property as `self`. To get results, we need to conform to [`MBPPhotopayOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPPhotopayOverlayViewControllerDelegate.html) protocol.
### <a name="using-custom-overlay-viewcontroller"></a> Custom overlay view controller

Please check our Samples for custom implementation of overlay view controller.

Overlay View Controller is an abstract class for all overlay views.

Its responsibility is to provide meaningful and useful interface for the user to interact with.

Typical actions which need to be allowed to the user are:

- intuitive and meaniningful way to guide the user through scanning process. This is usually done by presenting a "viewfinder" in which the user need to place the scanned object
- a way to cancel the scanning, typically with a "cancel" or "back" button
- a way to power on and off the light (i.e. "torch") button

PhotoPay SDK always provides it's own default implementation of the Overlay View Controller for every specific use. Your implementation should closely mimic the default implementation as it's the result of thorough testing with end users. Also, it closely matches the underlying scanning technology.

For example, the scanning technology usually gives results very fast after the user places the device's camera in the expected way above the scanned object. This means a progress bar for the scan is not particularly useful to the user. The majority of time the user spends on positioning the device's camera correctly. That's just an example which demonstrates careful decision making behind default camera overlay view.

### 1. Subclassing

To use your custom overlay with Microblink's camera view, you must first subclass [`MBPCustomOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPCustomOverlayViewController.html) and implement the overlay behaviour conforming wanted protocols.

### 2. Protocols

There are five [`MBPRecognizerRunnerViewController`](http://photopay.github.io/photopay-ios/Protocols/MBPRecognizerRunnerViewController.html) protocols and one overlay protocol [`MBPPhotoPayOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPPhotoPayOverlayViewControllerDelegate.html).

Five `RecognizerRunnerViewController` protocols are:
- [`MBPScanningRecognizerRunnerViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPScanningRecognizerRunnerViewControllerDelegate.html)
- [`MBPDetectionRecognizerRunnerViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPDetectionRecognizerRunnerViewControllerDelegate.html)
- [`MBPOcrRecognizerRunnerViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPOcrRecognizerRunnerViewControllerDelegate.html)
- [`MBPDebugRecognizerRunnerViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPDebugRecognizerRunnerViewControllerDelegate.html)
- [`MBPRecognizerRunnerViewControllerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPRecognizerRunnerViewControllerDelegate.html)

In `viewDidLoad`, other protocol conformation can be done and it's done on `recognizerRunnerViewController` property of [`MBPOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPOverlayViewController.html), for example:

Swift and Objective-C
```swift
self.scanningRecognizerRunnerViewControllerDelegate = self;
```

### 3. Initialization
In [Quick Start](#quick-start) guide it is shown how to use a default overlay view controller. You can now swap default view controller with your implementation of `CustomOverlayViewController`

Swift
```swift
let recognizerRunnerViewController : UIViewController = MBPViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: CustomOverlayViewController)
```

Objective-C
```objective-c
UIViewController<MBPRecognizerRunnerViewController>* recognizerRunnerViewController = [MBPViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:CustomOverlayViewController];
```

## <a name="direct-api-processing"></a> Direct processing API

This guide will in short present you how to process UIImage objects with PhotoPay SDK, without starting the camera video capture.

With this feature you can solve various use cases like:
	- recognizing text on images in Camera roll
	- taking full resolution photo and sending it to processing
	- scanning barcodes on images in e-mail etc.

DirectAPI-sample demo app here will present UIImagePickerController for taking full resolution photos, and then process it with PhotoPay SDK to get scanning results using Direct processing API.

Direct processing API is handled with [`MBPRecognizerRunner`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerRunner.html). That is a class that handles processing of images. It also has protocols as [`MBPRecognizerRunnerViewController`](http://photopay.github.io/photopay-ios/Protocols/MBPRecognizerRunnerViewController.html).
Developer can choose which protocol to conform:

- [`MBPScanningRecognizerRunnerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPScanningRecognizerRunnerDelegate.html)
- [`MBPDetectionRecognizerRunnerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPDetectionRecognizerRunnerDelegate.html)
- [`MBPDebugRecognizerRunnerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPDebugRecognizerRunnerDelegate.html)
- [`MBPOcrRecognizerRunnerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPOcrRecognizerRunnerDelegate.html)

In example, we are conforming to [`MBPScanningRecognizerRunnerDelegate`](http://photopay.github.io/photopay-ios/Protocols/MBPScanningRecognizerRunnerDelegate.html) protocol.

To initiate the scanning process, first decide where in your app you want to add scanning functionality. Usually, users of the scanning library have a button which, when tapped, starts the scanning process. Initialization code is then placed in touch handler for that button. Here we're listing the initialization code as it looks in a touch handler method.

Swift
```swift
func setupRecognizerRunner() {
    var recognizers = [MBPRecognizer]()
    pdf417Recognizer = MBPPdf417Recognizer()
    recognizers.append(pdf417Recognizer!)
    let recognizerCollection = MBPRecognizerCollection(recognizers: recognizers)
    recognizerRunner = MBPRecognizerRunner(recognizerCollection: recognizerCollection)
    recognizerRunner?.scanningRecognizerRunnerDelegate = self
}

func processImageRunner(_ originalImage: UIImage) {
    var image: MBPImage? = nil
    if let anImage = originalImage {
        image = MBPImage(uiImage: anImage)
    }
    image?.cameraFrame = true
    image?.orientation = MBPProcessingOrientation.left
    let _serialQueue = DispatchQueue(label: "com.microblink.DirectAPI-sample-swift")
    _serialQueue.async(execute: {() -> Void in
        self.recognizerRunner?.processImage(image!)
    })
}

func recognizerRunner(_ recognizerRunner: MBPRecognizerRunner, didFinishScanningWith state: MBPRecognizerResultState) {
    if blinkInputRecognizer.result.resultState == MBPRecognizerResultStateValid {
        // Handle result
    }
}
```

Objective-C
```objective-c
- (void)setupRecognizerRunner {
    NSMutableArray<MBPRecognizer *> *recognizers = [[NSMutableArray alloc] init];

    self.pdf417Recognizer = [[MBPPdf417Recognizer alloc] init];

    [recognizers addObject: self.pdf417Recognizer];

    MBPRecognizerCollection *recognizerCollection = [[MBPRecognizerCollection alloc] initWithRecognizers:recognizers];

    self.recognizerRunner = [[MBPRecognizerRunner alloc] initWithRecognizerCollection:recognizerCollection];
    self.recognizerRunner.scanningRecognizerRunnerDelegate = self;
}

- (void)processImageRunner:(UIImage *)originalImage {
    MBPImage *image = [MBPImage imageWithUIImage:originalImage];
    image.cameraFrame = YES;
    image.orientation = MBPProcessingOrientationLeft;
    dispatch_queue_t _serialQueue = dispatch_queue_create("com.microblink.DirectAPI-sample", DISPATCH_QUEUE_SERIAL);
    dispatch_async(_serialQueue, ^{
        [self.recognizerRunner processImage:image];
    });
}

- (void)recognizerRunner:(nonnull MBPRecognizerRunner *)recognizerRunner didFinishScanningWithState:(MBPRecognizerResultState)state {
    if (self.blinkInputRecognizer.result.resultState == MBPRecognizerResultStateValid) {
        // Handle result
    }
}
```

Now you've seen how to implement the Direct processing API.

In essence, this API consists of two steps:

- Initialization of the scanner.
- Call of `- (void)processImage:(MBPImage *)image;` method for each UIImage or CMSampleBufferRef you have.


### <a name="direct-api-string-processing"></a> Using Direct API for `NSString` recognition (parsing)

Some recognizers support recognition from `NSString`. They can be used through Direct API to parse given `NSString` and return data just like when they are used on an input image. When recognition is performed on `NSString`, there is no need for the OCR. Input `NSString` is used in the same way as the OCR output is used when image is being recognized.
Recognition from `String` can be performed in the same way as recognition from image.
The only difference is that user should call `- (void)processString:(NSString *)string;` on [`MBPRecognizerRunner`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerRunner.html).

# <a name="recognizer"></a> `MBPRecognizer` and available recognizers

## The `MBPRecognizer` concept

The [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) is the basic unit of processing within the SDK. Its main purpose is to process the image and extract meaningful information from it. As you will see [later](#available-recognizers), the SDK has lots of different [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects that have various purposes.

Each [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) has a [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerResult.html) object, which contains the data that was extracted from the image. The [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerResult.html) object is a member of corresponding [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object its lifetime is bound to the lifetime of its parent [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object. If you need your `MBPRecognizerResult` object to outlive its parent [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object, you must make a copy of it by calling its method `copy`.

While [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object works, it changes its internal state and its result. The [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object's [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerResult.html) always starts in `Empty` state. When corresponding [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object performs the recognition of given image, its [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerResult.html) can either stay in `Empty` state (in case [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html)failed to perform recognition), move to `Uncertain` state (in case [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) performed the recognition, but not all mandatory information was extracted) or move to `Valid` state (in case [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) performed recognition and all mandatory information was successfully extracted from the image).

As soon as one [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object's [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerResult.html) within [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html) given to `MBPRecognizerRunner` or `MBPRecognizerRunnerViewController` changes to `Valid` state, the `onScanningFinished` callback will be invoked on same thread that performs the background processing and you will have the opportunity to inspect each of your [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects' [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerResult.html) to see which one has moved to `Valid` state.

As soon as `onScanningFinished` method ends, the `MBPRecognizerRunnerViewController` will continue processing new camera frames with same [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects, unless `paused`. Continuation of processing or `reset` recognition will modify or reset all [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects's [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerResult.html). When using built-in activities, as soon as `onScanningFinished` is invoked, built-in activity pauses the `MBPRecognizerRunnerViewController` and starts finishing the activity, while saving the [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html) with active [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html).

## `MBPRecognizerCollection` concept

The [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html) is is wrapper around [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects that has array of [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects that can be used to give [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects to `MBPRecognizerRunner` or `MBPRecognizerRunnerViewController` for processing.

The [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html) is always constructed with array `[[MBPRecognizerCollection alloc] initWithRecognizers:recognizers]` of [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects that need to be prepared for recognition (i.e. their properties must be tweaked already).

The [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html) manages a chain of [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects within the recognition process. When a new image arrives, it is processed by the first [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) in chain, then by the second and so on, iterating until a [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object's [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerResult.html) changes its state to `Valid` or all of the [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects in chain were invoked (none getting a `Valid` result state).

You cannot change the order of the [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects within the chain - no matter the order in which you give [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects to [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html), they are internally ordered in a way that provides best possible performance and accuracy. Also, in order for SDK to be able to order [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects in recognition chain in a best way possible, it is not allowed to have multiple instances of [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects of the same type within the chain. Attempting to do so will crash your application.

# <a name="available-recognizers"></a> List of available recognizers

This section will give a list of all [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects that are available within PhotoPay SDK, their purpose and recommendations how they should be used to get best performance and user experience.

## <a name="frame-grabber-recognizer"></a> Frame Grabber Recognizer

The [`MBPFrameGrabberRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPFrameGrabberRecognizer.html) is the simplest recognizer in SDK, as it does not perform any processing on the given image, instead it just returns that image back to its `onFrameAvailable`. Its result never changes state from empty.

This recognizer is best for easy capturing of camera frames with `MBPRecognizerRunnerViewController`. Note that [`MBPImage`](http://photopay.github.io/photopay-ios/Classes/MBPImage.html) sent to `onFrameAvailable` are temporary and their internal buffers all valid only until the `onFrameAvailable` method is executing - as soon as method ends, all internal buffers of [`MBPImage`](http://photopay.github.io/photopay-ios/Classes/MBPImage.html) object are disposed. If you need to store [`MBPImage`](http://photopay.github.io/photopay-ios/Classes/MBPImage.html) object for later use, you must create a copy of it by calling `copy`.

## <a name="success-frame-grabber-recognizer"></a> Success Frame Grabber Recognizer

The [`MBPSuccessFrameGrabberRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSuccessFrameGrabberRecognizer.html) is a special `MBPecognizer` that wraps some other [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) and impersonates it while processing the image. However, when the [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) being impersonated changes its [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerResult.html) into `Valid` state, the [`MBPSuccessFrameGrabberRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSuccessFrameGrabberRecognizer.html) captures the image and saves it into its own [`MBPSuccessFrameGrabberRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBPSuccessFrameGrabberRecognizerResult.html) object.

Since [`MBPSuccessFrameGrabberRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSuccessFrameGrabberRecognizer.html)  impersonates its slave [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object, it is not possible to give both concrete [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object and `MBPSuccessFrameGrabberRecognizer` that wraps it to same `MBPRecognizerCollection` - doing so will have the same result as if you have given two instances of same [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) type to the [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html) - it will crash your application.

This recognizer is best for use cases when you need to capture the exact image that was being processed by some other [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object at the time its [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerResult.html) became `Valid`. When that happens, `MBPSuccessFrameGrabberRecognizer's` `MBPSuccessFrameGrabberRecognizerResult` will also become `Valid` and will contain described image.

## <a name="pdf417-recognizer"></a> PDF417 recognizer

The [`MBPPdf417Recognizer`](http://photopay.github.io/photopay-ios/Classes/MBPPdf417Recognizer.html) is recognizer specialised for scanning [PDF417 2D barcodes](https://en.wikipedia.org/wiki/PDF417). This recognizer can recognize only PDF417 2D barcodes - for recognition of other barcodes, please refer to [BarcodeRecognizer](#barcode-recognizer).

This recognizer can be used in any overlay view controller, but it works best with the [`MBPBarcodeOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPBarcodeOverlayViewController.html), which has UI best suited for barcode scanning.

## <a name="barcode-recognizer"></a> Barcode recognizer

The [`MBPBarcodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPBarcodeRecognizer.html) is recognizer specialised for scanning various types of barcodes. This recognizer should be your first choice when scanning barcodes as it supports lots of barcode symbologies, including the [PDF417 2D barcodes](https://en.wikipedia.org/wiki/PDF417), thus making [PDF417 recognizer](#pdf417-recognizer) possibly redundant, which was kept only for its simplicity.

You can enable multiple barcode symbologies within this recognizer, however keep in mind that enabling more barcode symbologies affect scanning performance - the more barcode symbologies are enabled, the slower the overall recognition performance. Also, keep in mind that some simple barcode symbologies that lack proper redundancy, such as [Code 39](https://en.wikipedia.org/wiki/Code_39), can be recognized within more complex barcodes, especially 2D barcodes, like [PDF417](https://en.wikipedia.org/wiki/PDF417).

This recognizer can be used in any overlay view controller, but it works best with the [`MBPBarcodeOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPBarcodeOverlayViewController.html), which has UI best suited for barcode scanning.
## <a name="blinkinput-recognizer"></a> BlinkInput recognizer

The [`MBPBlinkInputRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkInputRecognizer.html) is generic OCR recognizer used for scanning segments which enables specifying `MBPProcessors` that will be used for scanning. Most commonly used `MBPProcessor` within this recognizer is [`MBPParserGroupProcessor`](http://photopay.github.io/photopay-ios/Classes/MBPParserGroupProcessor.html)) that activates all `MBPParsers` in the group to extract data of interest from the OCR result.

This recognizer can be used in any context. It is used internally in the implementation of the provided [`MBPFieldByFieldOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPFieldByFieldOverlayViewController.html).

`MBPProcessors` are explained in [The Processor concept](#processor-concept) section and you can find more about `MBPParsers` in [The Parser concept](#parser-concept) section.

## <a name="detector-recognizer"></a> Detector recognizer

The [`MBPDetectorRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPDetectorRecognizer.html) is recognizer for scanning generic documents using custom `MBPDetector`. You can find more about `Detector` in [The Detector concept](#detector-concept) section. `MBPDetectorRecognizer` can be used simply for document detection and obtaining its image. The more interesting use case is data extraction from the custom document type. `MBPDetectorRecognizer` performs document detection and can be configured to extract fields of interest from the scanned document by using **Templating API**. You can find more about Templating API in [this](#detector-templating) section.

## <a name="document-capture-recognizer"></a> Document Capture recognizer

The [`MBPDocumentCaptureRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPDocumentCaptureRecognizer.html) is used for taking cropped document images.
This recognizer can be used in any context, but it works best with the [`MBPDocumentCaptureOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPDocumentCaptureOverlayViewController.html) which takes high resolution document images and guides the user through the image capture process.
## <a name="blinkcard-recognizers"></a> BlinkCard recognizers
Payment card recognizers are used to scan payment cards.

### <a name="blink-card-recognizer"></a> MBPBlinkCardRecognizer
The MBPBlinkCardRecognizer extracts the card number (PAN), expiry date, owner information (name or company title), IBAN, and CVV, from a large range of different card layouts.

MBPBlinkCardRecognizer is a Combined recognizer, which means it's designed for scanning both sides of a card. However, if all required data is found on the first side, we do not wait for second side scanning. We can return the result early. A set of required fields is defined through the recognizer's settings.

"Front side" and "back side" are terms more suited to ID scanning. We start the scanning process with the side containing the card number. This makes the UX easier for users with cards where all data is on the back side.

### <a name="payment-card-recognizers"></a> MBPLegacyBlinkCardRecognizer (deprecated)
The [`MBPLegacyBlinkCardRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPLegacyBlinkCardRecognizer.html)is used for scanning the [front and back side of Payment / Debit card](https://en.wikipedia.org/wiki/Payment_card).

### <a name="elite-payment-card-recognizers"></a> MBPLegacyBlinkCardEliteRecognizer (deprecated)
The [`MBPLegacyBlinkCardEliteRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPLegacyBlinkCardEliteRecognizer.html) scans back side of elite Payment / Debit card after scanning the front side and combines data from both sides.
## <a name="blinkid-recognizers"></a> BlinkID recognizers

Unless stated otherwise for concrete recognizer, **single side BlinkID recognizes** from this list can be used in any context, but they work best with the [`MBPDocumentOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPDocumentOverlayViewController.html), which has UI best suited for document scanning.

**Combined recognizers** should be used with [`MBPDocumentVerificationOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPDocumentVerificationOverlayViewController.html) which manages scanning of multiple document sides in the single camera opening and guides the user through the scanning process. Some combined recognizers support scanning of multiple document types, but only one document type can be scanned at a time.

### <a name="mrtd-recognizer"></a> Machine Readable Travel Document recognizer
The [`MBPMrtdRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPMrtdRecognizer.html) is used for scanning and data extraction from the Machine Readable Zone (MRZ) of the various Machine Readable Travel Documents (MRTDs) like ID cards and passports. This recognizer is not bound to the specific country, but it can be configured to only return data that match some criteria defined by the [`MBPMrzFilter`](http://photopay.github.io/photopay-ios/Protocols/MBPMrzFilter.html).

You can find information about usage context at the beginning of [this section](#-blinkid-recognizers).

### Machine Readable Travel Document combined recognizer
The [`MBPMrtdCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPMrtdCombinedRecognizer.html) scans Machine Readable Zone (MRZ) after scanning the full document image and face image (usually MRZ is on the back side and face image is on the front side of the document). Internally, it uses [MBPDocumentFaceRecognizer](#document-face-recognizer) for obtaining full document image and face image as the first step and then [MBPMrtdRecognizer](#mrtd-recognizer) for scanning the MRZ.

You can find information about usage context at the beginning of [this section](#-blinkid-recognizers).

### <a name="passport-recognizer"></a> Passport recognizer
The [`MBPPassportRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPPassportRecognizer.html) is used for scanning and data extraction from the Machine Readable Zone (MRZ) of the various passport documents. This recognizer also returns face image from the passport.

You can find information about usage context at the beginning of [this section](#-blinkid-recognizers).

### <a name="visa-recognizer"></a> Visa recognizer
The [`MBPVisaRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPVisaRecognizer.html) is used for scanning and data extraction from the Machine Readable Zone (MRZ) of the various visa documents. This recognizer also returns face image from the visa document.

You can find information about usage context at the beginning of [this section](#-blinkid-recognizers).

### <a name="id-barcode-recognizer"></a> ID barcode recognizer
The [`MBPIdBarcodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPIdBarcodeRecognizer.html) is used for scanning barcodes from various ID cards. Check this document to see the list of supported document types.

You can find information about usage context at the beginning of [this section](#-blinkid-recognizers).

### <a name="document-face-recognizers"></a> Document face recognizer
The [`MBPDocumentFaceRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPDocumentFaceRecognizer.html) is a special type of recognizer that only returns face image and full document image of the scanned document. It does not extract document fields like first name, last name, etc. This generic recognizer can be used to obtain document images in cases when specific support for some document type is not available.

You can find information about usage context at the beginning of [this section](#-blinkid-recognizers).

### <a name="blink-id-recognizers"></a> BlinkID Recognizer
The [`MBPBlinkIdRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkIdRecognizer.html) scans and extracts data from the front side of the supported document.
You can find the list of the currently supported documents [`here`](https://github.com/PhotoPay/photopay-ios/tree/master/documentation/BlinkIDRecognizer.md).
We will continue expanding this recognizer by adding support for new document types in the future. Star this repo to stay updated.

### <a name="blink-id-combined-recognizers"></a> BlinkID Combined Recognizer
Use [`MBPBlinkIdCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkIdCombinedRecognizer.html) for scanning both sides of the supported document. First, it scans and extracts data from the front, then scans and extracts data from the barcode on the back, and finally, combines results from both sides. The [`BlinkIDCombinedRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPBlinkIdCombinedRecognizer.html) also performs data matching and returns a flag if the extracted data captured from the front side matches the data from the barcode on the back.
You can find the list of the currently supported documents [`here`](https://github.com/PhotoPay/photopay-ios/tree/master/documentation/BlinkIDRecognizer.md).
We will continue expanding this recognizer by adding support for new document types in the future. Star this repo to stay updated.

## <a name="photopay-recognizers"></a> PhotoPay recognizers by countries

### <a name="photopay-austria"></a> Austria

The [`MBPAustriaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPAustriaQrCodeRecognizer.html) is recognizer specialised for scanning Austrian payment QR codes, such as Stuzza codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBPAustriaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPAustriaSlipRecognizer.html) is recognizer specialised for scanning Austrian payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

### <a name="photopay-belgium"></a> Belgium

The [`MBPBelgiumSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPBelgiumSlipRecognizer.html) is recognizer specialised for scanning Belgian payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

### <a name="photopay-croatia"></a> Croatia

The [`MBPCroatiaPdf417Recognizer`](http://photopay.github.io/photopay-ios/Classes/MBPCroatiaPdf417Recognizer.html) is recognizer specialised for scanning Croatia payment Pdf417 codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBPCroatiaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPCroatiaQrCodeRecognizer.html) is recognizer specialised for scanning Croatia payment QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBPCroatiaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPCroatiaSlipRecognizer.html) is recognizer specialised for scanning Croatia payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for both side document scanning.

### <a name="photopay-czechia"></a> Czechia

The [`MBPCzechiaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPCzechiaQrCodeRecognizer.html) is recognizer specialised for scanning Czech payment QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBPCzechiaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPCzechiaSlipRecognizer.html) is recognizer specialised for scanning Czech payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

### <a name="photopay-germany"></a> Germany

The [`MBPGermanyQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPGermanyQrCodeRecognizer.html) is recognizer specialised for scanning German payment QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBPGermanySlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPGermanySlipRecognizer.html) is recognizer specialised for scanning German payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

### <a name="photopay-hungary"></a> Hungary

The [`MBPHungarySlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPHungarySlipRecognizer.html) is recognizer specialised for scanning Hungarian payment slips - white and yellow.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

### <a name="photopay-kosovo"></a> Kosovo

The [`MBPKosovoSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPKosovoSlipRecognizer.html) is recognizer specialised for scanning Kosovo payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPFieldOfViewOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPFieldOfViewOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBPKosovoCode128Recognizer`](http://photopay.github.io/photopay-ios/Classes/MBPKosovoCode128Recognizer.html) is recognizer specialised for scanning code 128 barcodes found on Kosovo payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPFieldOfViewOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPFieldOfViewOverlayViewController.html), which has UI best suited for one side document scanning.

### <a name="photopay-netherlands"></a> Netherlands

The [`MBPNetherlandsSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPNetherlandsSlipRecognizer.html) is recognizer specialised for scanning Dutch payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

### <a name="photopay-sepa"></a> SEPA

The [`MBPSepaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSepaQrCodeRecognizer.html) is recognizer specialised for scanning SEPA QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

### <a name="photopay-serbia"></a> Serbia

The [`MBPSerbiaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSerbiaQrCodeRecognizer.html) is recognizer specialised for scanning Serbian QR payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBPSerbiaPdf417Recognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSerbiaPdf417Recognizer.html) is recognizer specialised for scanning Serbian PDF 417 payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

### <a name="photopay-slovakia"></a> Slovakia

The [`MBPSlovakiaCode128Recognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSlovakiaCode128Recognizer.html) is recognizer specialised for scanning Slovakian CODE 128 payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBPSlovakiaDataMatrixRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSlovakiaDataMatrixRecognizer.html) is recognizer specialised for scanning Slovakian Data Matrix payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBPSlovakiaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSlovakiaQrCodeRecognizer.html) is recognizer specialised for scanning Slovakian QR payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for both side document scanning.

The [`MBPSlovakiaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSlovakiaSlipRecognizer.html) is recognizer specialised for scanning Slovakian payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for both side document scanning.

### <a name="photopay-slovenia"></a> Slovenia

The [`MBPSloveniaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSloveniaSlipRecognizer.html) is recognizer specialised for scanning Slovenian UPN payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

### <a name="photopay-switzerland"></a> Switzerland

The [`MBPSwitzerlandQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSwitzerlandQrCodeRecognizer.html) is recognizer specialised for scanning Swiss payment QR codes, such as Stuzza codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBPSwitzerlandSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSwitzerlandSlipRecognizer.html) is recognizer specialised for scanning Swiss payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

### <a name="photopay-uk"></a> United Kingdom

The [`MBPUnitedKingdomQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPUnitedKingdomQrCodeRecognizer.html) is recognizer specialised for scanning UK payment QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for one side document scanning.

The [`MBPUnitedKingdomSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPUnitedKingdomSlipRecognizer.html) is recognizer specialised for scanning UK payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for both side document scanning.
# <a name="processors-and-parsers"></a> `MBPProcessor` and `MBPParser`

The `MBPProcessors` and `MBPParsers` are standard processing units within *PhotoPay* SDK used for data extraction from the input images. Unlike the [`MBPRecognizer`](#recognizer-concept), `MBPProcessor` and `MBPParser` are not stand-alone processing units. `MBPProcessor` is always used within `MBPRecognizer` and `MBPParser` is used within appropriate `MBPProcessor` to extract data from the OCR result.

## <a name="processor-concept"></a> The `MBPProcessor` concept

`MBPProcessor` is a processing unit used within some `Recognizer` which supports processors. It process the input image prepared by the enclosing `Recognizer` in the way that is characteristic to the implementation of the concrete `MBPProcessor`.

`MBPProcessor` architecture is similar to `MBPRecognizer` architecture described in [The Recognizer concept](#recognizer-concept) section. Each instance also has associated inner `MBPRecognizerResult` object whose lifetime is bound to the lifetime of its parent `MBPProcessor` object and it is updated while `MBPProcessor` works. If you need your `MBPRecognizerResult` object to outlive its parent `MBPProcessor` object, you must make a copy of it by calling its method `copy`.

It also has its internal state and while it is in the *working state* during recognition process, it is not allowed to tweak `MBPProcessor` object's properties.

To support common use cases, there are several different `MBPProcessor` implementations available. They are listed in the next section.

##  <a name="available-processors"></a> List of available processors

This section will give a list of `MBPProcessor` types that are available within *PhotoPay* SDK and their purpose.

### <a name="image-processors"></a> Image Return Processor

The [`MBPImageReturnProcessor`](http://photopay.github.io/photopay-ios/Classes/MBPImageReturnProcessor.html) is used for obtaining input images. It simply saves the input image and makes it available after the scanning is done.

The appearance of the input image depends on the context in which `MBPImageReturnProcessor` is used. For example, when it is used within [`MBPBlinkInputRecognizer`](#blinkinput-recognizer), simply the raw image of the scanning region is processed. When it is used within the [`Templating API`](#detector-templating), input image is dewarped (cropped and rotated).

The image is returned as the raw [`MBPImage`](http://photopay.github.io/photopay-ios/Classes/MBPImage.html) type. Also, processor can be configured to [encode saved image to JPEG](http://photopay.github.io/photopay-ios/Classes/MBPImageReturnProcessor.html).

### <a name="parser-group-processor"></a> Parser Group Processor


The [`MBPParserGroupProcessor`](http://photopay.github.io/photopay-ios/Classes/MBPParserGroupProcessor.html) is the type of the processor that performs the OCR (*Optical Character Recognition*) on the input image and lets all the parsers within the group to extract data from the OCR result. The concept of `MBPParser` is described in [the next](#parser-concept) section.

Before performing the OCR, the best possible OCR engine options are calculated by combining engine options needed by each `MBPParser` from the group. For example, if one parser expects and produces result from uppercase characters and other parser extracts data from digits, both uppercase characters and digits must be added to the list of allowed characters that can appear in the OCR result. This is a simplified explanation because OCR engine options contain many parameters which are combined by the `MBPParserGroupProcessor`.

Because of that, if multiple parsers and multiple parser group processors are used during the scan, it is very important to group parsers carefully.

Let's see this on an example: assume that we have two parsers at our disposal: `MBPAmountParser` and `MBPEmailParser`. `MBPAmountParser` knows how to extract amount's from OCR result and requires from OCR only to recognize digits, periods and commas and ignore letters. On the other hand, `MBPEmailParser` knows how to extract e-mails from OCR result and requires from OCR to recognize letters, digits, '@' characters and periods, but not commas.

If we put both `MBPAmountParser` and `MBPEmailParser` into the same `MBPParserGroupProcessor`, the merged OCR engine settings will require recognition of all letters, all digits, '@' character, both period and comma. Such OCR result will contain all characters for `MBPEmailParser` to properly parse e-mail, but might confuse `MBPAmountParser` if OCR misclassifies some characters into digits.

If we put `MBPAmountParser` in one `MBPParserGroupProcessor` and `MBPEmailParser` in another `MBPParserGroupProcessor`, OCR will be performed for each parser group independently, thus preventing the `MBPAmountParser` confusion, but two OCR passes of the image will be performed, which can have a performance impact.

`MBPParserGroupProcessor` is most commonly used `MBPProcessor`. It is used whenever the OCR is needed. After the OCR is performed and all parsers are run, parsed results can be obtained through parser objects that are enclosed in the group. `MBPParserGroupProcessor` instance also has associated inner `MBPParserGroupProcessorResult` whose state is updated during processing and its property [`ocrLayout`](http://photopay.github.io/photopay-ios/Classes/MBPParserGroupProcessor.html) can be used to obtain the raw [`MBPOcrLayout`](http://photopay.github.io/photopay-ios/Classes/MBPOcrLayout.html) that was used for parsing data.

Take note that `MBPOcrLayout` is available only if it is allowed by the *PhotoPay* SDK license key. `MBPOcrLayout` structure contains information about all recognized characters and their positions on the image. To prevent someone to abuse that, obtaining of the `MBPOcrLayout` structure is allowed only by the premium license keys.

## <a name="parser-concept"></a> The `MBPParser` concept

`MBPParser` is a class of objects that are used to extract structured data from the raw OCR result. It must be used within `MBPParserGroupProcessor` which is responsible for performing the OCR, so `MBPParser` is not stand-alone processing unit.

Like [`MBPRecognizer`](#recognizer-concept) and all other processing units, each `MBPParser` instance has associated inner `MBPRecognizerResult` object whose lifetime is bound to the lifetime of its parent `MBPParser` object and it is updated while `MBPParser` works. When parsing is done `MBPParserResult` can be used for obtaining extracted data. If you need your `MBPParserResult` object to outlive its parent `MBPParser` object, you must make a copy of it by calling its method `copy`.

It also has its internal state and while it is in the *working state* during recognition process, it is not allowed to tweak `MBPParser` object's properties.

There are a lot of different `MBPParsers` for extracting most common fields which appear on various documents. Also, most of them can be adjusted for specific use cases. For all other custom data fields, there is `RegexParser` available which can be configured with the arbitrary regular expression.

##  <a name="available-parsers"></a> List of available parsers

### <a name="amount-parser"></a> Amount Parser

[`MBPAmountParser`](http://photopay.github.io/photopay-ios/Classes/MBPAmountParser.html) is used for extracting amounts from the OCR result.

### <a name="date-parser"></a> Date Parser

[`MBPDateParser`](http://photopay.github.io/photopay-ios/Classes/MBPDateParser.html) is used for extracting dates in various formats from the OCR result.

### <a name="email-parser"></a> Email Parser

[`MBPEmailParser`](http://photopay.github.io/photopay-ios/Classes/MBPEmailParser.html) is used for extracting e-mail addresses from the OCR result.

### <a name="iban-parser"></a> IBAN Parser

[`MBPIbanParser`](http://photopay.github.io/photopay-ios/Classes/MBPIbanParser.html) is used for extracting IBAN (*International Bank Account Number*) from the OCR result.

### <a name="license-plate-parser"></a> License Plates Parser

[`MBPLicensePlatesParser`](http://photopay.github.io/photopay-ios/Classes/MBPLicensePlatesParser.html) is used for extracting license plate content from the OCR result.

### <a name="raw-parser"></a> Raw Parser

[`MBPRawParser`](http://photopay.github.io/photopay-ios/Classes/MBPRawParser.html) is used for obtaining string version of raw OCR result, without performing any smart parsing operations.

### <a name="regex-parser"></a> Regex Parser

[`MBPRegexParser`](http://photopay.github.io/photopay-ios/Classes/MBPRegexParser.html) is used for extracting OCR result content which is in accordance with the given regular expression. Regular expression parsing is not performed with java's regex engine. Instead, it is performed with custom regular expression engine.

### <a name="topup-parser"></a> TopUp Parser

[`MBPTopUpParser`](http://photopay.github.io/photopay-ios/Classes/MBPTopUpParser.html) is used for extracting TopUp (mobile phone coupon) codes from the OCR result. There exists [`TopUpPreset`](http://photopay.github.io/photopay-ios/Enums/MBPTopUpPreset.html) enum with presets for most common vendors. Method `- (void)setTopUpPreset:(MBPTopUpPreset)topUpPreset` can be used to configure parser to only return codes with the appropriate format defined by the used preset.

### <a name="vin-parser"></a> VIN (*Vehicle Identification Number*) Parser

[`MBPVinParser`](http://photopay.github.io/photopay-ios/Classes/MBPVinParser.html) is used for extracting VIN (*Vehicle Identification Number*) from the OCR result.

# <a name="templating-api"></a> Scanning generic documents with Templating API

This section discusses the setting up of `MBPDetectorRecognizer` for scanning templated documents. Please check `Templating-sample` sample app for source code examples.

Templated document is any document which is defined by its template. Template contains the information about how the document should be detected, i.e. found on the camera scene and information about which part of the document contains which useful information.

## <a name="defining-document-detection"></a> Defining how document should be detected

Before performing OCR of the document, _PhotoPay_ first needs to find its location on a camera scene. In order to perform detection, you need to define [MBPDetector](#detector-concept).

You have to set concrete `MBPDetector` when instantiating the `MBPDetectorRecognizer` as a parameter to its constructor.

You can find out more information about detectors that can be used in section [List of available detectors](#detector-list). The most commonly used detector is [`MBPDocumentDetector`](#document-detector).

## <a name="defining-field-extraction"></a> Defining how fields of interest should be extracted

`MBPDetector` produces its result which contains document location. After the document has been detected, all further processing is done on the detected part of the input image.

There may be one or more variants of the same document type, for example for some document there may be old and new version and both of them must be supported. Because of that, for implementing support for each document, one or multiple templating classes are used. `MBPTemplatingClass` is described in [The Templating Class component](#templating-class) section.

`MBPTemplatingClass` holds all needed information and components for processing its class of documents. Templating classes are processed in chain, one by one. On first class for which the data is successfully extracted, the chain is terminated and recognition results are returned. For each input image processing is done in the following way:

1. Classification `MBPProcessorGroups` are run on the defined locations to extract data. `MBPProcessorGroup` is used to define the location of interest on the detected document and `MBPProcessors` that will extract data from that location. You can find more about `MBPProcessorGroup` in the [next section](#processor-group).

2. `MBPTemplatingClassifier` is run with the data extracted by the classification processor groups to decide whether the currently scanned document belongs to the current class or not. Its [classify](http://photopay.github.io/photopay-ios/Protocols/MBPTemplatingClassifier.html) method  simply returns `YES/true` or `NO/false`. If the classifier returns `NO/false`, recognition is moved to the next class in the chain, if it exists. You can find more about `MBPTemplatingClassifier` in [this](#implementing-templating-classifier) section.

3. If the `MBPTemplatingClassifier` has decided that currently scanned document belongs to the current class, non-classification `MBPProcessorGroups` are run to extract other fields of interest.

### <a name="processor-group"></a> The `MBPProcessorGroup` component

In templating API [`MBPProcessorGroup`](http://photopay.github.io/photopay-ios/Classes/MBPProcessorGroup.html) is used to define the location of the field of interest on the detected document and how that location should be processed by setting following parameters in its constructor:

1. Location coordinates relative to document detection which are passed as [`Rectangle`] object.

2. `MBPDewarpPolicy` which determines the resulting image chunk for processing. You can find a description of each `MBPDewarpPolicy`, its purpose and recommendations when it should be used to get the best results in [List of available dewarp policies](#dewarp-policy-list) section.

3. Collection of processors that will be executed on the prepared chunk of the image for current document location. You can find more information about processors in [The Processor concept](#processor-concept) section.

### <a name="dewarp-policy-list"></a> List of available dewarp policies

Concrete `MBPDewarpPolicy` defines how specific location of interest should be dewarped (cropped and rotated). It determines the height and width of the resulting dewarped image in pixels. Here is the list of available dewarp policies with linked doc for more information:

- [`MBPFixedDewarpPolicy`](http://photopay.github.io/photopay-ios/Classes/MBPFixedDewarpPolicy.html)
    - defines the exact height of the dewarped image in pixels
    - **usually the best policy for processor groups that use a legacy OCR engine**

- [`MBPDPIBasedDewarpPolicy`](http://photopay.github.io/photopay-ios/Classes/MBPDPIBasedDewarpPolicy.html):
    - defines the desired DPI (*Dots Per Inch*)
    - the height of the dewarped image will be calculated based on the actual physical size of the document provided by the used detector and chosen DPI
    - **usually the best policy for processor groups that prepare location's raw image for output**

- [`MBPNoUpScalingDewarpPolicy`](http://photopay.github.io/photopay-ios/Classes/MBPNoUpScalingDewarpPolicy.html):
    - defines the maximal allowed height of the dewarped image in pixels
    - the height of the dewarped image will be calculated in a way that no part of the image will be up-scaled
    - if the height of the resulting image is larger than maximal allowed, then the maximal allowed height will be used as actual height, which effectively scales down the image
    - **usually the best policy for processors that use neural networks, for example,  DEEP OCR, hologram detection or NN-based classification**

### <a name="templating-class"></a> The `MBPTemplatingClass` component

[`MBPTemplatingClass`](http://photopay.github.io/photopay-ios/Classes/MBPTemplatingClass.html) enables implementing support for a specific class of documents that should be scanned with templating API. Final implementation of the templating recognizer consists of one or more templating classes, one class for each version of the document.

`MBPTemplatingClass` contains two collections of `MBPProcessorGroups` and a `MBPTemplatingClassifier`.

The two collections of processor groups within `MBPTemplatingClass` are:

1. The classification processor groups which are set by using the [`- (void)setClassificationProcessorGroups:(nonnull NSArray<__kindof MBPProcessorGroup *> *)processorGroups`] method. `MBPProcessorGroups` from this collection will be executed before classification, which means that they are always executed when processing comes to this class.

2. The non-classification processor groups which are set by using the [`- (void)setNonClassificationProcessorGroups:(nonnull NSArray<__kindof MBPProcessorGroup *> *)processorGroups`]method. `MBPProcessorGroups` from this collection will be executed after classification if the classification has been positive.

A component which decides whether the scanned document belongs to the current class is [`MBPTemplatingClass`](http://photopay.github.io/photopay-ios/Classes/MBPTemplatingClass.html). It can be set by using the `- (void)setTemplatingClassifier:(nullable id<MBPTemplatingClassifier>)templatingClassifier` method. If it is not set, non-classification processor groups will not be executed. Instructions for implementing the `MBPTemplatingClassifier` are given in the [next section](#implementing-templating-classifier).

### <a name="implementing-templating-classifier"></a> Implementing the `MBPTemplatingClassifier`

Each concrete templating classifier implements the [`MBPTemplatingClassifier`](http://photopay.github.io/photopay-ios/Protocols/MBPTemplatingClassifier.html) interface, which requires to implement its `classify` method that is invoked while evaluating associated `MBPTemplatingClass`.

Classification decision should be made based on the processing result which is returned by one or more processing units contained in the collection of the classification processor groups. As described in [The ProcessorGroup component](#processor-group) section, each processor group contains one or more `MBPProcessors`. [There are different `MBPProcessors`](#processor-list) which may enclose smaller processing units, for example, [`MBPParserGroupProcessor`](#parser-group-processor) maintains the group of [`MBPParsers`](#parser-concept). Result from each of the processing units in that hierarchy can be used for classification. In most cases `MBPParser` result is used to determine whether some data in the expected format exists on the specified location.

To be able to retrieve results from the various processing units that are needed for classification, their instances must be available when `classify` method is called.

## Obtaining recognition results

When recognition is done, results can be obtained through processing units instances, such as: `MBPProcessors`, `MBPParsers`, etc. which are used for configuring the `MBPTemplatingRecognizer` and later for processing the input image.

# <a name="detector-concept"></a> The `MBPDetector` concept

[`MBPDetector`](http://photopay.github.io/photopay-ios/Classes/MBPDetector.html) is a processing unit used within some `MBPRecognizer` which supports detectors, such as [`MBPDetectorRecognizer`](#detector-recognizer). Concrete `MBPDetector` knows how to find the certain object on the input image. `MBPRecognizer` can use it to perform object detection prior to performing further recognition of detected object's contents.

`MBPDetector` architecture is similar to `MBPRecognizer` architecture described in [The Recognizer concept](#recognizer-concept) section. Each instance also has associated inner `MBPRecognizerResult` object whose lifetime is bound to the lifetime of its parent `MBPDetector` object and it is updated while `MBPDetector` works. If you need your `MBPRecognizerResult` object to outlive its parent `MBPDetector` object, you must make a copy of it by calling its `copy` method.

It also has its internal state and while it is in the *working state* during recognition process, it is not allowed to tweak `MBPDetector` object's properties.

When detection is performed on the input image, each `MBPDetector` in its associated `MBPDetectorResult` object holds the following information:

- [`MBPDetectionCode`](http://photopay.github.io/photopay-ios/Enums/MBPDetectionCode.html) that indicates the type of the detection.

- [`MBPDetectionStatus`](http://photopay.github.io/photopay-ios/Enums/MBPDetectionStatus.html) that represents the status of the detection.

- each concrete detector returns additional information specific to the detector type


To support common use cases, there are several different `MBPDetector` implementations available. They are listed in the next section.

## <a name="detector-list"></a> List of available detectors

### <a name="document-detector"></a> Document Detector

[`MBPDocumentDetector`](http://photopay.github.io/photopay-ios/Classes/MBPDocumentDetector.html) is used to detect card documents, cheques, A4-sized documents, receipts and much more.

It accepts one or more `MBPDocumentSpecifications`. [`MBPDocumentSpecification`](http://photopay.github.io/photopay-ios/Classes/MBPDocumentSpecification.html) represents a specification of the document that should be detected by using edge detection algorithm and predefined aspect ratio.

For the most commonly used document formats, there is a helper method  `+ (instancetype)createFromPreset:(MBPDocumentSpecificationPreset)preset` which creates and initializes the document specification based on the given [`MBPDocumentSpecificationPreset`](http://photopay.github.io/photopay-ios/Enums/MBPDocumentSpecificationPreset.html).

For the list of all available configuration methods see [`MBPDocumentDetector`](http://photopay.github.io/photopay-ios/Classes/MBPDocumentDetector.html) doc, and for available result content see [`MBPDocumentDetectorResult`](http://photopay.github.io/photopay-ios/Classes/MBPDocumentDetectorResult.html) doc.


### <a name="mrtd-detector"></a> MRTD Detector

[`MBPMrtdDetector`](http://photopay.github.io/photopay-ios/Classes/MBPMrtdDetector.html) is used to perform detection of *Machine Readable Travel Documents (MRTD)*.

Method `- (void)setMrtdSpecifications:(NSArray<__kindof MBPMrtdSpecification *> *)mrtdSpecifications` can be used to define which MRTD documents should be detectable. It accepts the array of `MBPMrtdSpecification`. [`MBPMrtdSpecification`](http://photopay.github.io/photopay-ios/Classes/MBPMrtdSpecification.html) represents specification of MRTD that should be detected. It can be created from the [`MBPMrtdSpecificationPreset`](http://photopay.github.io/photopay-ios/Enums/MBPMrtdSpecificationPreset.html) by using `+ (instancetype)createFromPreset:(MBPMrtdSpecificationPreset)preset` method.

If `MBPMrtdSpecifications` are not set, all supported MRTD formats will be detectable.

For the list of all available configuration methods see [`MBPMrtdDetector`](http://photopay.github.io/photopay-ios/Classes/MBPMrtdDetector.html) doc, and for available result content see [`MBPMrtdDetectorResult`](http://photopay.github.io/photopay-ios/Classes/MBPMrtdDetectorResult.html) doc.

# Creating customized build of PhotoPay SDK

If your final app size is too large, you can create a customised build of _MicroBlink.framework_ and _MicroBlink.bundle_ which will contain only features and resources that you really need.

In order to create customised build of PhotoPay SDK, you first need to download the static distribution of PhotoPay SDK. A valid production licence key is required in order to gain access to the download link of PhotoPay SDK static distribution. Once you have a valid production licence key, please contact our [support team](http://help.microblink.com) and ask them to provide you with the download link. After they give you access to the static distribution of PhotoPay SDK, you will be able to download it from you account at [MicroBlink Developer Dashboard](https://www.microblink.com/login).

The static distribution of PhotoPay SDK is a large zip file (several hundred megabytes) which contains static libraries of PhotoPay SDK's native code, all assets and resources and a script which will create the customised build for you.

### Prerequisites for creating customised build

In order to create customised build of PhotoPay SDK, you will need following tools:

- XCode and latest iOS SDK
- CMake - you can install it from Homebrew with `brew install cmake`, or you can download it from [official page](https://cmake.org/download/)
	- please note that command-line version of CMake is required, so if you have downloaded CMake from official page, make sure you install command-line support as well

### Steps for creating customised build

1. Obtain the static distribution of PhotoPay SDK by [contacting us](http://help.microblink.com)
2. Download the zip from link that you will be provided
3. Unzip the file into an empty folder
4. Edit the file `enabled-features.cmake`
	- you should enable only features that you need to use by setting appropriate variables to `ON`. 
	- the list of all possible feature variables can be found in `features.cmake` 
		- for each `feature_option` command, first parameter defines the feature variable, and the second is the description of the feature, i.e. what it provides. Other parameters are information for script to work correctly.
	- you should not edit any file except `enabled-features.cmake` (except if instructed so by our support team) to ensure creation of customised build works well
5. Open terminal and navigate to folder with zip's contents.
6. Execute command `./create-custom-build.sh` and select whether you want static or dynamic frameowk.
	- when asked, enter `s` to build static framework or `d` to build dynamic framework
7. After several minutes (depedending of CPU speed of your computer), customised build will appear in the `Release` folder. Use that bundle and framework in your app instead of default one.

#### Warning:

Attempt to use feature within your app which was not enabled in customised build will cause a linker error when linking against the customised framework.

### Troubleshooting:

#### Getting `unrecognized selector sent to instance` when using customised static framework, while everything works OK with dynamic framework

This happens when your app has not been linked with `-ObjC` flag against static framework. The problem is related to using Objective C categories within static library which are thrown away by linker. You can see more information in [official Apple documentation](https://developer.apple.com/library/content/qa/qa1490/_index.html).

#### App crashing when scanning starts with log message _Failed to load resource XX. The program will now crash._

This means that a required resource was not packaged into final app. This usually indicates a bug in our script that makes the customised build. Please [contact us](http://help.microblink.com) and send your version of `enabled-features.cmake` and crash log.

#### CMake error while running script.

You probably have a typo in `enabled-features.cmake`. CMake is very sensitive language and will throw an non-understandable error if you have a typo or invoke any of its commands with wrong number of parameters.

#### Linker error while running the script.

This sometimes happens when XCode's link time optimizer runs out of memory. Usually running the script again solves the problem. Please reboot your Mac if this keeps happening.

#### Keeping only `FEATURE_MRTD` creates rather large `MicroBlink.bundle`

`FEATURE_MRTD` marks the _MRTD recognizer_. However, _MRTD recognizer_ can also be used in _Templating API_ mode where non-MRZ data can be scanned. To perform OCR of non-MRZ data, a rather large OCR model must be used, which supports all fonts. If you only plan to scan MRZ part of the document, you can edit the `features.cmake` in following way:

- find the following line:

```
feature_resources( FEATURE_MRTD model_mrtd model_general_blink_ocr model_micr model_arabic )
```

- keep only `model_mrtd` in the list, i.e. modify the line so that it will be like this:

```
feature_resources( FEATURE_MRTD model_mrtd )
```

This will keep only support for reading MRZ zone in OCR - you will not be able to scan non-MRZ data with such configuration using _MRTD recognizer_, however you will reduce the `MicroBlink.bundle` and then final app size by more than 4MB.

##### More information about OCR models in `FEATURE_MRTD`

- `model_mrtd` is OCR model for performing OCR of MRZ zone
- `model_arabic` is OCR model for performing OCR of digits used in arabic languages - text scanning is not supported
- `model_micr` is OCR model for performing OCR of [Magnetic Ink Characters](https://en.wikipedia.org/wiki/Magnetic_ink_character_recognition)
- `model_general_blink_ocr` is OCR model for performing general-purpose OCR. This model is usually required for performing OCR of non-MRZ text on documents.

# <a name="localization"></a> Localization

The SDK is localized on following languages: Arabic, Chinese simplified, Chinese traditional, Croatian, Czech, Dutch, Filipino, French, German, Hebrew, Hungarian, Indonesian, Italian, Malay, Portuguese, Romanian, Slovak, Slovenian, Spanish, Thai, Vietnamese.

If you would like us to support additional languages or report incorrect translation, please contact us at [help.microblink.com](http://help.microblink.com).

If you want to add additional languages yourself or change existing translations, you need to set `customLocalizationFileName` property on [`MBPMicroblinkApp`](http://photopay.github.io/photopay-ios/Classes/MBPMicroblinkApp.html) object to your strings file name.

For example, let's say that we want to change text "Scan the front side of a document" to "Scan the front side" in BlinkID sample project. This would be the steps:
* Find the translation key in en.strings file inside PhotoPay.framework
* Add a new file MyTranslations.strings to the project by using "Strings File" template
* With MyTranslations.string open, in File inspector tap "Localize..." button and select English
* Add the translation key "blinkid_generic_message" and the value "Scan the front side" to MyTranslations.strings
* Finally in AppDelegate.swift in method `application(_:, didFinishLaunchingWithOptions:)` add `MBPMicroblinkApp.instance()?.customLocalizationFileName = "MyTranslations"`

# <a name="troubleshooting"></a> Troubleshooting

## <a name="troubleshooting-integration-problems"></a> Integration problems

In case of problems with integration of the SDK, first make sure that you have tried integrating it into Xcode by following [integration instructions](#quick-start).

If you have followed [Xcode integration instructions](#quick-start) and are still having integration problems, please contact us at [help.microblink.com](http://help.microblink.com).

## <a name="troubleshooting-sdk-problems"></a> SDK problems

In case of problems with using the SDK, you should do as follows:

### <a name="troubleshooting-licensing-problems"></a> Licencing problems

If you are getting "invalid licence key" error or having other licence-related problems (e.g. some feature is not enabled that should be or there is a watermark on top of camera), first check the console. All licence-related problems are logged to error log so it is easy to determine what went wrong.

When you have determine what is the licence-relate problem or you simply do not understand the log, you should contact us [help.microblink.com](http://help.microblink.com). When contacting us, please make sure you provide following information:

* exact Bundle ID of your app (from your `info.plist` file)
* licence that is causing problems
* please stress out that you are reporting problem related to iOS version of PhotoPay SDK
* if unsure about the problem, you should also provide excerpt from console containing licence error

### <a name="troubleshooting-other-problems"></a> Other problems

If you are having problems with scanning certain items, undesired behaviour on specific device(s), crashes inside PhotoPay SDK or anything unmentioned, please do as follows:
	
* Contact us at [help.microblink.com](http://help.microblink.com) describing your problem and provide following information:
	* log file obtained in previous step
	* high resolution scan/photo of the item that you are trying to scan
	* information about device that you are using
	* please stress out that you are reporting problem related to iOS version of PhotoPay SDK

## <a name="troubleshooting-faq"></a> Frequently asked questions and known problems
Here is a list of frequently asked questions and solutions for them and also a list of known problems in the SDK and how to work around them.

#### In demo everything worked, but after switching to production license I get `NSError` with `MBPMicroblinkSDKRecognizerErrorDomain` and `MBPRecognizerFailedToInitalize` code as soon as I construct specific [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object

Each license key contains information about which features are allowed to use and which are not. This `NSError` indicates that your production license does not allow using of specific `MBPRecognizer` object. You should contact [support](http://help.microblink.com) to check if provided licence is OK and that it really contains all features that you have purchased.

#### I get `NSError` with `MBPMicroblinkSDKRecognizerErrorDomain` and `MBPRecognizerFailedToInitalize` code with trial license key

Whenever you construct any [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object or, a check whether license allows using that object will be performed. If license is not set prior constructing that object, you will get `NSError` with `MBPMicroblinkSDKRecognizerErrorDomain` and `MBPRecognizerFailedToInitalize` code. We recommend setting license as early as possible in your app.

#### Undefined Symbols on Architecture armv7

Make sure you link your app with iconv and Accelerate frameworks as shown in [Quick start](#quick-start).
If you are using Cocoapods, please be sure that you've installed `git-lfs` prior to installing pods. If you are still getting this error, go to project folder and execute command `git-lfs pull`.

### Crash on armv7 devices

SDK crashes on armv7 devices if bitcode is enabled. We are working on it.

#### In my `didFinish` callback I have the result inside my `MBPRecognizer`, but when scanning activity finishes, the result is gone

This usually happens when using [`MBPRecognizerRunnerViewController`](http://photopay.github.io/photopay-ios/Protocols/MBPRecognizerRunnerViewController.html) and forgetting to pause the [`MBPRecognizerRunnerViewController`](http://photopay.github.io/photopay-ios/Protocols/MBPRecognizerRunnerViewController.html) in your `didFinish` callback. Then, as soon as `didFinish` happens, the result is mutated or reset by additional processing that `MBPRecognizer` performs in the time between end of your `didFinish` callback and actual finishing of the scanning activity. For more information about statefulness of the `MBPRecognizer` objects, check [this section](#recognizer-concept).

#### Unsupported architectures when submitting app to App Store

PhotoPay.framework is a dynamic framework which contains slices for all architectures - device and simulator. If you intend to extract .ipa file for ad hoc distribution, you'll need to preprocess the framework to remove simulator architectures.

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

### Disable logging

Logging can be disabled by calling `disableMicroblinkLogging` method on [`MBPLogger`](http://photopay.github.io/photopay-ios/Classes/MBPLogger.html) instance.
# <a name="info"></a> Additional info

Complete API reference can be found [here](http://photopay.github.io/photopay-ios/index.html). 

For any other questions, feel free to contact us at [help.microblink.com](http://help.microblink.com).