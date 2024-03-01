<p align="center" >
  <img src="https://raw.githubusercontent.com/wiki/blinkid/blinkid-ios/Images/logo-microblink.png" alt="Microblink" title="Microblink">
</p>

# PhotoPay SDK for payment slips scanning

PhotoPay SDK is a delightful component for quick and easy scanning of payment slips and payment barcodes. The SDK is powered with [Microblink's](http://www.microblink.com) industry-proven and world leading OCR and barcode scanning technology, and offers:

- integrated camera management
- layered API, allowing everything from simple integration to complex UX customizations.
- lightweight and no internet connection required
- enteprise-level security standards
- data parsing from payment slips and barcode standards

PhotoPay is a part of family of SDKs developed by [Microblink](http://www.microblink.com) for optical text recognition, barcode scanning, ID document scanning, payment slips and barcodes and many others.
# Table of contents

- [Requirements](#requirements)
- [Quick Start](#quick-start)
- [Advanced PhotoPay integration instructions](#advanced-integration)
	- [Built-in overlay view controllers and overlay subviews](#ui-customizations)
		- [Using `MBPBarcodeOverlayViewController`](#using-pdf417-overlay-viewcontroller)
		- [Using `MBPFieldByFieldOverlayViewController`](#using-fieldbyfield-overlay-viewcontroller)
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
		- [IBAN Parser](#iban-parser)
		- [Raw Parser](#raw-parser)
		- [Regex Parser](#regex-parser)
- [Troubleshooting](#troubleshooting)
	- [Integration problems](#troubleshooting-integration-problems)
	- [SDK problems](#troubleshooting-sdk-problems)
		- [Licencing problems](#troubleshooting-licensing-problems)
		- [Other problems](#troubleshooting-other-problems)
	- [Frequently asked questions and known problems](#troubleshooting-faq)
- [Additional info](#info)


# <a name="requirements"></a> Requirements

SDK package contains PhotoPay framework and one or more sample apps which demonstrate framework integration. The framework can be deployed in **iOS 13.0 or later**.
**NOTE:** The SDK doesn't contain bitcode anymore. 
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

Also, for initialization purposes, the ViewController which initiates the scan have private properties for [`MBPCroatiaPdf417PaymentRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPCroatiaPdf417PaymentRecognizer.html), so we know how to obtain result.

Swift

```swift
class ViewController: UIViewController, MBPPhotopayOverlayViewControllerDelegate  {

    var croPdf417PaymentRecognizer: MBPCroatiaPdf417PaymentRecognizer?

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    @IBAction func didTapScan(_ sender: AnyObject) {

        let settings = MBPPhotopayOverlaySettings()
        croPdf417PaymentRecognizer = MBPCroatiaPdf417PaymentRecognizer()

        let recognizerList = [self.croPdf417PaymentRecognizer!]
        let recognizerCollection = MBPRecognizerCollection(recognizers: recognizerList)

        /** Create your overlay view controller */
        let photopayOverlayViewController = MBPPhotopayOverlayViewController(settings: settings, recognizerCollection: recognizerCollection, delegate: self)

        /** Create recognizer view controller with wanted overlay view controller */
        let recognizerRunnerViewController: UIViewController = MBPViewControllerFactory.recognizerRunnerViewController(withOverlayViewController: photopayOverlayViewController)

        /** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
        present(recognizerRunnerViewController!, animated: true, completion: nil)
    }
}
```

Objective-C

```objective-c
@interface ViewController () <MBPPhotopayOverlayViewControllerDelegate>

@property (nonatomic, strong) MBPCroatiaPdf417PaymentRecognizer *croPdf417PaymentRecognizer;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
}


- (IBAction)didTapScan:(id)sender {

    MBPPhotopayOverlaySettings* settings = [[MBPPhotopayOverlaySettings alloc] init];

    self.croPdf417PaymentRecognizer = [[MBPCroatiaPdf417PaymentRecognizer  alloc] init];

    /** Create recognizer collection */
    MBPRecognizerCollection *recognizerCollection = [[MBPRecognizerCollection alloc] initWithRecognizers:@[self.croPdf417PaymentRecognizer ]];

    MBPPhotopayOverlayViewController *overlayVC = [[MBPPhotopayOverlayViewController alloc] initWithSettings:settings recognizerCollection:recognizerCollection delegate:self];
    UIViewController<MBPRecognizerRunnerViewController>* recognizerRunnerViewController = [MBPViewControllerFactory recognizerRunnerViewControllerWithOverlayViewController:overlayVC];

    /** Present the recognizer runner view controller. You can use other presentation methods as well (instead of presentViewController) */
    [self presentViewController:recognizerRunnerViewController animated:YES completion:nil];

}

@end
```

### 4. License key

A valid license key is required to initialize scanning. You can request a **free trial license key**, after you register, at [Microblink Developer Hub](https://account.microblink.com/signin).

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
MBPMicroblinkSDK.shared().setLicenseResource("license-key-file", withExtension: "key", inSubdirectory: "directory-to-license-key", for: Bundle.main, errorCallback: block)
```

Objective-C

```objective-c
[[MBPMicroblinkSDK sharedInstance] setLicenseResource:@"license-key-file" withExtension:@"key" inSubdirectory:@"" forBundle:[NSBundle mainBundle] errorCallback: block];
```

If the licence is invalid or expired then the methods above will throw an **exception**.

### 5. Registering for scanning events

In the previous step, you instantiated [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html) object with a delegate object. This object gets notified on certain events in scanning lifecycle. In this example we set it to `self`. The protocol which the delegate has to implement is [`MBPPhotopayOverlayViewControllerDelegate`](http://photopay.github.io/photopay-ios//Protocols/MBPPhotopayOverlayViewControllerDelegate.html) protocol. It is necessary to conform to that protocol. We will discuss more about protocols in [Advanced integration section](#advanced-integration). You can use the following default implementation of the protocol to get you started.

Swift

```swift
func photopayOverlayViewControllerDidFinishScanning(_ photopayOverlayViewController: MBPPhotopayOverlayViewController, state: MBPRecognizerResultState) {

    // this is done on background thread
    // check for valid state
    if state == .valid {

        // first, pause scanning until we process all the results
        photopayOverlayViewController.recognizerRunnerViewController?.pauseScanning()

        DispatchQueue.main.async(execute: {() -> Void in
            // All UI interaction needs to be done on main thread
        })
    }
}

func photopayOverlayViewControllerDidTapClose(_ photopayOverlayViewController: MBPPhotopayOverlayViewController) {
    // Your action on cancel
}
```

Objective-C

```objective-c
- (void)photopayOverlayViewControllerDidFinishScanning:(MBPPhotopayOverlayViewController *)photopayOverlayViewController state:(MBPRecognizerResultState)state {

    // this is done on background thread
    // check for valid state
    if (state == MBPRecognizerResultStateValid) {

        // first, pause scanning until we process all the results
        [photopayOverlayViewController.recognizerRunnerViewController pauseScanning];

        dispatch_async(dispatch_get_main_queue(), ^{
            // All UI interaction needs to be done on main thread
        });
    }
}

- (void)photopayOverlayViewControllerDidTapClose:(MBPPhotopayOverlayViewController *)photopayOverlayViewController {
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

Each [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) has a [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes.html#/c:objc(cs)MBPRecognizerResult) object, which contains the data that was extracted from the image. The [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes.html#/c:objc(cs)MBPRecognizerResult) object is a member of corresponding [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object its lifetime is bound to the lifetime of its parent [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object. If you need your `MBPRecognizerResult` object to outlive its parent [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object, you must make a copy of it by calling its method `copy`.

While [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object works, it changes its internal state and its result. The [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object's [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes.html#/c:objc(cs)MBPRecognizerResult) always starts in `Empty` state. When corresponding [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object performs the recognition of given image, its [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes.html#/c:objc(cs)MBPRecognizerResult) can either stay in `Empty` state (in case [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html)failed to perform recognition), move to `Uncertain` state (in case [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) performed the recognition, but not all mandatory information was extracted) or move to `Valid` state (in case [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) performed recognition and all mandatory information was successfully extracted from the image).

As soon as one [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object's [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes.html#/c:objc(cs)MBPRecognizerResult) within [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html) given to `MBPRecognizerRunner` or `MBPRecognizerRunnerViewController` changes to `Valid` state, the `onScanningFinished` callback will be invoked on same thread that performs the background processing and you will have the opportunity to inspect each of your [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects' [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes.html#/c:objc(cs)MBPRecognizerResult) to see which one has moved to `Valid` state.

As soon as `onScanningFinished` method ends, the `MBPRecognizerRunnerViewController` will continue processing new camera frames with same [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects, unless `paused`. Continuation of processing or `reset` recognition will modify or reset all [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects's [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerResult). When using built-in activities, as soon as `onScanningFinished` is invoked, built-in activity pauses the `MBPRecognizerRunnerViewController` and starts finishing the activity, while saving the [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html) with active [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html).

## `MBPRecognizerCollection` concept

The [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html) is is wrapper around [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects that has array of [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects that can be used to give [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects to `MBPRecognizerRunner` or `MBPRecognizerRunnerViewController` for processing.

The [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html) is always constructed with array `[[MBPRecognizerCollection alloc] initWithRecognizers:recognizers]` of [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects that need to be prepared for recognition (i.e. their properties must be tweaked already).

The [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html) manages a chain of [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects within the recognition process. When a new image arrives, it is processed by the first [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) in chain, then by the second and so on, iterating until a [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object's [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes.html#/c:objc(cs)MBPRecognizerResult) changes its state to `Valid` or all of the [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects in chain were invoked (none getting a `Valid` result state).

You cannot change the order of the [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects within the chain - no matter the order in which you give [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects to [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html), they are internally ordered in a way that provides best possible performance and accuracy. Also, in order for SDK to be able to order [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects in recognition chain in a best way possible, it is not allowed to have multiple instances of [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects of the same type within the chain. Attempting to do so will crash your application.

# <a name="available-recognizers"></a> List of available recognizers

This section will give a list of all [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) objects that are available within PhotoPay SDK, their purpose and recommendations how they should be used to get best performance and user experience.

## <a name="frame-grabber-recognizer"></a> Frame Grabber Recognizer

The [`MBPFrameGrabberRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPFrameGrabberRecognizer.html) is the simplest recognizer in SDK, as it does not perform any processing on the given image, instead it just returns that image back to its `onFrameAvailable`. Its result never changes state from empty.

This recognizer is best for easy capturing of camera frames with `MBPRecognizerRunnerViewController`. Note that [`MBPImage`](http://photopay.github.io/photopay-ios/Classes/MBPImage.html) sent to `onFrameAvailable` are temporary and their internal buffers all valid only until the `onFrameAvailable` method is executing - as soon as method ends, all internal buffers of [`MBPImage`](http://photopay.github.io/photopay-ios/Classes/MBPImage.html) object are disposed. If you need to store [`MBPImage`](http://photopay.github.io/photopay-ios/Classes/MBPImage.html) object for later use, you must create a copy of it by calling `copy`.

## <a name="success-frame-grabber-recognizer"></a> Success Frame Grabber Recognizer

The [`MBPSuccessFrameGrabberRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSuccessFrameGrabberRecognizer.html) is a special `MBPecognizer` that wraps some other [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) and impersonates it while processing the image. However, when the [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) being impersonated changes its [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes.html#/c:objc(cs)MBPRecognizerResult) into `Valid` state, the [`MBPSuccessFrameGrabberRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSuccessFrameGrabberRecognizer.html) captures the image and saves it into its own [`MBPSuccessFrameGrabberRecognizerResult`](http://photopay.github.io/photopay-ios/Classes/MBPSuccessFrameGrabberRecognizerResult.html) object.

Since [`MBPSuccessFrameGrabberRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSuccessFrameGrabberRecognizer.html)  impersonates its slave [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object, it is not possible to give both concrete [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object and `MBPSuccessFrameGrabberRecognizer` that wraps it to same `MBPRecognizerCollection` - doing so will have the same result as if you have given two instances of same [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) type to the [`MBPRecognizerCollection`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizerCollection.html) - it will crash your application.

This recognizer is best for use cases when you need to capture the exact image that was being processed by some other [`MBPRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPRecognizer.html) object at the time its [`MBPRecognizerResult`](http://photopay.github.io/photopay-ios/Classes.html#/c:objc(cs)MBPRecognizerResult) became `Valid`. When that happens, `MBPSuccessFrameGrabberRecognizer's` `MBPSuccessFrameGrabberRecognizerResult` will also become `Valid` and will contain described image.

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
## <a name="photopay-recognizers"></a> PhotoPay recognizers by countries

### <a name="photopay-austria"></a> Austria

The [`MBPAustriaQrCodePaymentRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPAustriaQrCodePaymentRecognizer.html) is recognizer specialised for scanning Austrian payment QR codes, such as Stuzza codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

The [`MBPAustriaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPAustriaSlipRecognizer.html) is recognizer specialised for scanning Austrian payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

### <a name="photopay-belgium"></a> Belgium

The [`MBPBelgiumSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPBelgiumSlipRecognizer.html) is recognizer specialised for scanning Belgian payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

### <a name="photopay-croatia"></a> Croatia

The [`MBPCroatiaPdf417PaymentRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPCroatiaPdf417PaymentRecognizer.html) is recognizer specialised for scanning Croatia payment Pdf417 codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

The [`MBPCroatiaQrCodePaymentRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPCroatiaQrCodePaymentRecognizer.html) is recognizer specialised for scanning Croatia payment QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

The [`MBPCroatiaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPCroatiaSlipRecognizer.html) is recognizer specialised for scanning Croatia payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

### <a name="photopay-czechia"></a> Czechia

The [`MBPCzechiaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPCzechiaQrCodeRecognizer.html) is recognizer specialised for scanning Czech payment QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

The [`MBPCzechiaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPCzechiaSlipRecognizer.html) is recognizer specialised for scanning Czech payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

### <a name="photopay-germany"></a> Germany

The [`MBPGermanyQrCodePaymentRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPGermanyQrCodePaymentRecognizer.html) is recognizer specialised for scanning German payment QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

The [`MBPGermanySlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPGermanySlipRecognizer.html) is recognizer specialised for scanning German payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

### <a name="photopay-hungary"></a> Hungary

The [`MBPHungarySlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPHungarySlipRecognizer.html) is recognizer specialised for scanning Hungarian payment slips - white and yellow.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

### <a name="photopay-kosovo"></a> Kosovo

The [`MBPKosovoSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPKosovoSlipRecognizer.html) is recognizer specialised for scanning Kosovo payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPFieldOfViewOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPFieldOfViewOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

The [`MBPKosovoCode128PaymentRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPKosovoCode128PaymentRecognizer.html) is recognizer specialised for scanning code 128 barcodes found on Kosovo payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPFieldOfViewOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPFieldOfViewOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

### <a name="photopay-netherlands"></a> Netherlands

The [`MBPNetherlandsSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPNetherlandsSlipRecognizer.html) is recognizer specialised for scanning Dutch payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPFieldOfViewOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPFieldOfViewOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

### <a name="photopay-sepa"></a> SEPA

The [`MBPSepaQrCodePaymentRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSepaQrCodePaymentRecognizer.html) is recognizer specialised for scanning SEPA QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

### <a name="photopay-serbia"></a> Serbia

The [`MBPSerbiaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSerbiaQrCodeRecognizer.html) is recognizer specialised for scanning Serbian QR payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

The [`MBPSerbiaPdf417Recognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSerbiaPdf417Recognizer.html) is recognizer specialised for scanning Serbian PDF 417 payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

### <a name="photopay-slovakia"></a> Slovakia

The [`MBPSlovakiaCode128PaymentRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSlovakiaCode128PaymentRecognizer.html) is recognizer specialised for scanning Slovakian CODE 128 payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

The [`MBPSlovakiaDataMatrixPaymentRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSlovakiaDataMatrixPaymentRecognizer.html) is recognizer specialised for scanning Slovakian Data Matrix payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

The [`MBPSlovakiaQrCodeRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSlovakiaQrCodeRecognizer.html) is recognizer specialised for scanning Slovakian QR payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

The [`MBPSlovakiaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSlovakiaSlipRecognizer.html) is recognizer specialised for scanning Slovakian payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

### <a name="photopay-slovenia"></a> Slovenia

The [`MBPSloveniaSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSloveniaSlipRecognizer.html) is recognizer specialised for scanning Slovenian UPN payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

The [`MBPSloveniaQrCodePaymentRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSloveniaQrCodePaymentRecognizer.html) is recognizer specialised for scanning Slovenian QR payment barcodes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

### <a name="photopay-switzerland"></a> Switzerland

The [`MBPSwitzerlandQrCodePaymentRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSwitzerlandQrCodePaymentRecognizer.html) is recognizer specialised for scanning Swiss payment QR codes, such as Stuzza codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

The [`MBPSwitzerlandSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPSwitzerlandSlipRecognizer.html) is recognizer specialised for scanning Swiss payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

### <a name="photopay-uk"></a> United Kingdom

The [`MBPUnitedKingdomQrCodePaymentRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPUnitedKingdomQrCodePaymentRecognizer.html) is recognizer specialised for scanning UK payment QR codes.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.

The [`MBPUnitedKingdomSlipRecognizer`](http://photopay.github.io/photopay-ios/Classes/MBPUnitedKingdomSlipRecognizer.html) is recognizer specialised for scanning UK payment slips.

This recognizer can be used in any overlay view controller, but it works best with the [`MBPPhotopayOverlayViewController`](http://photopay.github.io/photopay-ios/Classes/MBPPhotopayOverlayViewController.html), which has UI best suited for payment slip and barcode scanning.
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

`MBPParserGroupProcessor` is most commonly used `MBPProcessor`. It is used whenever the OCR is needed. After the OCR is performed and all parsers are run, parsed results can be obtained through parser objects that are enclosed in the group. `MBPParserGroupProcessor` instance also has associated inner `MBPParserGroupProcessorResult` whose state is updated during processing and its property `ocrLayout` can be used to obtain the raw [`MBPOcrLayout`](http://photopay.github.io/photopay-ios/Classes/MBPOcrLayout.html) that was used for parsing data.

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

### <a name="iban-parser"></a> IBAN Parser

[`MBPIbanParser`](http://photopay.github.io/photopay-ios/Classes/MBPIbanParser.html) is used for extracting IBAN (*International Bank Account Number*) from the OCR result.

### <a name="raw-parser"></a> Raw Parser

[`MBPRawParser`](http://photopay.github.io/photopay-ios/Classes/MBPRawParser.html) is used for obtaining string version of raw OCR result, without performing any smart parsing operations.

### <a name="regex-parser"></a> Regex Parser

[`MBPRegexParser`](http://photopay.github.io/photopay-ios/Classes/MBPRegexParser.html) is used for extracting OCR result content which is in accordance with the given regular expression. Regular expression parsing is not performed with java's regex engine. Instead, it is performed with custom regular expression engine.

### Other parsers

There are also other specialized parsers. You can see a list of all o them [here](http://photopay.github.io/photopay-ios/Classes.html).
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

#### In my `didFinish` callback I have the result inside my `MBPRecognizer`, but when scanning activity finishes, the result is gone

This usually happens when using [`MBPRecognizerRunnerViewController`](http://photopay.github.io/photopay-ios/Protocols/MBPRecognizerRunnerViewController.html) and forgetting to pause the [`MBPRecognizerRunnerViewController`](http://photopay.github.io/photopay-ios/Protocols/MBPRecognizerRunnerViewController.html) in your `didFinish` callback. Then, as soon as `didFinish` happens, the result is mutated or reset by additional processing that `MBPRecognizer` performs in the time between end of your `didFinish` callback and actual finishing of the scanning activity. For more information about statefulness of the `MBPRecognizer` objects, check [this section](#recognizer-concept).

### Disable logging

Logging can be disabled by calling `disableMicroblinkLogging` method on [`MBPLogger`](http://photopay.github.io/photopay-ios/Classes/MBPLogger.html) instance.
# <a name="info"></a> Additional info

Complete API reference can be found [here](http://photopay.github.io/photopay-ios/index.html). 

For any other questions, feel free to contact us at [help.microblink.com](http://help.microblink.com).