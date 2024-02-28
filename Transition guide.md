## 9.1.0

- The SDK dropped support for Mac Catalyst

## 9.0.0
- The SDK now supports iOS 13 or higher.

## 8.1.0
- No changes

## 8.0.0

- We renamed the SDK from `Microblink` to `PhotoPay`.
- The prefix for SDK classes changed from `MB` to `MBP`.
- The SDK now includes only `PhotoPay` and `BlinkInput` recognizers. For `BlinkID` recognizers, please import `BlinkID` SDK.
- The SDK now supports iOS 11 or higher.

## 7.13.0

- No changes

## 7.12.0

### New features:

- We have full support for Apple Silicon!

### Framework formats and architectures:

- Use `.xcframework` as we have full Apple Silicon and Intel support.
- We are still supporting `fat binary .framework` format, but we removed simulator slices from it.

### Minor API changes:

- We've replaced `Using time-limited license!` warning with `Using trial license!` warning. The warning message is displayed when using a trial license key. To disable it, use `showTrialLicenseWarning` on `MBCMicroblinkSDK`.

## 7.11.0

### Major API changes:

- We've added an error callback when setting license keys on `MBMicroblinkSDK`
	- You will be getting error callback containing the reason why you could not unlock the SDK - see `MBLicenseError`

### Note on ARM Macs

- We are supporting `ARM64 Device` slice through our `.xcframework` format.
- We are still working on supporting the `ARM64 Simulator` slice for newly released ARM Macs. We will update our SDK with `ARM64 Simulator` support as soon as it’s out.

### Known issues:

- SDK crashes on armv7 devices if bitcode is enabled. We are working on it.

## 7.10.0

### iOS version support change:

- From now on, we are not supporting **iOS 8** version.

### Major API change:

 - We added `errorCallback` on `MBMicroblinkSDK` methods which needs to be implemented for properly setting up the license key.

### Minor API changes:

- We have made some changes to the **MBBlinkIdRecognizer** and **MBBlinkIdCombinedRecognizer**:
	- We renamed `MBDocumentImageMoireStatus` to `MBImageAnalysisDetectionStatus`.
	- We grouped the `conditions` member from the results with the `MBDriverLicenseDetailedInfo` structure.
- We renamed `MBRecogitionMode` to `MBRecognitionDebugMode` in `MBRecognizerCollection`.
- Swift:
	- We renamed all `sharedInstance` to `shared`.
	- All enums are now `Int`.
	- All `unsigned integers` are now `Int`.


## 7.9.0

- We moved `MBBlinkIdRecognizerResult` members `colorStatus` and `moireStatus` to the result's `imageAnalysisResult` (`frontImageAnalysisResult` and `backImageAnalysisResult` in `MBBlinkIDCombinedRecognizerResult`).
- We moved all resources inside framework, we are not shipping `Microblink.bundle` anymore

## 7.8.0

- No changes

## 7.7.0

- Major API changes:
    - Swift Module has been renamed from `MicroBlink` to `Microblink`

- Minor API changes:
    - in combined recognizers results, `documentDataMatch` value is now returned as `MBDataMatchResult` enum with three possible values: `NotPerformed`,  `Failed` and `Success`
    - methods `pauseScanning` and `resumeScanningAndResetState` in `MBRecognizerRunnerViewController` do not return anymore `BOOL`
        - use `isScanningPaused` to check if scanning is paused

## 7.5.0
- Minor API changes:
    - `MBDocumentFaceRecognizer` - removed the `tryBothOrientations` option (improved scan in all directions is enabled by default)
    - renamed following recognizers:
        - `MBCroatiaPdf417Recognizer` to `MBCroatiaPdf417PaymentRecognizer`
        - `MBCroatiaQrCodeRecognizer` to `MBCroatiaQrCodePaymentRecognizer`
        - `MBSepaQrCodeRecognizer` to `MBSepaQrCodePaymentRecognizer`
        - `MBSlovakiaCode128Recognizer` to `MBSlovakiaCode128PaymentRecognizer`
        - `MBSlovakiaDataMatrixRecognizer` to `MBSlovakiaDataMatrixPaymentRecognizer`
        - `MBSlovakiaQrCodeRecognizer` to `MBSlovakiaQrCodePaymentRecognizer`
        - `MBSloveniaQrCodeRecognizer` to `MBSloveniaQrCodePaymentRecognizer`
        - `MBSwitzerlandQrCodeRecognizer` to `MBSwitzerlandQrCodePaymentRecognizer`
        - `MBUnitedKingdomQrCodeRecognizer` to `MBUnitedKingdomQrCodePaymentRecognizer`

## 7.4.0
- Minor API changes:
    - `partialRecognitionTimeout` in `MBRecognizerCollection` default value has been changed to **0** which means no timeout will be reported in which partial scanning results will be returned to the user

## 7.3.0

- Minor API changes
    - renamed `MBAustriaQrCodeRecognizer` to `MBAustriaQrCodePaymentRecognizer` and fields in its `Result`:
        - `IBAN` to `iban`
        - `BIC` to `bic`
        - `referenceNumber` to `reference`
    - renamed `MBGermanyQrCodeRecognizer` to `MBGermanyQrCodePaymentRecognizer` and updated fields in its `Result`:
        - `authority` is returned as `String`
        - renamed fields:
            - `IBAN` to `iban`
            - `BIC` to `bic`
            - `paymentReference` to `reference`
            - `BLZ` to `bankCode`
        - added fields:
            - `creditorId`
            - `dateOfSignature`
            - `displayData`
            - `formFunction`
            - `formType`
            - `formVersion`
            - `mandateId`
            - `periodicFirstExecutionDate`
            - `periodicLastExecutionDate`
            - `periodicTimeUnit`
            - `periodicTimeUnitRotation`
            - `postingKey`
    - renamed `MBKosovoCode128Recognizer` to `MBKosovoCode128PaymentRecognizer` and updated fields in its `Result`:
        - removed `currency` field
        - renamed fields:
            - `payerAccount` to `payerAccountNumber`
            - `referenceNumber` to `reference`
            - `slipID` to `slipId`
    - removed `MBSerbiaIdFrontRecognizer`, `MBSerbiaIdBackRecognizer` and `MBSerbiaCombinedRecognizer`
    - fields that are **not** deprecated anymore:
        - Sweden DL - reference number
        - Ireland DL - driver number
        - Malaysia iKad - passport number
        - Hong Kong ID - commercial code
    - deprecated the following methods in `MBUsdlRecognizerResult` and `MBUsdlCombinedRecognizerResult`: (they have been replaced with new getters):
        - getField(UsdlKeys)
        - optionalElements
    - added new getters to following results:
        - `MBUsdlRecognizerResult` and `MBUsdlCombinedRecognizerResult`:
            - `firstName`
            - `lastName`
            - `fullName`
            - `address`
            - `documentNumber`
            - `sex`
            - `restrictions`
            - `endorsements`
            - `vehicleClass`
            - `dateOfBirth`
            - `dateOfIssue`
            - `dateOfExpiry`
        - `MBMrzResult`:
           - `sanitizedOpt1`
           - `sanitizedOpt2`
           - `sanitizedNationality`
           - `sanitizedIssuer`
    - renamed methods in the following recognizers and its results:
        - `MBCzechiaCombinedRecognizer`:
            - `lastName` to `surname`
            - `firstName` to `givenNames`
            - `identityCardNumber` to `documentNumber`
            - `address` to `permanentStay`
            - `issuingAuthority` to `authority`
            - `personalIdentificationNumber` to `personalNumber`
        - `MBGermanyCombinedRecognizer`:
            - `lastName` to `surname`
            - `firstName` to `givenNames`
            - `identityCardNumber` to `documentNumber`
            - `issuingAuthority` to `authority`
            - `eyeColor` to `colourOfEyes`
        - `MBJordanCombinedRecognizer`:
            - `issuer` to `issuedBy`
        - `MBPolandCombinedRecognizer`:
            - `issuer` to `issuedBy`
        - `MBRomaniaIdFrontRecognizer`:
           - `lastName` to `surname`
           - `cardNumber` to `documentNumber` from `MrzResult`
           - `parentNames` to `parentName`
           - `nonMRZNationality` to `nationality`
           - `nonMRZSex` to `sex`
           - `validFrom` to `dateOfIssue`
           - `validUntil` to `dateOfExpiry`
           - removed field `idSeries`
           - removed field `cnp`
           - MRZ fields are available through `MBMrzResult` which can be obtained by using `mrzResult` property
        - `MBSlovakiaCombinedRecognizer`:
           - `issuingAuthority` to `issuedBy`
           - `personalIdentificationNumber` to `personalNumber`
        - `MBSloveniaIdFrontRecognizer`:
           - `lastName` to `surname`
           - `firstName` to `givenNames`
        - `MBSloveniaIdBackRecognizer`:
           - `authority` to `administrativeUnit`
           - MRZ fields are available through `MBMrzResult` which can be obtained by using `mrzResult` property
        - `MBSloveniaCombinedRecognizer`:
           - `lastName` to `surname`
           - `firstName` to `givenNames`
           - `identityCardNumber` to `documentNumber`
           - `citizenship` to `nationality`
           - `issuingAuthority` to `administrativeUnit`
           - `personalIdentificationNumber` to `pin`
    - `MBMrtdRecognizer` and `MBMrtdCombinedRecognizer` do not return MRZ image any more
    - `MBMrtdCombinedRecognizer` does not have glare detection options (it does not detect glare anymore)
    - replaced `MBPaymentCardFrontRecognizer`, `MBPaymentCardBackRecognizer` and `MBPaymentCardCombinedRecognizer` with single recognizer - `MBBlinkCardRecognizer`
    - replaced `MBElitePaymentCardFrontRecognizer`, `MBElitePaymentCardBackRecognizer` and `MBElitePaymentCardCombinedRecognizer` with single recognizer - `MBBlinkCardEliteRecognizer`

## 7.2.0

- Minor API changes
    - renamed properties in `MBMalaysiaDlFrontRecognizerResult`:
        - `state` to `ownerState`
        - `zipCode` to `zipcode`
    - renamed properties in `MBIndonesiaIdFrontRecognizerResult`:
        - `validUntil` to `dateOfExpiry`
        - `validUntilPermanent` to `dateOfExpiryPermanent`
    - renamed property in `MBSingaporeIdFrontRecognizerResult`:
        - `bloodType` to `bloodGroup`
    - renamed property in `MBSingaporeCombinedRecognizerREsult`:
        - `bloodType` to `bloodGroup`
    - renamed `MBMyTenteraRecognizer` to `MBMalaysiaMyTenteraFrontRecognizer`
    - renamed `MBMyTenteraRecognizerResult` to `MBMalaysiaMyTenteraFrontRecognizerResult` and properties
        - `nricNumber` to `nric`
        - `ownerAddress` to `fullAddress`
        - `ownerAddressCity` to `city`
        - `ownerAddressState` to `ownerState`
        - `ownerAddressZipCode` to `zipcode`
        - `ownerAddressStreet` to `street`
        - `ownerBirthDate` to `birthDate` and it is now of type `MBDateResult`
        - `ownerFullName` to `fullName`
        - `ownerReligion` to `religion`
        - `ownerSex` to `sex`
    - renamed properties in `MBGermanyIdFrontRecognizerResult`
        - `firstName` to `givenNames`
        - `lastName` to `surname`
        - `dateOfBirth` adn `dateOfExpiry` are now of type `MBDateResult`
    - renamed `MBIkadRecognizer` to `MBMalaysiaIkadFrontRecognizer` and  methods in recognizer and its `Result`:
        - `expiryDate` to `dateOfExpiry `
        - `sex ` to `gender`
    - renamed `MBMyKadFrontRecogniezer` to `MBMalaysiaMyKadFrontRecognizer` and  methods in recognizer and its `Result`:
        - `ownerFullName ` to `fullName`
        - `ownerAddress ` to `fullAddress`
        - `addressStreet ` to `street`
        - `ownerAddressZipCode ` to `zipcode`
        - `ownerAddressCity ` to `city `
        - `ownerAddressState ` to `ownerState`
        - `ownerBirthDate ` to `birthDate`
        - `ownerSex ` to `sex`
        - `ownerReligion ` to `religion`
        - `nricNumber ` to `nric`
    - `MBMalaysiaMyKadFrontRecognizer` does not extract `armyNumber` anymore, use `MBMalaysiaMyTenteraFrontRecognizer` for scanning `MyTentera`
    - `MBMrtdRecognizer`: 
        - method `saveImageDPI` which has been used to set DPI for full document and MRZ image is replaced with methods `fullDocumentImageDpi` and `mrzImageDpi`
    - renamed methods in `MBSwitzerlandIdBackRecognizer` and its `Result`: 
        - `nonMrzDateOfExpiry` to `dateOfExpiry`
        - `nonMrzSex` to `sex`
    - renamed methods in `MBSwitzerlandPassportRecognizer` and its `Result`:
        - `placeOfBirth` to `placeOfOrigin`
        - `nonMrzDateOfBirth` to `dateOfBirth`
        - `nonMrzDateOfExpiry` to `dateOfExpiry`
        - `nonMrzSex` to `sex`
    - removed `sex` and `signatureImage` properties from `MBMalaysiaMyKadBackRecognizer`
    - renamed properties in `MBCroatiaCombinedRecognizerResult`:
        - `identityCardNumber` to `documentNumber`
        - `address` to `residence`
        - `issuingAuthority` to `issuedBy`
        - `personalIdentificationNumber` to `oib`
        - `nonResident` to `documentForNonResident`
    - removed `mrzImage` from `MBMrtdCombinedRecognizer` and `MBMrtdCombinedRecognizerResult`
    - renamed properties in `MBAustraliaDlFrontRecognizerResult`:
        - `name` to `fullName`
        - `dateOfExpiry` to `licenceExpiry`
    - renamed `eyeColour` to `colourOfEyes` in `MBGermanyIdBackRecognizerResult`
    - recognizers that are deprecated:
        - `MBSerbiaIdBackRecognizer` and `MBSerbiaIdBackRecognizerResult`
        - `MBSerbiaIdFrontRecognizer` and `MBSerbiaIdFrontRecognizerResult`
        - `MBSerbiaCombinedRecognizer` and `MBSerbiaCombinedRecognizerResult`
    - all properties that are deprecated for recognizers:
        - `MBHongKongIdFrontRecognizerResult`:
            - `commercialCode`
        - `MBIndonesiaIdFrontRecognizerResult`:
            - `bloodType`
            - `district`
            - `kelDesa`
            - `rt`
            - `rw`
        - `MBNewZealandDlFrontRecognizerResult`:
            - `donorIndicator`
            - `cardVersion`
        - `MBMalaysiaMyKadBackRecognizerResult`:
            - `extendedNric`
        - `MBMexicoVoterIdFrontRecognizerResult`:
            - `electorKey`
        - `MBIrelandDlFrontRecognizerResult`:
            - `driverNumber`
        - `MBSwedenDlFrontRecognizerResult`:
            - `referenceNumber`
        - `MBMalaysiaIkadFrontRecognizerResult`:
            - `passportNumber`    
        - `MBAustriaIdBackRecognizerResult`:
            - `principalResidence`
            - `height`
            - `eyeColour`
        - `MBAustriaPassportRecognizerResult`:
            - `height`
         - `MBGermanyIdBackRecognizerResult`:
            - `colourOfEyes`
            - `height`
        - `MBSwitzerlandIdBackRecognizerResult`:
            - `height`
        - `MBSwitzerlandPassportRecognizerResult`:
            - `height`
         - `MBSingaporeIdBackRecognizerResult`:
            - `bloodGroup`
        - `MBColombiaIdBackRecognizerResult`:
            - `bloodGroup`
        - `MBSwitzerlandPassportRecognizerResult`:
            - `height`
        - `MBPolandIdFrontRecognizerResult`:
            - `familyName`
            - `parentsGivenNames`
        - `MBMoroccoIdBackRecognizerResult`:
            - `fathersName`
            - `mothersName`
        - `MBRomaniaIdFrontRecognizerResult`:
            - `parentNames`

## 7.1.2

- No changes

## 7.1.1

- No changes

## 7.1.0

- Minor API changes
    - Renamed properties in `MBCroatiaIdBackRecognizerResult`:
        - `address` to `residence`
        - `documentForNonResident` to `isDocumentForNonResident`
        - `issuingAuthority` to `issuedBy`
        - MRZ fields are available through `MBMrzResult` which can be obtained by using property `mrzResult`
    - Renamed properties in `MBSingaporeIdFrontRecognizerResult`:
        - `cardNumber` to `identityCardNumber`
    - Renamed properties in `MBSingaporeCombinedRecognizerResult`:
        - `cardNumber` to `identityCardNumber`
        - `bloodGroup` to `bloodType`
    - Renamed properties in `MBCroatiaIdFrontRecognizerResult`:
        - `identityCardNumber` to `documentNumber`
    - Renamed properties in `MBMalaysiaDlFrontRecognizer`:
        - `state` to `ownerState`
        - `zipCode` to `zipcode`
    - Renamed properties in `MBIndonesiaIdFrontRecognizer`:
        - `validUntil` to `dateOfExpiry`
        - `validUntilPermanent` to `dateOfExpiryPermanent`
    - `isScanningUnsupportedForCameraType:` is now class method of `MBMicroblinkSDK` 

## 7.0.0
Please check [README](README.md) and updated demo applications for more information, but the gist of it is:
    - `PPScanningViewController` has been renamed to `MBRecognizerRunnerViewController` and `MBCoordinator` to `MBRecognizerRunner`
    - `PPBarcodeOverlayViewController` has been renamed to `MBBarcodeOverlayViewController`
    - previously internal `MBRecognizer` objects are not internal anymore - instead of having opaque `MBRecognizerSettings` and `MBRecognizerResult` objects, you now have stateful `MBRecognizer` object that contains its `MBResult` within and mutates it while performing recognition. For more information, see [README](README.md) and updated demo applications
    - introduced `MBFieldByFieldOverlayViewController` that can be used for easy integration of the _field-by-field scanning_ feature (previously known as _segment scan_)
    - introduced `MBDocumentVerificationController` that can be used for easy integration of _ID verification scanning_ feature (previously available only in [BlinkID AppStore app](https://itunes.apple.com/us/app/blinkid/id1258136557?mt=8)
    - introduced `MBProcessor` concept. For more information, check updated code samples, [README](README.md) and [this blog post](https://microblink.com/blog/major-change-of-the-api-and-in-the-license-key-formats)
- new licence format, which is not backward compatible. Full details are given in [README](README.md) and in updated applications, but the gist of it is:
    - licence can now be provided with either file, byte array or base64-encoded bytes

## Transition to 5.15.0

- No changes


## Transition to 5.14.1

- No changes

## Transition to 5.14.0

- No changes

## Transition to 5.13.0

- Renamed `PPMyKadRecognizerSettings` and `PPMyKadRecognizerResult` to `PPMyKadFrontRecognizerSettings` and `PPMyKadFrontRecognizerResult`

## Transition to 5.12.1

- No changes

## Transition to 5.12.0

- Removed `imageDPI` property on `PPTemplatingRecognizerSettings`  

## Transition to 5.11.0

- `PPDocumentDetectorResult` does not contain information about screen orientation any more

## Transition to 5.10.2

- No changes

## Transition to 5.10.1

- There is no more option in PPUsdlRecognizerSettings to scan 1D barcodes. Previously this setting did nothing - it's OK to just delete the setter call if you use it.

## Transition to 5.10.0

- `PPBlinkOcrRecognizerResult` and `PPBlinkOcrRecognizerSettings` are now deprecated. Use `PPDetectorRecognizerResult` and `PPDetectorRecognizerSettings` for templating or `PPBlinkInputRecognizerResult` and `PPBlinkInputRecognizerSettings` for segment scan
- `PPSerbianPdf417RecognizerResult` and `PPSerbianPdf417RecognizerSettings` are renamed to `PPSerbianBarcodeRecognizerResult` and `PPSerbianBarcodeRecognizerSettings`

## Transition to 5.9.4

- `extractAddress` property in `PPSlovakIDBackRecognizerSettings` is now removed since previously wasn't use

## Transition to 5.9.3

- `PPAztecRecognizerResult` and `PPAztecRecognizerSettings` are now deprecated. Use `PPBarcodeRecognizerResult` and `PPBarcodeRecognizerSettings`
- `PPBarDecoderRecognizerResult` and `PPBarDecoderRecognizerSettings` are now deprecated. Use `PPBarcodeRecognizerResult` and `PPBarcodeRecognizerSettings`
- `PPZXingRecognizerResult` and `PPZXingRecognizerSettings` are now deprecated. Use `PPBarcodeRecognizerResult` and `PPBarcodeRecognizerSettings`

## Transition to 5.9.2

- No changes

## Transition to 5.9.1

- No changes

## Transition to 5.9.0

- Since Microblink.framework is a dynamic framework, you also need to **add it to embedded binaries section in General settings of your target.**

- Library size was reduced by removing all unnecessary components. One of the components removed was internal libz library. You now need to **add libz.tbd into "Linked frameworks and binaries" setting.**

- Microblink.framework is a dynamic framework which contains slices for all architectures - device and simulator. If you intend to extract .ipa file for ad hoc distribution, you'll need to preprocess the framework to remove simulator architectures. 

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

- Deprecated `PPHelpDisplayMode`. It still works, but ideally, you should replace it with a custom logic for presenting help inside the application using the SDK.
- Simplified `PPOcrLayout` class (removed properties which were not used). If you used it previously, simply remove that code because it does not provide any value.
- If you're using PPGermanIDMrzRecognizer, it's functionality is now split into two recognizers:
    - one for back side of the new ID (PPGermanIDBackRecognizer)
    - one for front side of the old ID (PPGermanOldIDRecognizer)

## Transition to 5.8.0

- No changes

## Transition to 5.7.2

- No changes

## Transition to 5.7.1

- If you used `payerAddress` and `recipientAddress` properties in `PPSloQrCodeRecognizerResult`, those properties are now split into `payerStreet`, `payerPlace`, `recipientStreet` and `recipientPlace`. 
- If you used `accountNumber` property in `PPSkSlipRecognizerResult`, this property can now be replaced by `iban` property

## Transition to 5.7.0

- If you're using PPSingaporeIdRecognizer, you should now decide which side of the ID you want to use, and use either PPSingaporeIDBackRecognizer, or PPSingaporeIDFrontRecognizer
- `PPMobileCouponsOcrParserFactory` changed name to `PPTopUpOcrParserFactory`

## Transition to 5.6.0

- No changes

## Transition to 5.5.0

- No changes

## Transition to 5.4.1

- No changes

## Transition to 5.4.0
- `PPMrtdRecognizerResult` now returns `NSDate` in methods `dateOfBirth` and `dateOfExpiry`. Previously `NSString` was returned and user had to parse the string to get the date. If you want old behaviour, use methods `rawDateOfBirth` and `rawDateOfExpiry` which will return strings in same format as in previous versions.
- this also applies for all recognizer results that inherit `PPMrtdRecognizerResult`
- although `PPDateOcrParser` now returns `PPDateResult` object (which contains both `NSDate` and original `NSString` from which date was parsed), when obtaining parser result via `parserResultForName:` or `parserResultForName:parserGroup:` methods, you will be provided with string just like in previous versions. If you want `NSDate`, you should use methods `specificParsedResultForName:` or `specificParsedResultForName:parserGroup:` and cast `NSObject` they return into `NSDate`.
- all recognizer results (classes that derive `PPRecognizerResult`) now have annotated nullability for their getters. Some of them used to assume non-null, while still returning `nil` sometimes. This has now been corrected and all getters are `_Nullable`


## Transition to 5.3.0
- No changes

## Transition to 5.2.0

- No changes

## Transition to 5.1.0

- No changes

## Transition to 5.0.3

- No changes

## Transition to 5.0.2

- No changes

## Transition to 5.0.1

- No changes

## Transition to 5.0.0

- `PPCameraCoordinator` now assumes the role of `PPCoordinator`. If you do not use your own camera management or Direct API you can rename all instances of `PPCoordinator` to `PPCameraCoordinator`
- `PPCoordinator` method `cameraViewControllerWithDelegate:` has been removed. To create `PPScanningViewControllers` you can now use `[PPViewControllerFactory cameraViewControllerWithDelegate: coordinator: error:]`
- Direct API is now located in `PPCoordinator`. To process image use 'processImage:' method and be sure to set 'PPCoordinatorDelegate' when creating 'PPCoordinator' to recieve scanning results and events. You can se processing image roi and processing orientation on 'PPImage' object.
- Methods of 'PPOverlayContainerViewController' protocol should now be called after camera view has appeared.
- Removed PPOcrEngineOptions property from PPRegexOcrParserFactory and PPRawOcrParserFactory. Replaced property with setter method.

## Transition to 4.6.4

- No changes

## Transition to 4.6.2

- Renamed `PPOcrRecognizerSettings` and `PPOcrRecognizerResult` to `PPBlinkOcrRecognizerSettings` and `PPBlinkOcrRecognizerResult` respectively

- Changed interface of some Detector API methods

- `PPDocumentDecodingInfoEntry` renamed to `PPDecodingInfo`
- Removed `PPDocumentDecodingInfo`. All occurances need to be replaced with `NSArray<PPDecodingInfo>`

## Transition to 4.6.1

- If you implement custom camera UI and handle `cameraViewController:didFindLocation:withStatus`, this method was changed to `cameraViewController:didFinishDetectionWithResult:`. `PPDetectorResult` object now contains all information previosusly passed to this method. Simply update the code to use the new method signature. Verify the exact type of the passed detectorResult object, cast it to this class, and use provided getters to obtain all information.

### Transition to 4.6.0

- PPOverlayViewController changed the way Overlay Subviews are added to the view hierarchy. Instead of calling `addOverlaySubview:` (which automatically added a view to view hierarachy), you now need to call `registerOverlaySubview:` (which registers subview for scanning events), and manually add subview to view hierarchy using `addSubview:` method. This change gives you more flexibility for adding views and managing autolayout and autoresizing masks. So, replace all calls to (assuming self is a `PPOverlayViewController` instance)

```objective-c
[self addOverlaySubview:subview];
```

with 
```objective-c
[self registerOverlaySubview:subview];
[self.view addSubview:subview];
```
- Localization Macros MB_LOCALIZED and MB_LOCALIZED_FORMAT can now be overriden in your app to provide completely custom localization mechanisms.

### Transition to 4.5.1

- No changes

### Transition to 4.5.0

- Remove MicroBlinkResources.bundle and add MicroBlink.bundle.

- If you use `PPMetadataSettings` objects, 
    - `successfulScanFrame` property replace with `successfulFrame`
    - `currentVideoFrame` property replace with `currentFrame`

### Transition to 4.4.0

- Remove the old .embeddedframework package completely from your project

- Add new .framework and .bundle package to your project. Verify that Framework search path really contains a path to the .framework folder.

- If necessary, add all required system frameworks and libraries:

    - libc++.tbd
    - libiconv.tbd
    - AVFoundation.framework
    - AudioToolbox.framework
    - CoreMedia.framework
    - AssetsLibrary.framework
    - Accelerate.framework

- If you previously used built-in onboarding (help) screens, since they are moved outside the SDK, you need to re-add it to your project. You can do that by overriding the method :

```objective-c
- (void)scanningViewControllerWillPresentHelp:(UIViewController<PPScanningViewController> *)scanningViewController
```

In the implementation, presenting the Onboarding screens. Default implementation which is provided in the Sample app for your country / use case. We recommend you use provided help screens - just drag & drop the classes, storyboard and xcassets files in your project.

### Transition to 4.3.0

- You can now use bitcode = YES in your build settings.

### Transition to 4.2.0

- PhotoPayHelpViewController.nib resource was renamed to PhotoPayHelpViewController.xib. This allows it to be open with new Xcode versions (Xcode 7)

### Transition to 4.1.0

- `rotatedImage` property of `PPImageMetadata` no longer exists. Use `image` property instead, it correctly handles rotation.

- The ability to use native iOS orientation handling features is now added. `PPScanningViewController` now has properties `autorotate` and `supportedOrientations`. ScanningViewController uses these properties in the implementation of standard iOS rotation handling methods: `shouldAutorotate` and `supportedInterfaceOrientations`.

If you use autorotation with the new features, make sure you disable autorotation of the overlay using `settings.uiSettings.autorotateOverlay = NO`

### Transition to 4.0.3

- No changes

### Transition to 4.0.2

- No changes

### Transition to 4.0.1

- This version uses a new license key format. If you had a license key generated prior to v4.0.1, contact us so we can regenerate the license key for you.

- `PPCoordinator` class now exposes fewer public methods and properties.

- `PPScanningViewController`'s methods `resumeScanning` and `resumeScanningWithoutStateReset` merged into one `resumeScanningAndResetState:`. 
        - All calls to `resumeScanning` replace with `resumeScanningAndResetState:YES`.
        - All calls to `resumeScanningWithoutStateReset` replace with `resumeScanningAndResetState:NO`

- Classes representing scanning results were renamed. Renaming was performed to match naming convention of `PPRecognizerSettings` hierarcy: now each `PPRecognizerSettings` class has it's matching `PPRecognizerResult`. Replace all existing references to old class names with the new ones:

	- `PPBaseResult` was renamed to `PPRecognizerResult`. 

	- `PPOcrScanResult` was renamed to `PPOcrRecognizerResult`, this is the result of a recognizer whose settings are given with `PPOcrRecognizerSettings.`

	- `PPCroResult` renamed to `PPCroRecognizerResult`. This is a common superclass to `PPCroSlipRecognizerResult`, `PPCroPdf417RecognizerResult` and `PPCroQrRecognizerResult`.

	- `PPCroSlipResult` renamed to `PPCroSlipRecognizerResult`, this is the result of a recognizer whose settings are given with `PPCroSlipRecognizerSettings.`

	- `PPCroBarcodeResult` renamed to `PPCroPdf417RecognizerResult`, this is the result of a recognizer whose settings are given with `PPCroPdf417RecognizerSettings.`

	- Introduced `PPCroQrRecognizerResult`, this is the result of a recognizer whose settings are given with `PPCroQrRecognizerSettings.`
	
- `PPOcrResult` (class representing a result of the OCR process, with individual characters, lines and blocks), was renamed to `PPOcrLayout`. Name change was introduced to further distinguish the class from `PPRecognizerResult` classes.

- Remove all references to `updateScanningRegion` method since it's now being called automatically in `setScanningRegion setter`.

### Transition to 4.0.0

- Framework was renamed to MicroBlink.embeddedframework. Remove the existing .embeddedframwork package from your project, and drag&drop MicroBlink.embeddedframework in the project explored of your Xcode project.

- If necessary, after the update, modify your framework search path so that it points to the  MicroBlink.embeddedframework folder.

- Main header of the framework was renamed to `<MicroBlink/Microblink.h>`. Change all references to previous header with the new one.

- method `[PPCoordinator isPhotoPaySupported]` was renamed to `[PPCoordinator isScanningSupported]`. Change all occurances of the method name.

- Remove all references to `updateScanningRegion` method since it's now being called automatically in `setScanningRegion setter`.

- Retrieving results has changed. Consult guides in Docs/ especially those with prefix 01.

