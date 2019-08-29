# Release notes

## 7.5.0

- Updates and additions:
    - added `MBBlinkIdRecognizer` for scanning front side of ID cards and `MBBlinkIdCombinedRecognizer` for combined scanning of front and back side of ID cards
        - for now, these recognizers classify and extract data from **87** different classes of **United States driver's licenses** (front and back side)
        - in the upcoming releases, we are planning to add support for more document types from different countries
    - completely new UX for scanning ID cards with scan overlay view controller: `MBBlinkIdOverlayViewController`:
        -  best suited for scanning with `MBBlinkIdRecognizer` and `MBBlinkIdCombinedRecognizer`
        - other single side and combined document recognizers are also supported
    - added support for reading back side of Nigerian Voter ID card - use `MBNigeriaVoterIdBackRecognizer`
    - added support for reading front and back side of Belgium ID - use `MBBelgiumIdFrontRecognizer`, `MBBelgiumIdBackRecognizer` and `MBelgiumCombinedRecognizer`
    - added support for reading all visa documents containing Machine Readable Zone - use `MBVisaRecognizer`

- Improvements in ID scanning performance:
    - improved `MBRomaniaIdFrontRecognizer`
        - now extracts `CNP` number
    - improved `MBSloveniaIdFrontRecognizer` and `MBloveniaCombinedRecognizer`:
        - return boolean flag which indicates whether **date of expiry** is permanent - use `dateOfExpiryPermanent`
    - improved `MBGermanyPassportRecognizer`:
        - better passport classification
    - improved `MBColombiaIdFrontRecognizer`:
        - support for document number in format 1-3-3
    - improved `MBSlovakiaIdFrontRecognizer`:
        - support for German letters
    - Malaysia:
        - `MBMalaysiaMyTenteraFrontRecognizer` supports 6-digit army number
        - `MBMalaysiaIkadFrontRecognizer` - better extraction of the following fields (DeepOCR support): date of birth, sector, employer, address and date of expiry
    - United Arab Emirates:
        - glare detection is disabled by default for `MBUnitedArabEmiratesIdFrontRecognizer` and `MBUnitedArabEmiratesIdBackRecognizer` 
        - `MBUnitedArabEmiratesIdBackRecognizer` - optimized detection for black backgrounds
        - improved `MBMrtdRecognizer`: 
        - added support for documents with non-binary gender specification (symbol X)
    - improved `MBDocumentFaceRecognizer`:
        - improved scanning time (faster scan)
        - added support for vertical IDs
        - removed the `tryBothOrientations` option (improved scan in all directions is enabled by default)
    - improved scanning time (faster scan) for `MBPassportRecognizer`

- Minor API changes:
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

- Bugfixes:
    - fixed bug in `MBDocumentFaceRecognizer` which caused that DPI settings has not been applied to dewarped image
    - fixed bug in `MBBlinkCardOverlayViewController` which caused memory issues
    - fixed bug in `MBSloveniaQrCodeRecognizer` which caused invalid parsing

## 7.4.0

**This release fixes issues with Xcode 10.2**

- Updates and additions:
    - added support for reading all passports with MRZ - use `MBPassportRecognizer`
    - added setting on `MBDocumentFaceRecognizer` for control over face image processor - use `tryBothOrientations`
    - added result property on `MBGermanyCombinedRecognizerResult` to get full mrz string result - use `rawMrzString`

- Improvements in ID scanning performance:
    - added support for reading commercial code in two rows for `MBHongKongIdFrontRecognizer`
    - added support for `MBHongKongIdFrontRecognizer` 2018 version
    - improved reading accuracy for the following recognizers (**DeepOCR** support):
    - `MBMalaysiaIKadFrontRecognizer`
    - improved scanning time of all Malaysian ID front recognizers: MyKad, MyKAS, MyPR, MyTentera

- Minor API changes:
    - `partialRecognitionTimeout` in `MBRecognizerCollection` default value has been changed to **0** which means no timeout will be reported in which partial scanning results will be returned to the user

- Bugfixes:
    - fixed issue with combining surnames in `MBGermanyCombinedRecognizer`'s logic
    - fixed a validation issue for the gender field in `MBSloveniaCombinedRecognizer`
    - fixed DPI options on images are now correctly applied to dewarped image results in `MBDocumentFaceRecognizer`
    - fixed bug in `MBDocumentFaceRecognizer` which caused that DPI settings has not been applied to dewarped image
    - fixed bug in `MBBlinkCardOverlayViewController` which caused memory issues



## 7.3.0

**Important notice on MRTD recognizer in the latest PhotoPay SDK release (v7.3.0)**

Please note that we have significantly improved accuracy for MRZ/MRTD scanning because now we switched to the newest OCR technology based on machine learning.
To be more precise, we measured and compared existing vs. new MRTD scanning. The new OCR system based on machine learning achieves 99.9% accuracy on the character level, which results with a 50% reduction in the error rate in MRZ extraction.

In order to use new *MrtdRecognizer* or *MrtdCombinedRecognizer* or to continue using any additional *Recognizer for scanning any ID with the MRZ (machine readable zone)* within the latest PhotoPay SDK update, you *must* have a new license key. Before updating to the SDK version 7.3.0, please contact your account manager or send an email to support@microblink.com to obtain the *new production license key*.

**Important notes**:

- The MRTD scanning with the older PhotoPay SDK versions (v7.2.0 and below) will continue to work without any problems - until you decide to update.
- If you upgrade to the SDK version 7.3.0 without a new license key scanning of MRTD/MRZ documents will not work.
- Contact us at support@microblink.com to obtain a new license key if you plan to update your app with the latest release.

For any questions, you might have, we stand at your service.

- Updated and additions
    - added support for reading front and back side of Brunei Military ID - use `MBBruneiMilitaryIdFrontRecognizer` and `MBBruneiMilitaryIdBackRecognizer`
    - added support for reading front and back side of Brunei Temporary Residence Permit - use `MBBruneiTemporaryResidencePermitFrontRecognizer` and `MBBruneiTemporaryResidencePermitBackRecognizer`
    - added `MBBlinkCardOverlayViewController` to be used with BlinkCard recognizers

- Improvements in ID scanning performance
    - improved reading accuracy for all MRZ recognizers
    - enabled reading year-only dates of birth on **Kuwait IDs**
    - improved `MBSingaporeIdBackRecognizer`:
        - better reading of documents with sticker
    - improved `MBMrtdRecognizer`:
        - added `allowSpecialCharacters` option which is required for parsing Malaysian Passport IMM13P MRZ type
    - all recognizers now reset their results on shake, except Combined recognizers
    - `MBBlinkCardRecognizer` returns card issuer

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

- Bugfixes
    - fixed bug in `MBSlovakiaQrCodeRecognizer`
    - fixed bug in `MBIbanParser` for **Romanian IBANs**
    - `MBMrtdRecognizer` result state is now properly invalidated after detection fails
    - templating recognizers no longer execute callbacks with `valid` state once they are `valid` on every frame even if nothing is 'detected'
    - various other bug fixes and improvements

## 7.2.0

- Updated and additions
    - Added support for reading front side of Ireland Driver's License  - use `MBIrelandDlFrontRecognizer`
    - Added support for reading front side of Colombia Driver's License - use `MBColombiaDlFrontRecognizer`
    - Added support for reading front side of Italy Driver's License - use `MBItalyDlFrontRecognizer`
    - Added standalone recognizer for reading front side of Austria Driver's License - use `MBAustriaDlFrontRecognizer`
    - Added support for reading front and back side of elite Payment / Debit cards - use `MBElitePaymentCardFrontRecognizer`, `MBElitePaymentCardBackRecognizer` and `MBElitePaymentCardCombinedRecognizer`
    - Added support for reading back side of German Driver's License with B10 support - use `MBGermanyDlBackRecognizer`
    - Added support for reading front side of Mexican Voter Id card - use `MBMexicoVoterIdFrontRecognizer`
    - Added support for reading  ExpiresOn date on `MBCyprusIdBackRecognizer`
    - Added support for image(s) anonymization on `MBPaymentCardFrontRecognizer`
        - use `anonymizeCardNumber` and `anonymizeOwner`
    - Added support for image(s) anonymization on `MBPaymentCardBackRecognizer`
        - use `anonymizeCvv`
    - Added support for image(s) anonymization on `MBPaymentCardCombinedRecognizer`
        - use `anonymizeCardNumber`, `anonymizeOwner` and `anonymizeCvv`
    - Added support for image(s) anonymization on `MBElitePaymentCardFrontRecognizer`
        - use `anonymizeOwner`
    - Added support for image(s) anonymization on `MBElitePaymentCardBackRecognizer`
        - use `anonymizeCvv` and `anonymizeCardNumber`
    - Added support for image(s) anonymization on `MBElitePaymentCardCombinedRecognizer`
        - use `anonymizeCardNumber`, `anonymizeOwner` and `anonymizeCvv`
    - Added support for full document image extension factors on `MBUsdlCombinedRecognizer`
    - Added support for reading front side of Brunei ID - use `MBBruneiIdFrontRecognizer`
    - Added support for reading front and back side of Cyprus ID, issued after 2015.  - use `MBCyprusIdFrontRecognizer` and `MByprusIdBackRecognizer`
    - Added support for reading front side of Malaysian MyKAS - use `MBMalaysiaMyKasFrontRecognizer`
    - Added support for reading front side of Malaysian MyPR - use `MBMalaysiaMyPrFrontRecognizer`
    - Enabled capturing high resolution camera frames:
        - When custom UI integration is performed, use `- (void)captureHighResImage:(MBCaptureHighResImage)highResoulutionImageCaptured` on `MBRecognizerRunnerViewController`
        - When using provided scan overlay view controllers, high resolution full camera frames taken at the moment of successful scan are returned if this option is enabled through `MBOverlaySettings`. Concrete `MBDocumentOverlaySettings` and `MBDocumentVerificationOverlaySettings` have property `captureHighResImage` to support this feature and new optional delegate on respective delegates
    - Added support for reading front side of German Driver's License - use `MBGermanyDlFrontRecognizer`
    - Added support for reading back side of Brunei ID - use `MBBruneiIdBackRecognizer`
    - Added support for reading front side of Brunei Residence Permit - use `MBBruneiResidencePermitFrontRecognizer`
    - Added support for reading back side of Brunei Residence Permit - use `MBBruneiResidencePermitBackRecognizer`
    - Updated overlay view controllers with new icons for `close` and `torch` buttons

- Improvements in ID scanning performance
    - improved `MBMrtdCombinedRecognizer`:
        - added option to allow unparsed and unverified MRZ results - use `allowUnparsedResults` and `allowUnverifiedResults`
    - improved `MBMalaysiaDlFrontRecognizer`:
        - added support for reading Malaysia Dl for foreigners 
    - improved `MBUsdlRecogniezr`:
        - added support for reading dates on Nigerian Driver's licenses
    - added support for setting full document image extension factors for almost all ID document recognizers, they implement interface `MBFullDocumentImageExtensionFactors`
    - added support for setting the number of stable detections threshold on `MBDocumentFaceRecognizer` and recognizers which use it internally: `MBMrtdCombinedRecognizer` and `MBUsdlCombinedRecognizer` - use `numStableDetectionsThreshold`. This can help to avoid returning of blurry images.
    - improved `MBEudlRecognizer`:
        - better reading accuracy for UK Driver's license
    - moved these recognizers to DeepOCR engine (improved reading accuracy): `MBSingaporeIdFrontRecognizer`, `MBSingaporeIdBackRecognizer`, `MBCroatiaIdFrontRecognizer`,  `MBCroatiaIdBackRecognizer`
    - improved DeepOCR accuracy
    - improved reading of Swiss front side ID cards
    - improved reading of German front side ID cards
    - improved `MBMalaysiaMyTenteraFrontRecognizer` with DeepOcr support
    - improved reading of Singapore front side Driver's Licenses with DeepOcr support
    - improved reading of Croatian front side ID cards
    - improved personal number extraction on Slovakian ID cards
    - improved reading of Indonesian front side ID cards with DeepOcr support
    - updated image return processor 
        - the processor now estimates detected (dewarped) document image quality and returns the best quality dewarped image from the best quality detection
    - improved reading accuracy for the following recognizers (**DeepOCR** support):
        - `MBHongKongIdFrontRecognizer`
        - `MBMalaysiaMyKadFrontRecognizer`
        - `MBMalaysiaMyKadBackRecognizer`
        - `MBMalaysiaMyTenteraFrontRecognizer`
        - `MBMalaysiaDlFrontRecognizer`
        - `MBNewZealandDlFrontRecognizer`
        - `MBMalaysiaMyKadBackRecognizer`
    - improved `MBPaymentCard` recognizers:
        - better OCR and data extraction
        - added support for reading payment card numbers in 4x6x4 and 4x6x5 format
    - improveed UAE recognizers:
        - glare detection is enabled for all images returned from `MBUnitedArabEmiratesDlFrontRecognizer`, `MBUnitedArabEmiratesIdBackRecognizer` and `MBUnitedArabEmiratesIdFrontRecognizer` recognizers
    - improved `MBMrtdRecognizer`:
        - added option to set extension factors for full document image: use method `fullDocumentImageExtensionFactors`
        - added option to encode `fullDocumentImage` and `mrzImage` to JPEG and save them to `MBMrtdRecognizerResult`: use `encodeMrzImage` and `encodeFullDocumentImage` to enable encoding
    - updated `MBSerbiaQrCodeRecognizer` with support for new Serbia QR code standard
        - renamed `MBSerbiaBarcodePaymentResult` result member: `accountNumber` to `recipientAccountNumber`
            - added result members: `identificationCode`, `payerAccountNumber`, `paymentCode`, `merchantCodeCategory`, `oneTimePaymentCode`, `merchantReference` and `rawBarcodeData`
    - improved Swiss payment slip scanning
    - improved QR code scanning for images
    - updated `MBCzechiaQrCodeRecognizer` with support for default currency

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

- Bugfixes
    - enabled wrapping of combined recogniezrs with `MBSuccessFrameGrabberRecognizer`
    - fixed bug in `MBEudlRecognizer` which caused that sometimes face image is not returned, even if the recognition was successful
    - updated overlay view controllers for iPhone X Series
    - various other bug fixes and improvements
    - fix memory issue while using current frame grabber
    - fix UI bug on `MBDocumentVerificationOverlayViewController` - now showing `Document scanning done` when scanning finish
    - all combined recognizers are not optional any more in Swift
    - MBDocumentFaceRecognizer now correctly applies DPI settings to returned face and full document images
    - fixed a crash which happened when scanning region was set before overlay view controller loaded, but after it was initialized
    - fixed missing `init` in `MBDotsResultSubview` for Swift

## 7.1.2

- Updated and additions
    - Added method `photopayOverlayViewControllerDidTapHelp:` on delegate `MBPhotopayOverlayViewControllerDelegate`
    - Added missing support for reading Slovenian QR Payment Codes - use `MBSloveniaQrCodeRecognizer`

- Bugfixes
    - Fixed issue when orientation changed notification recevied before creating preview layer on iOS 12


## 7.1.1

- Updated and additions
    - Added support for hiding `help` button on `MBPhotoPayOverlayViewController` with `displayHelpButton` proeprty on `MBPhotopayOverlaySettings`

## 7.1.0

- Updates and additions
    - Added support for reading front side of Spain Driver's License - use `MBSpainDlFrontRecognizer`
    - Added support for reading front side of UAE Driver's License - use `MBUnitedArabEmiratesDlFrontRecognizer`
    - Added support for reading front side of Cyprus ID card - use `MBCyprusIdFrontRecognizer`
    - Added support for reading back side of Cyprus ID card - use `MBCyprusIdBackRecognizer`
    - Added support for reading front side of Kuwait ID card - use `MBKuwaitIdFrontRecognizer`
    - Added support for reading back side of Kuwait ID card - use `MBKuwaitIdBackRecognizer`
    - Added support for reading front side of Payment Card - use `MBPaymentCardFrontRecognizer`
    - Added support for reading back side of Payment Card - use `MBPaymentCardBackRecognizer`
    - Added support for reading front and back side of Payment Card - use `MBPaymentCardCombinedRecognizer`
    - Added support for optional protocol method implementation in `MBDocumentVerificationOverlayViewControllerDelegate` - `documentVerificationOverlayViewControllerDidFinishScanningFirstSide:`
    - Added support for reading back side of Morocco ID card - use `MBMoroccoIdBackRecognizer`
    - Added support for reading Singapore Changi Employee ID card - use `MBSingaporeChangiEmployeeIdRecognizer`
    - Added support for reading front side of Irish Driver's License - use `MBIrelandDlFrontRecognizer`
    - Added support for reading front side of Colombian Driver's License - use `MBColombiaDlFrontRecognizer`
    - Added support for reading residential status on front side of Hong Kong ID Card
    - Added support for reading partial dates on all MRTD documents
    - Added support for returning encoded images on all recognizers that support image return
    - Added support for checking if scanning is unsupported for camera type on `MBRecognizerRunnerViewController`
    - Added support for reading sticker with new address on back side of Singapore ID card
    - Added missing `oldNric` property on `MBMyKadBackRecognizerResult`
    - Added missing `currencyCode` property on `MBSwitzerlandSlipRecognizerResult`
    - Added `allowUnparsedResults` and `allowUnverifiedResults` properties on `MBMrtdCombinedRecognizer`
    - Added `required` property on `MBParser` which defines whether the parser configured with this parser settings object will be required or optional

- Improvements in ID scanning performance
    - Added support for reading sticker with new address on back side of Singapore ID card with `MBSingaporeCombinedRecognizer`
    - Performance improvements
    - Improved reading of New Zealeand Driver's License
    - Improved reading of Malaysian Driver's License
    - Improved reading of Croatian PDF417 payment codes
    - Better name and nationality extraction on `MBUnitedArabEmiratesIdFrontRecognizer`

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

- Bugfixes
    - Fixed bug where SDK crashed with exception when the user wanted to use custom resource bundle
    - Fixed memory leak when image is created using CMSampleBuffer
    - Fixed signing identifier when creating custom builds
    - Various other bug fixes and improvements

## 7.0.0

- new API, which is not backward compatible. Please check [README](README.md) and updated demo applications for more information, but the gist of it is:
    - `PPScanningViewController` has been renamed to `MBRecognizerRunnerViewController` and `MBCoordinator` to `MBRecognizerRunner`
    - `PPBarcodeOverlayViewController` has been renamed to `MBBarcodeOverlayViewController`
    - previously internal `MBRecognizer` objects are not internal anymore - instead of having opaque `MBRecognizerSettings` and `MBRecognizerResult` objects, you now have stateful `MBRecognizer` object that contains its `MBResult` within and mutates it while performing recognition. For more information, see [README](README.md) and updated demo applications
    - introduced `MBFieldByFieldOverlayViewController` that can be used for easy integration of the _field-by-field scanning_ feature (previously known as _segment scan_)
    - introduced `MBDocumentVerificationController` that can be used for easy integration of _ID verification scanning_ feature (previously available only in [BlinkID AppStore app](https://itunes.apple.com/us/app/blinkid/id1258136557?mt=8)
    - introduced `MBProcessor` concept. For more information, check updated code samples, [README](README.md) and [this blog post](https://microblink.com/blog/major-change-of-the-api-and-in-the-license-key-formats)
- new licence format, which is not backward compatible. Full details are given in [README](README.md) and in updated applications, but the gist of it is:
    - licence can now be provided with either file, byte array or base64-encoded bytes

## 5.15.0
    
- Updates and additions
    - Added support for reading front side of Swedish Driver's License- use `PPSwedenDLFrontRecognizerSettings`
    - Added ability to extend full document cropping zone on `PPGermanIDFrontRecognizerSettings`
    - Added support for CAN number extraction on German ID Front
    - Added support for iKAD MM55 ID's
    
- Improvements in ID scanning performance
    - Improved reading of document number on Hong Kong ID
    - Improvements when returning partial data in Document Face Recognizer
    - Improvements in USDL data parsing
    - when `PPDocumentFaceRecognizer` is activated at the same time with another more specific recognizer(s) (e.g. EUDL Recognizer), preference is given to the more specific recognizer which means that it will get a chance to extract additional data from the concrete document type
    
- Bugfixes
    - Added support for nonstandard pdf417 barcodes  which wrongly encode number of data codewords
    - `coordinatorDidDealloc` method in `PPCoordinatorDelegate` is now correctly called when all resources are released
    - fixed reading of IBAN and BIC numbers in payment CzechQRCodeRecognizer

## 5.14.1

- Bugfixes
    - Fixed errors related to fetching document number from Egyptian ID
    - Fixed errors related to fetching validFrom and validUntil dates from Malaysian DL
    - Fixed errors related to returning and encoding face and full document images when using `PPJordanIDCombinedRecognizerSettings`
    - Fixed dateOfExpiry property type from `NSString` to `NSDate` on  `PPJordanIDCombinedRecognizerSettings`
    - Fixed crashes that happened when trying to activate the torch while video input hasn't loaded
    - Fixed rare OpenGL crash that happened on resigning VC activities
    - Fixed error where app upload to the store would be rejected because of missing bitcode
    - Fixed document number fetching from Egyptian ID result
    - Fixed return types of date field on Malaysian DL

## 5.14.0

- Updates and additions
    - added support for reading front and back side of Jordan ID - use `PPJordanIDFrontRecognizerSettings` and `PPJordanIDBackRecognizerSettings`
    - added Jordan Combined Recognizer - use `PPJordanIDCombinedRecognizerSettings`
    - added support for reading Egyptian ID Front - use `PPEgyptIDFrontRecognizerSettings`
    - added support for reading Malaysian DL Front - use `PPMalaysianDLFrontRecognizerSettings`
    - added support for reading Malaysian Passport IMM13P MRTD - be sure to set `allowSpecialCharacters` to `true` when creating `PPMrtdRecognizerSettings`

- Improvements in ID scanning performance
    - Improved reading Malaysian MyKad and MyTentera
    - Improved reading VINs
    - Improved parsing of USDL

- Bugfixes
    - Fixed reading amount, reference and bic on Slovenian payment slip
    - Fixed expiry date for magnetic stripe USDL subtype - using day of birth not last day of the month for license expiry day
    - Fixed rare crashes that sometimes happened when trying to fetch unparsed dates



## 5.13.0

- Updates and additions
    - added support for reading front side of Hong Kong ID - use `PPHongKongIDFrontRecognizerSettings`
    - added support for reading front and back side of Colombian ID - use `PPColombiaIDFrontRecognizerSettings` and `PPColombiaIDBackRecognizerSettings`
    - added support for reading front and back side of United Arab Emirates ID - use `PPUnitedArabEmiratesIDFrontRecognizerSettings` and `PPUnitedArabEmiratesIDBackRecognizerSettings`
    - added support for reading front side of New Zealand drivers license - use `PPNewZealandDLFrontRecognizerSettings`
    - added support for reading back side of Malaysian MyKad - use `PPMyKadBackRecognizerSettings`
    - added support for reading Malaysian MyTentera documents - use `PPMyTenteraRecognizerSettings`
    - added support for reading Malaysian MyTentera documents with MyKad recognizer - use `PPMyKadFrontRecognizerSettings` and enable reading of army number
    - added support for setting DPI for full document images returned by `PPMyKadFrontRecognizerSettings`, `PPMyKadBackRecognizerSettings`, `PPMyKadFrontRecognizerSettings` and `PPIKadRecognizerSettings`:
    - use `fullDocumentImageDPI` on the corresponding recognizer settings
    - added full support for iPhone X layout for all SDK's overlay views

- Minor API changes
    - renamed `PPMyKadRecognizerSettings` and `PPMyKadRecognizerResult` to `PPMyKadFrontRecognizerSettings` and `PPMyKadFrontRecognizerResult`

- Improvements in ID scanning performance
    - Improved reading of Belgium ID BRZ OPT2 field
    - added support for reading Belgium MRZ with partial date of birth - `PPMrtdRecognizerSettings.allowUnverifiedResults` must be set to `true`
    - added support for reading Kenya MRZ - `PPMrtdRecognizerSettings.allowUnverifiedResults` must be set to `true`
    - improved `MyKadFrontSideRecognizer` and `MyTenteraRecognizer`:
        - better reading of name field
        - better reading of address field
    - improved `PPAustraliaDLFrontRecognizer`:
        - improved reading of names and addresses
        - added support for reading first names with more words
    - improved `PPSingaporeIDFrontRecognizer`:
        - tuned ID card data extraction positions
    - improved Malaysian `IKadRecognizer`:
        - better reading of date of expiry and employer fields

- Bugfixes
    - fixed incorrect setting of missing dates to current date in MRTD recognizers. If date is not present in MRTD, the corresponding getter will now return nil
    - fixed an error where application would sometimes crash because of `PPModernToastOverlaySubview` constraints
	- when setting DPI for full document image in concrete recognizer settings that has property `fullDocumentImageDPI`, exception is thrown if DPI value is not in the expected range `[100, 400]`
	- fixed a crash in Templating API caused by using a `MultiDetector` with `DetectorRecognizer`
        - fixed returning of face image when using `PPUnitedArabEmiratesIDFrontRecognizer`:
            - fixed face image position
    - fixed crash in `PPDocumentFaceRecognizer`


## 5.12.1

- Bugfixes:
    - fixed an issue where toast messages were not displayed when set
    - fixed reading of Czech QR codes which contain encoded URLs
    - fixed reading of Croatian HUB3 payment PDF417 barcodes:
    - added support for reading barcodes whose encoding is not explicitly defined and encoding is cp1250
    
## 5.12.0

- Updates and additions
    - added support for reading back side of new Australian Driver's licence for state Victoria - use `PPAustraliaDLBackRecognizerSettings` and `PPAustraliaDLBackRecognizerResult`
    - added support for reading front side of Indonesian ID - use `PPIndonesianIDFrontRecognizerSettings` and `PPIndonesianIDFrontRecognizerSettings`
    - added support for Malaysian visa with document code TS - use `PPMrtdRecognizerSettings` and `PPMrtdRecognizerResult`
    - added support for setting DPI for full document images returned by `PPMrtdRecognizerSettings`, `PPAustraliaDLBackRecognizerSettings`, `PPAustraliaDLFrontRecognizerSettings` and `PPEudlRecognizerSettings`:
        - use `fullDocumentImageDPI` on the corresponding recognizer settings
    - added full support for iPhone X layout for all SDK's overlay views

- Minor API changes
    - removed `imageDPI` property on `PPTemplatingRecognizerSettings`        

- Improvements in ID scanning performance:
    - `PPCroSlipRecognizer`: added support for Croatian references with models 64, 26, 35, 40, 69
    - improved parsing of payment QR codes
    - improved reading of Malaysian MyKad address   

- Bugfixes:
    - added missing document classifier property `documentClassifier` to `PPTemplatingRecognizerSettings`    

## 5.11.0

- Updates and additions
    - Added Polish ID Back Recognizer `PPPolishIDBackRecognizerResult` and `PPPolishIDBackRecognizerSettings`
    - Added Polish ID Front Recognizer `PPPolishIDFrontRecognizerResult` and `PPPolishIDFrontRecognizerSettings`
    - Added Polish ID Combined Recognizer `PPPolishIDCombinedRecognizerResult` and `PPPolishIDCombinedRecognizerSettings`
    - Added Australian Driver Licence Recognizer `PPAustraliaDLFrontRecognizerResult` and `PPAustraliaDLFrontRecognizerSettings` for state Victoria
    - Added Swiss ID Front Recognizer `PPSwissIDFrontRecognizerResult` and `PPSwissIDFrontRecognizerSettings`
    - Added Swiss QR Payment `PPChQrCodeRecognizerResult` and `PPChQrCodeRecognizerSettings`
    - Added option `extractRecipient` on `PPNedSlipRecognizerSettings` to enable or disable reading of recipient name. By default, this is turned off and recipient name will not be returned
    - Added reading of mirrored QR codes
    - Added `PPMrzFilter` protocol and delegate `mrzFilter` on `PPMrtdRecognizerSettings`
        - Determines whether document should be processed or it is filtered out, based on its MRZ (Machine Readable Zone)
    - Introduced `GlareDetector` which is by default used in all recognizers whose settings implement `GlareDetectorOptions`:
        - When glare is detected, OCR will not be performed on the affected document position to prevent errors in the extracted data
        - If the glare detector is used and obtaining of glare metadata is enabled in `MetadataSettings`
        - Glare detector can be disabled by using `detectGlare` property on the recognizer settings
    - Added `PPQuadDetectorResultWithSize` which inherits existing `PPQuadDetectorResult`
        - It's subclasses are `PPDocumentDetectorResult` and `PPMrtdDetectorResult`
        - Returns information about physical size (height) in inches of the detected location when physical size is known
        - added support for scanning front and back side of Polish ID - use `PPPolishIDFrontRecognizerSettings`, `PolishIDBackRecognizerSettings` and `PPPolishIDCombinedRecognizerSettings`
    - New document specification presets in `PPDocumentPreset` enum:  `PPDocumentPresetId1VerticalCard` and  `PPDocumentPresetId2VerticalCard` - use `[PPDocumentSpecification newFromPreset]` method to create document specification for detector
    - `PPEudlRecognizer` can return face image from the driver's license
    - Warning for time limited license keys when using provided activities, custom UI integration or Direct API:
        - the goal is to prevent unintentional publishing of application to production with the demo license key that will expire
        - warning toast can be disabled by using `showLicenseKeyTimeLimitedWarning` property on `PPUiSettings`
    - Added `PPMrtdSpecification` and method `setMrtdSpecifications` on `PPMrtdDetectorSettings`
        - setting `PPMrtdSpecification` on `PPMrtdDetectorSettings` will return results only for specified MRTD Documents
        - `PPMrtdSpecification` can be created using `PPMrtdPreset`: `PPMrtdPresetTd1, PPMrtdPresetTd2, PPMrtdPresetTd3`    

- Minor API changes
    - `PPDocumentDetectorResult` does not contain information about screen orientation any more

- Bugfixes:
    - Fixed crash which sometimes happened while scanning MRTD documents
    - Fixed scanning return result type of `PPDetectorRecognizerSettings` when initialized with `PPMrtdDetectorSettings` - returning `PPMrtdDetectorResult`
    - Fixed race condition crash when initialising and getting videoPreviewLayer 

- Improvements in ID scanning performance:
    - Date parsing improvements
    - Better extraction of fields on back side of the Croatian ID card
    - Improved reading of issuing authority on Croatian ID back side
    - Improved face detection in `DocumentFaceRecognizer`: stable detection is required to prevent returning of blurred images
    - Improved reading of Malaysian `MyKad` documents: 
        - Improved reading and parsing of address fields; previously recognizer was unable to read some documents because of the expected address format
    - Improved reading of Malaysian visas and work permits
    - Better reading of dates on Australian Driver's Licence
    - Improved parsing of variable symbol on Czech payment slips
    - Allowed scanning of Czech QR codes which have colon (:) in payment description field
    - Italian IBAN parsing now validates the BBAN check digit

## 5.10.2

- Bug fixes
    - fixed amount parsing from German BezahlCode
        - 0.8 is now parsed as 80 cents, not as 8 cents anymore
    - fixed autorotation of overlay view controller  

## 5.10.1

- Bug fixes
    - fixed crash in QR code which happened periodically in all recognizers

- Minor API changes
    - removed option to scan 1D Code39 and Code128 barcodes on US Driver's licenses that contain those barcodes alongside PDF417 barcode

- Improvements for existing features
    - better extraction of fields on back side of the Croatian ID card
    - improved USDLRecognizer - added support for new USDL standard
    - KosovoCode128Recognizer returns reference number for new barcode type

## 5.10.0

- Updates and additions
    - `PPBlinkOcrRecognizerResult` and `PPBlinkOcrRecognizerSettings` are now deprecated. Use `PPDetectorRecognizerResult` and `PPDetectorRecognizerSettings` for templating or `PPBlinkInputRecognizerResult` and `PPBlinkInputRecognizerSettings` for segment scan
    - `PPSerbianPdf417RecognizerResult` and `PPSerbianPdf417RecognizerSettings` are renamed to `PPSerbianBarcodeRecognizerResult` and `PPSerbianBarcodeRecognizerSettings`
    - Added Austrian Passport Recognizer `PPAusPassportRecognizerResult` and `PPAusPassportRecognizerSettings`
    - Added Swiss Passport Recognizer `PPSwissPassportRecognizerResult` and `PPSwissPassportRecognizerSettings`
    - Added Swiss ID Back Recognizer `PPSwissIDBackRecognizerResult` and `PPSwissIDBackRecognizerSettings`
    - Added support for scanning MRZ on Mexican voters card
    - Added support for reading Croatian ID with permanent DateOfExpiry in `PPCroIDFrontRecognizerResult` and `PPCroIDCombinedRecognizerResult` with BOOL property `isDocumentDateOfExpiryPermanent`
    - Added combining data from MRZ and fields in Austrian passport through `PPAusIDCombinedRecognizerResult` and `PPAusIDCombinedRecognizerSettings`
    - Added support for reading Serbian payment QR codes with `PPSerbianBarcodeRecognizerResult` and `PPSerbianBarcodeRecognizerSettings`
    - Added reading of mirrored QR codes

- Bugfixes:
    - Fixed crash which sometimes happened while scanning MRTD documents
    - Fixed returning valid data for MRZ based recognizers when not all fields outside MRZ have been scanned

- Improvements in ID scanning performance:
    - Improved scanning of IKad addresses
    - Improved reading of Croatian ID Address field
    - Improved reading of Croatian ID IssuedBy field
    - Date parsing improvements
    - Improved parsing of amounts with less than 2 decimals

## 5.9.4

- Updates and additions
    - Added `nonMRZNationality` and `nonMRZSex` properties to Romanian ID Recognizer for getting sex and nationality outside MRZ
    - Added support for long addresses and employer names for iKad
    - `extractAddress` property in `PPSlovakIDBackRecognizerSettings` is now removed since previously wasn't used
    - Added `extractDocumentNumber` property in `PPSlovakIDFrontRecognizerSettings` for defining if issuing document number should be extracted from Slovakian ID 
    - Removed `PPAztecRecognizerSettings` and `PPAztecRecognizerResult`
    - Added to Slovakian ID Combined Settings options properties:
        - `extractSex`
        - `extractNationality`
        - `extractDateOfBirth`
        - `extractDateOfExpiry`
        - `extractDateOfIssue`
        - `extractIssuedBy`
        - `extractDocumentNumber`
        - `extractSurnameAtBirth`
        - `extractPlaceOfBirth`
        - `extractSpecialRemarks`

- Bugfixes:
    - Fixed crash while scanning QR Code
    - Fixed reading positions of ID elements on Slovakian ID card
    - Fixed reading positions of ID elements on Singapore ID card

- Improvements in ID scanning performance:
    - Always read personal number field on front side of Slovakian ID card
    - Improved reading precision of address, place of birth, last name and issuing authority on Slovakian ID card
    - Improved reading of name and blood type on Singapore ID card        


## 5.9.3

- Updates and additions
    - Added Barcode Recognizer `PPBarcodeRecognizerResult` and `PPBarcodeRecognizerSettings`
    - Deprecated `PPAztecRecognizerResult` and `PPAztecRecognizerSettings`. Use Barcode Recognizer
    - Deprecated `PPBarDecoderRecognizerResult` and `PPBarDecoderRecognizerSettings`. Use Barcode Recognizer
    - Deprecated `PPZXingRecognizerResult` and `PPZXingRecognizerSettings`. Use Barcode Recognizer

- Bugfixes:
    - Fixed Czech QR Code amount scanning - improved parsing of amounts with less than 2 decimals
    - Fixed support for Slovenian references without prefix

## 5.9.2

- Updates and additions
    - Added creation of customized build of framework. If your final app size is too large, you can create a customised build of MicroBlink.framework and MicroBlink.bundle which will contain only features and resources that you really need. You can see detailed explanation at [Creating customized build of PhotoPay SDK](https://github.com/PhotoPay/photopay-ios/wiki/Creating-customized-build)
    - Added Serbian PDF417 Barcode `PPSerbianPdf417RecognizerResult` and `PPSerbianPdf417RecognizerSettings`
    - Added US Driver's license Combined Recognizer `PPUsdlCombinedRecognizerResult` and `PPUsdlCombinedRecognizerSettings` 
    - Added Austrian ID Combined Recognizer `PPAusIDCombinedRecognizerResult` and `PPAusIDCombinedRecognizerSettings`
    - Added Czech ID Combined Recognizer `PPCzIDCombinedRecognizerResult` and `PPCzIDCombinedRecognizerSettings`
    - Added Serbian ID Combined Recognizer `PPSerbianIDCombinedRecognizerResult` and `PPSerbianIDCombinedRecognizerSettings`
    - Added Singapore ID Combined Recognizer `PPSingaporeIDCombinedRecognizerResult` and `PPSingaporeIDCombinedRecognizerSettings`
    - Added Slovakian ID Combined Recognizer `PPSlovakIDCombinedRecognizerResult` and `PPSlovakIDCombinedRecognizerSettings`
    - Added Slovenian ID Combined Recognizer `PPSlovenianIDCombinedRecognizerResult` and `PPSlovenianIDCombinedRecognizerSettings`
    - Added VIN Recognizer `PPVinRecognizerResult` and `PPVinRecognizerSettings`
    - Updated Kosovo Code128 Barcode with new data: `district`, `dueDate`, `customerID`, `serviceCode`

- Bugfixes:
    - Fixed Czech translation

- Improvements in PhotoPay scanning:
    - Improved reading of French IBANs
    - Added support for all IBANs return always with prefix
    - Improved reading of pdf417 barcodes having width:height bar aspect ratio less than 2:1

## 5.9.1

- Fixed crash which sometimes happened when presenting help screens (if `PPHelpDisplayModeAlways` or `PPHelpDisplayModeFirstRun` were used)

## 5.9.0

- Updates and additions:
    - Microblink.framework is now a dynamic framework. The change is introduced because of the following reasons:
        - isolation of code
        - smaller binary size - roughly 16%
        - better interop with third party libraries (such as Asseco SEE Mobile Token)
    - Improved Screen shown when Camera permission is not granted:
        - fixed crash which happened on tap anywhere on screen
        - close button can now be removed (for example, if the scanning screen is inside `UINavigationController` instance)
        - Header is now public so you can instantiate that class if needed
    - Updated PPUiSettings with new features:
        - flag `showStatusBar` which you can use to show or hide status bar on camera screen 
        - flag `showCloseButton` which you can use to show or hide close button on camera screen. By default it's presented, but when inside `UINavigationController` it should be hidden
        - flat `showTorchButton` which you can use to show or hide torch button on camera screen.
    - Deprecated `PPHelpDisplayMode`. You should replace it with a custom logic for presenting help inside the application using the SDK.
    - Renamed internal extension method with namespace so that they don't interfere with third party libraries
    - Added standard tap to focus overlay subview in all default OverlayViewControllers. Also added it as a public header.
    - PPScanningViewController now has a simple method to turn on torch
    - Simplified `PPOcrLayout` class (removed properties which were not used)
    - 
- Bugfixes:
    - Fixed bug which caused didOutputResults: not to get called in DirectAPI
      Fixed case sensitivity in class & file naming
    - Fixed issue which sometimes caused scanning not to be started when the user is asked for camera permission (first run of the app)
    - Fixed rare crash which Camera paused label UI being updated on background thread
    - Fixed incorrect handling of camera mirror when using front facing camera
    -   
- Improvements in PhotoPay scanning:
    - Improved reading of the receiver field on Austrian payslips
    - Improved recipient name and address extraction in Slovakian slips (recipient name is not returned when second line of address is empty)
    - Added support for polish IBAN without PL prefix

- Improvements in ID scanning performance:
    - ID result classes which have Date fields now return both parsed `NSDate` and raw `NSString`
    - Restructured German ID recognizers into:
        - GermanIDFrontRecognizer, for scanning front side of the new German ID
        - GermanIDBackRecognizer, for scanning back side of the new German ID
        - GermanOldIDRecognizer, for scanning front side of the old German ID
        - GermanPassportRecognizer, for scanning front side of the German Passport
        - added PPGermanIDCombinedRecognizer which enables reading of all data contained on German passports, old and new IDs
    - Splitting address on new German IDs to ZIP code, city, street and house number
    - Added name and surname dictionaries for the German ID front side recognizer which improves the scanning performance
    - MyKadRecognizer now knows how to split address to street, ZIP code, city and state
    - Improved CroIDCombinedRecognizer, which can scan both sides of the ID consecutively 
    - Improvements in CroID scanning, use multiple scans to boost confidence
        - croID combined bugfix - now always showing DocumentBilingual flag
    - Improvements in MRTD scanning:
        - WSA (World Goverment of World Citizens) added as valid country code when parsing MRZ 
    - Added option of encoding images of MRZ and full document in Machine readable travel documents and encoding of images in DocumentFaceRecognizer
    - Handling names containing dashes and extra long names inside combined recognizers
    - Added AztecRecognizer for state of the art scanning of Aztec barcodes
    - Added RomanianIDFrontRecognizer for scanning Romanian IDs
    
- Sim & TopUp scanning improvements:
    - Added suport for 14 digits long sim numbers in addition to existing lengths (12, 19, 20)
    - TopUp scanning improvements
    
- Changes in Samples:
     - Added libz to all samples to prevent linker errors (caused by slimming down the SDK)
     - Samples updated to use new dynamic framework
     - Added a build phase in each sample which removes unused architectures from the dynamic framework

## 5.8.0

- added designated initializers to all `PPOcrParserFactory` objects
- added play success sound method to `PPScanningViewController` protocol
- improved `TopUpParser`: added option to enable all prefixes at the same time (generic prefix)
- improved `MRTDRecognizer` with better support for Arab MRZ
- updated `CroatianIDFrontSideRecognizer`: returning sex as written on front side of a document
- fixed issue with Direct API which disabled processing
- fixed crash when multiple QR code-based recognizers were used together
- fixed crash in Slovenian QR Code Recognizer
- Internal switch to new build system using cmake. This allows faster deployments and easier updates in the future

## 5.7.2

- Fixed issue with blurred camera display when `PPCoordinator` instance was reused between consecutive scanning sessions
- Fixed crashed which happened when multiple instances of `PPCoordinator` were used simultaneously (one being terminated and one starting recognition). This most commonly happened when after scanning session, a new view controller was pushed to a Navigation View Controller, when the user repeated the procedure a number of times (five or more).
- Added SimCardRecognizer
- Added Generic parsing in TopUpOcrParser

## 5.7.1

- Updated Slovakian Payment slip results. Now they don't have redundant property for account number, instead of that all results now have iban property.
- Updated Slovenian QR code scanning results. We now split street and place information from payer and recipient address. Instead of using `payerAddress`, please use `payerStreet` and `payerPlace`, and instead of `recipientAddress`, please use `recipientStreet` and `recipientPlace` properties.
- Updated getters for obtaining names of images returned when scanning ID documents. You can now use static properties in `PPRecognizerSettings` to obtain names of the images. Previously an instance of `PPRecognizerSettings` subclass was needed.
- Improved Croatian Reference number parsing in Croatian PhotoPay 
  - references in form HRXX-XXXX... are not returned if model is not valid, for example HR22-2360 is not a valid reference because model 22 does not exist
  - trailing whitespace is removed from result when using Segment scanning
- Fixed an issue which caused camera settings to be reset each time PPCoordinator's applySettings method was called. This issue manifested, for example, by automatically turning off torch after successful scan in SegmentScan.
- Fixed redundant log warnings in setting language ("Trying to set language to nil, returning") and CameraManager ("hould not have been observing autofocus")
- Fixed issue with resuming camera when user is first asked for camera permission. This manifested as sometimes camera going black.
- Fixed encoding issue in Slovenian QR code result - `rawResult` property. We now return UTF8 string.

## 5.7.0

- Added support for new UPN format in Slo payment slip recognizer (works out of the box)
- Added support for Slovak Code128 scanning on payment slips  
- Added CroIDCombined recognizer which can scan both sides consecutively
- Added DocumentFace recognizer which can be used to get the image of the ID document which contains a face
- Added FaceDetector feature which can now be used in DetectorRecognizer.
- Added support for extracting place of birth on old German IDs
- Added property allowResultForEveryFrame in PPScanSettings which can be used when using Direct API to force calling didOutputResults: callback for every frame
- Added feature to enable frame quality estimation when using Direct API (by exposing property estimateFrameQuality)
- Added support for scanning IBAN from Georgia in Segment Scan
- Added logging of the SDK name when the license key is invalid for easier troubleshooting
- Added Belgian account number check to IBAN parser
- Added scaling of the default viewfinder in ID scanning overlay view
- Added a property which you can use to set a custom location for resources. For example, if you would like to avoid using Microblink.bundle as resources bundle, you can set a different one in PPSettings object.
- Improved quality of German ID address recognition
- Updated - Singapore ID recognizer has now split in two recognizers - one for front and one for back side
- Fixed Date of Birth scanning issue in MyKad Recognizer
- Fixed MRTD returning payment data with verified = false when mrtdSettings.allowUnverified(false)
- Fixed bug in MRTD recognizer where mrtd image were not returned although scanning was successful
- Fixed crash when Single dispatch queue was used for processing
- Fixed frame quality issue in PPimageMetadata. Previously it was always nan if used after image getter.
- Fixed Torch button on default camera overlays. Previously it never changed state after it was turned on.
- Fixed help display mode "First run", which previously didn't work
- Fixed crash when the user tapped anywhere on the view controller presented when camera permission wasn't allowed
- Fixed warning message when language is set to something other than @en, @de and @fr and @cro
- Fixed crash on start in swift if custom UI was used to handle detector results
- Fixed a problem which caused internal recognizer state not to be reset when using the scanner for the second time with the same PPCoordinator instance
- Fixed ocrLayout getter in PPBlinkOcrRecognizer which previously returned nil

## 5.6.0

- Added support for scanning IBAN from Georgia in Segment Scan
- Added Belgian account number check to IBAN parser
- Added PhotoPay support for scanning Slovak payslips and Slovak Data Matrix codes
- Fixed MRTD returning payment data with verified = false when mrtdSettings.allowUnverified(false)
- Singapore ID recognizer has now split in two recognizers - one for front and one for back side

## 5.5.0

- updated `SepaQRRecognizer` to process image frames in the same way as `ZXingRecognizer`. Now, `useSlowerThoroughScan` is enabled by default.
- improved quality of german ID address recognition
- added support for extracting place of birth on old German IDs

## 5.4.1

- added support for scanning IBANs that contain spaces and dashes
- support for scanning Croatian slips that have no amount, but have currency in amount field

## 5.4.0

- added support for scanning front and back side of Serbian ID cards
- improved IBAN parser
- `PPMrtdRecognizerResult` now returns date of expiry and date of birth as `NSDate` instead of `NSString`
- all recognizer results (classes that derive `PPRecognizerResult`) now have annotated nullability for their getters. Some of them used to assume non-null, while still returning `nil` sometimes. This has now been corrected and all getters are `_Nullable`
- Czech account number is now separated into first (prefix) and second part. First part is not mandatory on czech payslips.
- improved amount parser
- added support for returning optional data beyond the end of SEPA payment QR code

## 5.3.0

- added support for scanning Beneficiary name in slovenian payslips
- added support for scanning PayerID from slip (if missing in OCR line) in hungarian payslips
- improved czech code and symbol parsing
- added support for recognizing SEPA payment QR codes

## 5.2.0

- iOS updates:
	- added support for hungarian parsers in segment scan
		- account number parser
		- payer ID parser
	- added support for slovenian parsers in segment scan
		- reference parser
	- improved Croatian ID recognition
		- address parsing improved with dictionary
		- first and last name parsing improved with dictionary

## 5.1.0

- iOS updates:
	- Added support for Slovakian and German ID cards
	- Added support for Austrian driver's license
	- Improved scanning performance of Croatian ID cards

- iOS bugfixes:
	-Fixed localization issues

## 5.0.3

- iOS fixes:
	- Fixed issue where scanning didn't work when user first accepted camera permission

## 5.0.2

- iOS fixes:
	- CFBundleSUpportedPlatforms removed from Info.plist files
	- Applying affine transformation to `PPQuadrangle` now correctly assigns points.
	- When using both Direct API and `PPCameraCoordinator`, scanning results will now be correctly outputted to `PPCoordinatorDelegate` and `PPScanningDelegate` respectively
	- Fixed crashes related to camera permissions and added dummy view when camera permission is disabled
	- Fixed issues related to topLayoutGuide on iOS6
	- Improved performance of CroID recognizers
	- USDL elements can now be separated by \r
	- Improved performance of Date parser
	- Improved Macedonian reference and account parsers

## 5.0.1

- iOS updated:
	- exposed new features of PPPriceOcrParserFactory

## 5.0.0

- iOS updates:

	- Implemented `PPCameraCoordinator`. `PPCameraCoordinator` assumes the role of `PPCoordinator` from previous versions while new `PPCoordinator` is used for Direct API (image processing without camera out management).
	- Increased speed of scanning for barcode type recognizers.
	- Implemented `PPImage`. When using Direct API you can wrap `UIImage` and `CMSampleBufferRef` into `PPImage` to ensure optimal performance.
	- Improved performance of Direct API. In addition, you can now use Direct API with your own camera management without any performance drawbacks.
	- Added method `isCameraPaused` to `PPScanningViewController`.
	- Added option to fllip input images upside down for processing with `cameraFlipped` property of `PPCameraSettings`.
	- Implemented `PPViewControllerFactory` for managing creation of `PPScanningViewController` objects.
	- `PPImageMetadata` now contains `PPImageMetadataType` property, which describes which image type was outputted
	- Added option to mirror camera frames in 'PPCameraSettings'
	- Added recognizer for Singapore ID
	- Added recognizer for Austrian ID
	- Added recognizer for Czech ID
	- Improved Macedonian parsers
	
- iOS bugfixes:
	- PPOcrEngineOptions are now applyed correctly when set

## 4.6.4

- Improved Parsing of Croatian reference number parser

## 4.6.2

- iOS bugfixes:

- Fixed issue with presentation of some overlay subviews
- Fixed issue with parsing amount on Swedish payment slips

- Implemented templating API

- Templating API allows implementing custom document scanners, linking specific parsers to specific locations on detected documents

- Added license plate parser

## 4.6.1

- iOS bugfixes:

    - Fixed issue with starting camera after first help display.
    - Fixed possible deadlock in some cases when MRTD documents are scanned.

- PhotoPay improvements

    - Fixed issue with IBAN scanning on Slovenian UPN-s when 1D barcode is located on the slip
    - Fixed Belgian reading of amounts on Belgian slips which are less than 1.00 EUR
    - Added support for scanning Receiver name on Dutch Acceptgiros
    - Added support for Scanning Slovakian and Czech QR codes
    - SweGiroParser can now parse both BankGiro and PlusGiro
    - fixed missing last digit (check digit) in SweReferenceParser
    
- ID scanning improvements

    - Added EUDL recognizer (replaced UKDL recognizer). EUDL is capable of automatically detecting various EU Drivers licenses. Currently it works only on German and UK DLs.
    - Fixed issue with 0 and O misclassifications in MRTD recognition
    - Added support for Austrian MRTD ID documents

- Internal changes:

    - Implementeded Templating API for easier implementation of new document types
    - Implemented Face detection
    - Implemented support for Eastern Arabic numeral characters
    
## 4.6.0

- PPOverlayViewController changed the way Overlay Subviews are added to the view hierarchy. Instead of calling `addOverlaySubview:` (which automatically added a view to view hierarachy), you now need to call `registerOverlaySubview:` (which registers subview for scanning events), and manually add subview to view hierarchy using `addSubview:` method. This change gives you more flexibility for adding views and managing autolayout and autoresizing masks.

- Localization Macros MB_LOCALIZED and MB_LOCALIZED_FORMAT can now be overriden in your app to provide completely custom localization mechanisms.

- `PPPhotoPayUiSettings` now has an option to display dots UI effect, when QR code is scanned. To enable it, use:

```objective-c
PPSettings* settings = [[PPSettings alloc] init];

PPPhotoPayUiSettings *ppUiSettings = [[PPPhotoPayUiSettings alloc] init];
ppUiSettings.displayBarcodeDots = YES;

settings.uiSettings = ppUiSettings;
```

- Fixed issue with OCR speed on arm7 devices when Accelerate framework was used.

- Improvements in Slovenian slip recognition

    - fixed hang that occurred in slovenian PhotoPay on some poor quality slips
    - improved quality and robustness of slovenian reference number parsing
    - improved OCR quality of slovenian slip containing courier condensed font
    
- Improvements in Croatian slip recognition

    - Fixed crash which happened when device was pointed something other than a payment slip
    - Added scanning of Payer information from HUB1 and HUB3 barcodes
    
- Improvements in Segment scan

    - Added Montenegro parsers
    - Added Regex parser
    
- Improved detection of German payslips

- Dramatically increased OCR engine initialization speed

- Increased speed of scanning cancellation when Cancel button is pressed.

## 4.5.1

- Improved recognition of Slovenian payment slips

    - Added support for RF references
    - Reference is no longer required field for scanning, meaning payment slips which don't have reference number are now scanned much faster 

## 4.5.0
    
- Improvements in Hungarian payslip scanning
    - Fixed issue which caused dictionary not to be used while scanning beneficiary name
    - Added reading of Amount from the upper part of the payslip
    
- Library format updated
    - Smaller library size using link time optimizations
    - Fewer exported symbols.
    - Library is now static instead of Relocatable Object File
    
- Resource bundle updated
    - Fixed issue with MicroBlinkResources.bundle while archiving application (Invalid `CFBundleExecutable` key)
    - Renamed MicroBlinkResoucres.bundle to MicroBlink.bundle
    
- Better Swift interoperability
    - Support for modules
    - Added nullability annotations
    
- Bugfixes and tweaks in camera management code
    - fixed potential deadlock when multiple instances of `PPCoordinator` objects are instantiated.
    - exiting from the scanning when user presses "cancel" button is now faster
    
- Refactored `PPMetadataSettings` 
    - Added debug metadata settings for debugging payslip detection and image processing
    - `successfulScanFrame` renamed to `successfulFrame`
    - `currentVideoFrame` renamed to `currentFrame`

## 4.4.1

- Improvements in Hungarian payslips scanning:

    - Added dictionary postprocessing of recipient names
    - fixed issue where payer ID was parsed as an account number
    - small improvements in payslip detection

- New Stuzza QR format supported in German recognition. Also, QR codes without BIC are now allowed.
     
- Fixed race condition which potentially crashed the scanner when user exited and entered camera screen consecutively very fast. 

- Data extraction now exits immediately when user cancels scanning. 

## 4.4.0

- Framework is now distributed as a .framework + .bundle, instead of .embeddedframework. This helps keep resources in a separate "namespace", and avoids mistakes
- The library inside the framework is now static library. This makes it easier to include the library inside other libraries.
- PhotoPay on boarding is moved outside the library itself. The intention is to make it easier to modify and extend to the users of the library.

- Improvements in the recognition process:
    
    - Added support for Hungarian White payslips
    - Improved support for scanning Recipient name in Hungarian Yellow payslips.
    - Improved support for scanning Reference numbers in Slovenina payslips
    - Added Stuzza 2.0 QR code format in AusQRRecognizer

- Bugfixes and tweaks in camera management code

## 4.3.0

- Added support for scanning recipient name in Hungarian payslips
- Added bitcode support for Xcode 7

## 4.2.0

- iOS 9 introduced new app multitasking features Split View and Slide Over. When the scanner is on screen and one of those features are used, iOS automatically pauses the Camera (this behaviour is default as of iOS 9 beta 5). This SDK version introduces new setting in `PPUISettings` class, called `cameraPausedView`, where you can define the `UIView` which is presented centered on screen when this happens.

- Frame quality estimation can now be enabled using `PPScanSettings frameQualityEstimationMode` property:
    - when set to `PPFrameQualityEstimationModeOn`, frame quality estimation is always enabled
    - when set to `PPFrameQualityEstimationModeOff`, frame quality estimation is always disabled
    - when set to `PPFrameQualityEstimationModeDefault`, frame quality estimation is enabled internally, if the SDK determines it makes sense
    
- Better frame quality estimation: only the sharpest and the most focused frames now go to OCR processing

- `PPScanningViewController` methods `pauseScanning`, `isScanningPaused`, and `resumeScanningAndResetState:` should now be called only from Main thread, and they are effective immediately. E.g., if `pauseScanning` is called and there is a video frame being processed, result of processing of that frame will be discarded, if `resumeScanningAndResetState:` isn't called in the meantime.

- Added support for `PPCameraPresetPhoto` camera preset. Use this if you need the same zoom level as in iOS Camera app. The resolution for video feed when using this preset is the same as devices screen resolution.

- Known issue: if you use Autorotate overlay feature, present `PPScanningViewController` as a modal view controller, and support Split View iOS 9 feature, then autorotation of camera overlays isn't correct. The best way is to opt-out of Split View feature (which Apple suggests for camera-centric apps), and wait for PhotoPay fix when iOS 9 comes out of beta.


## 4.1.1
- Worked around Apple bug in [NSString stringEncodingForData], which resulted with non-deterministic Croatian slip QR code scanning 

## 4.1.0
- Internal refactoring and cleanup
- PPImageMetadata now correctly handles image rotation.
- Direct API and regular camera management API can now be used simultaneously. 
- Added feature for using native iOS orientation handling methods in `PPScanningViewController`

## 4.0.3
- Bugfixes and improvements in OCR engine and payslip scanning

## 4.0.2
- Improved BIC extraction
- Bugfixes in Slovenian scanning

## 4.0.1
- Peformance improvements in OCR engine
- Added Direct processing API
- Added scanning of Optional data in croatian payment QR codes

## 4.0.0
- New API

## 3.5.0
- Tweaks in text parsing methods (Sieve algorithm)
- PhotoPay.h renamed to PPScanner.h
- PPPhotoPayDelegate renamed to PPScanDelegate
- methods of the delegate renamed
- These updates are performed for upcoming 4.0 version

## 3.4.0
- Added callback method for handling case when user doesn't authorise Camera access. In this method you have the chance to update the CameraViewController (or OverlayViewController) UI so that user knows why scanning won't work. 
- Removed ViewfinderMoveable property

## 3.3.0
- Bug fixes and performance improvements
- Added support for Swiss payslips

## 3.2.1
- Updated icons and launch images in Sample app

## 3.2.0
- Status bar is no longer set with [[UIApplication sharedApplication] setStatusBarStyle:statusBarStyle animated:animated], because this caused problems with FormSheet and PageSheet presentation styles.
- Users of the library are now responsible for proper handling of status bar on pre iOS7 devices.
- Help view fix for FormSheet and PageSheet modes

## 3.1.1
- Added OCR line support for new fonts for UK
- Fixed amount reuse bug with consecutive scans when UINavigationController is used for presenting camera
- Sample app converted to ARC
- Documentation is split into two parts - README describing general PhotoPay integration, and CustomUI describing API for creating custom Camera Overlays.

## 3.1.0
- Default camera overlay is now improved for a better user experience
- Added support for SEPA payslips in german scanner
- Improved detection of German payslips
- Fixed internal bugs and API inconsistencies

- 3.1.0 includes changes to resource files in the framework. Some resource files were added, some were changed, and some were deleted. To avoid erros in building your app, it's best to remove the old PhotoPay.embeddedframework from your project, and add it again by drag-and-droping the new one to the Frameworks group. 

After that, clean-build your application.

## 3.0.6
- Fix for crashing on pre iOS 7 devices

## 3.0.5
- Utility ID in Kosovo now returned without check digit
- Fixed status bar style in camera view on fist app run

## 3.0.4
- Fixed scanning for Croatian Reference model 11

## 3.0.3
- Fixed problems with autorotation callbacks in overlay view not being called when autorotation is disabled
- Camera can now be presented on UIPageViewController

## 3.0.2
- Removed unnecessary log outputs in run-time
- Fixed whitespace handling in Austrian scanner

## 3.0.1
- Removed status bar properties from PPScanningViewController protocol. Replaced with preferredStatusBarStyle and prefersStatusBarHidden in PPOverlayViewController
- Default PhotoPay Overlay for Austria, Croatia, Germany, Slovenia and Belgium is now rotation independent.
- kPPHudOrientation replaced with kPPOverlayShouldAutorotate.
  - HUD orientation is now determined optimally inside PhotoPay library
  - Autorotation can now be explicitly disallowed, e.g when presenting camera on NavigationController or when presenting as FormSheet or PageSheet 
- Removed deprecated methods from PPOverlayViewController
- Fixed crash when Overlay those orientations which are not supported by the app. Now overlay works in that situations.
- Fixes in Austrian Amount and Bank Account number scanning
- Added additonal scanning fields in German QR code scanning

## 3.0.0
- By Semantic versioning, since this version is not completely backwards compatible with previous versions, we increased the Major version number.
- Tweaks to OCR engine and text recognition
- Bugfixes in Austrian OCR recognition
- `PPRecognitionResult` object no longer has convenience properties for, e.g., account number, iban, reference number, etc. You can access that values using `fields` dictionary.
- Changes to API method for retrieving recognition results:

instead of using 

	- (void)cameraViewController:(UIViewController<PPScanningViewController>*)cameraViewController
         	 didFinishWithResult:(PPRecognitionResult*)result;
         
Use:

	- (void)cameraViewController:(UIViewController<PPScanningViewController>*)cameraViewController
            	didOutputResults:(NSArray*)results;
            
New method adds an additional layers of abstraction to result obtaining. It makes possible several new features, most importantly, returning more than one recognition result, and returning results other than payslip scanning results (for example, pure OCR or barcode scanning results).

Objects passed in NSArray* `results` are always of type `PPBaseResult`. `PPRecognitionResult` (object passed in the all callback method) is now a subclass of `PPBaseResult`.

How to implement the new API?

1. Rename `cameraViewController:didFinishWithResult` to, i.e. `processRecognitionResult`
2. Implement `cameraViewController:didOutputResults`. Commonly, it can be implemented in the following way:

		- (void)cameraViewController:(UIViewController<PPScanningViewController> *)cameraViewController
            		didOutputResults:(NSArray *)results {
    		// find the first recognition result and present it.
    		// you can have more complex logic here, which can, for example compare fields in multiple results

    		[results enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
        		PPBaseResult* result = (PPBaseResult*)obj;
        		if ([result resultType] == PPBaseResultTypePhotoPay) {
            		PPRecognitionResult* recognitionResult = (PPRecognitionResult*)result;
            		[self processRecognitionResult:recognitionResult];
            		*stop = YES;
        		}
    		}];

    		[self dismissCameraViewControllerModal:useModalCameraView];
		}
3. Test to see if it works.

## 2.5.1
- Fixed model and reference parsing when there was no whitespace between the fields on Croatian payslips
- Improved reading on Kosovo slips with Generalized OCR Line detector
- Fixed check digit inclusion on Kosovo data
- Added ROI for scanning Kosovo slips
- Fixed drawing of OCR results when ROI is used

## 2.5.0
- support for reading `Payer name` in Croatia
- support for turning reading of `Payer name` and `Payment description` on or off on Croatian slips
- added branding for HSBC app
- added a feature for customising camera view status bar appearance and visibility

## 2.4.0
- Added HUB3 QR Code for Croatian scanning
- Enabled use of integrated barcode scanner
- Code 39 and Code 128 are now handled by a completely new and improved algorithm
- Fixed autofocus problem on iPod Touch 4th Gen
- Improved scanning on iPod Touch 5th gen by enforcing 720p resolution
- Added OCR result drawing for countries which have OCR line scanning (Dutch, UK, Kosovo...)
- Package now contains additional file (buildCommit.txt) for easier point version bugfixes
- Fixed issue with scanning Account number on German SEPA QR code
- Implemented initial scanning of payslips for Kosovo

## 2.3.5
- Added full support for UK Bank Giro Credit scanning
- Updated demo app effect when the bill is paid

## 2.3.4
- Support for Xcode 5.1. Updated project settings

## 2.3.3
- Support for basic UK Giro slip scanning

## 2.3.2
- Croatian reference model now returns HR prefix if it exists on the payslip
- All strings returned by PhotoPay now have ASCII encoding fallback. This means no more "Error in encoding!" messages should appear in scanning results.

## 2.3.1
- fixed Austrian STUZZA QR code parsing - now supports both LF and CRLF line endings and does not confuse reference number and payment description

## 2.3.0
- improved croatian reference extraction
- determining croatian refernence status
- several bugfixes in detection algorithm

## 2.2.2
- Added simple OCR result drawing in camera overlay
- Improved payment description reading for Croatian payslips
- Fixed issue with Austrian QR codes containing ECI encoding

## 2.2.1
- Updated Hungarian build for old and legendary yellow payslips  

## 2.2.0
- Improved OCR for Croatian payslips
- Added new field for Croatian scanning "Reference number insecure"
- Fixed issue with returning completely empty results in some cases when scanning timeout occurs 

## 2.1.0
- Added data sanitization for Croatian payslips
- Additional bugfixes and improvements
- Fixed Barcodes with LF chars on Austrian payslips
- Fixed issue with visible status bar when using View controller-based status bar appearance

## 2.0.5
- Improvements to Slovenian slip scanning
- Fixed issue with presenting camera view on navigation view controller.

## 2.0.4
- Support for setting custom language via API
- Optimization for Sieve algorithm for OCR result aligning

## 2.0.3
- Fix to UIStatusBarStyle change issue when returning from CameraViewController
- OverlayViewController appearance callback are now correctly forwarded

## 2.0.2
- Orientation of payment slip detection can now be specified according to OverlayViewController orientation.
- Dutch payslip scanning now supports scanning in all orientations specified by Overlay View Controller.

## 2.0.1
- Fixes to duplicate class names in Demo app in this SDK

## 2.0.0
- Simplifications to UI overlay concept
- Support for form/page sheet modal styles on iPad
- New options for setting the position of viewfinder for Dutch builds
- Improved PDF417 barcode scanning
- More advanced API - added support for pausing/resuming scanning which can be used to scan multiple payslips/barcodes before proceeding to checkout
- Issue with isPhotoPayUnsupported method resolved, now recognizes devices without camera
- Devices with high camera resolution can now be held much higher than before when scanning Dutch payslips. This solves some of the scanning issues.
- Status bar style is now set only for iOS 6 and older devices. Also, tests performed with status bar style, since the old style is popped from a stack in viewWillDissapear of CameraViewController, it shouldn't interfere with style of the caller

## 1.8.5
- Minimum iOS deployment target is now set to 5.0
- Added support for armv7s architectures
- Chanaged standard c++ library from libstdc++ to libc++

## 1.8.4
- Improved image processing with smarter threshold calculation
- Payment description scanning in croatian slips now more reliable
- Added scanning of recipient address for barcodes on croatian slips
- Faster frame quality estimation and image initialization
- New ABBYY version
- Other stability improvements and bugfixes
- Support for unlimited text lengths in help screens

## 1.8.3
- Major refactor of UI layer
- Quicker camera load
- Improved focus management, meaning now frames that are coming to OCR are less blurred which results with faster and
more reliable scanning
- New localization files (Slovenian, French)

## 1.8.2
- new QR code features for Austria
- Hardware accelerated viewfinder

## 1.8.1
- Slovenian version now scans payment description
- Bugfix for scanning with sudden movements of the phone
- Updated strings
- Support for Austrian payslips with nonstandard amount printed on the bottom
- Support for arbitrary length austrian reference numbers

## 1.8.0
- More advanced algorithm for Dictionary
- Focused frame selection bugfix

## 1.7.9
- Added Dictionary checking for tokens returned by Ocr Engine

## 1.7.8
- Added sound on successful scan
- Fix for Austrian amounts in format like ==3.600,--
- Whitespace fix for text recognition
- More improvements to Sieve boosting algorithm
- Added help screen with a message to check results

## 1.7.7
- Sieve algorithm improved, results are not boosted between results
- Belegart recognition in Austria improved, detection algorithm for non SEPA slips improved
- Positioning of some elements fixed, especially on wide screen phones
- In Austria, detection frame is now unmovable. 

## 1.7.6
- Multiple recognitions of receiver name in Austrian slips
- Implemented basic Sieve method for video OCR algorithm

## 1.7.5
- Autoupdate functionality made more reliable
- Testing app more robust, serial dispatch queue used for saving of usage data
- PDF417 update for Croatian payslips

## 1.7.4
- Added autoupdate functionality

## 1.7.3
- Reading of Prufziffer and Belegnr. for Austrian slips
- Slightly modified status text on camera view (larger text, darker background)
- Kundendaten is returned on non-Sepa slips. Otherwise reference number is returned.

## 1.7.1
- Implemented Slovenian check digit calculators
- Slovenian recognition updated to new image processing algorithm and processing by regions
- Austrian recognition handles tax slips
- Fixed problem with recognition of Austrian BLZs
- Payment description recognized if reference is not present on Austrian slips

## 1.7.0
- Added polishers which fix some obvious gramatical errors in recognition results
- Fixed german localization
- Additional stability and bug fixes

## 1.6.8
- Framework is now ARC ready.
- Viewfinder color can now be modified externally
- Fixed to some german localisation strings
- Added formatting of IBANs in groups of 4 chars in demo app
- Some stability issues solved
- Added DataType property in RecognitionResult, this enables users of the library to know what was scanned (QR code or some slip type)

## 1.6.7
- modifications for iPad

## 1.6.6
- Tweaks to Austrian recognition, especially TextBlock extractor
- Toast messages now have minimum duration - fixed problem on too fast devices which caused messages to be unreadable
- Camera management improvements

## 1.6.4
- Changed internal structure of recognition result - now fields are stored inside NSDictionary object
- Existing properties are not changed
- Updated documentation to for new result obtaining method
- Numerous bugfixes in austrian recognition
- Removed alert view for partial recognition - now after timeout you receive the data on `cameraViewController:didFinishWithResult delegate method.

## 1.6.3
- Simplified distribution method for iphone
- Fixed problem with new help view which was not displayed on viewWillAppear
- Sample app uses modal camera view controller
- Language checks are now performed. If language is not set, or set to unsupported language, language is set to phone's default language.
- Fixed autorotation problem on iOS 6

## 1.6.1
- Added kontocheck - collection of algorithms for checking validity of german and austrian bank account numbers

## 1.6.0
- New image processing algorithm
- Payment forms are now recognized field by field instead all fields at once
- New UI, supports all orientations, depending on the payment slip dimensions
- Added QR code support for Austrian BDT format
- Improved detection algorithm with robust thresholding and new scan line generation strategy

## 1.5.2
- Faster image preprocessing for QR codes
- Fix for hangs in BIC recognition

## 1.5.0
- QR Code recognition made more robust in combination with OCR
- Tweaked Austrian recognition
- Fixed bug in recognition of empty elements

## 1.4.5
- Fixed a problem with autofocus
- Added support for high res video frames
- Increased resolution of processed frames before OCR
- Tweaked postprocessing for germany

## 1.4.3
- Added device specific recognition parameters and camera options
- Improved recognition confidence with reusing subsequent recognition results
- Accelerometer control changes - now high pass filter to get more accurate measurements

## 1.4.0
- Improved detection algorithm generation with internal tools
- Fixed image deformation problem with GPU dewarping

## 1.3.8
- Fixed bug with pre iOS 5 video orientation
- Fixed bug with ROI in image preprocessing

## 1.3.7
- Updated to Xcode 4.5
- Updated to new version of Ocr engine
- License file for ocr engine now needed to initialize Ocr engine

## 1.3.6
- Added support for reading QR codes

## 1.3.5
- Using single OpenGL context which additionally speeds up the processing and fixes the hanging bug
- Added arbitrary text block extractor
- Updated ACS algorithm 

## 1.3.1
- Further improvements to adaptive contrast stretching
- Detection algorithm allows more flexibility

## 1.3.0
- Added adaptive contrast stretching for improving recognition in bad light conditions
- Improved recognition speed with doing ocr by regions
- Fixed a memory leak with patterns file
- Improved postprocessing of croatian amounts

## 1.2.7
- Added automatic white balance detection for improving detection
- Small corrections to contrast stretching method
- Rectangle sent to recognition now stretched to allow errors in slip printing
- Improved speed of detection algorithm witch non constant number of iterations

## 1.2.5
- Fixed a problem with detection of croatian slips

## 1.2.4
- Added options for disabling toast messages and setting the partial recognition time interval

## 1.2.3
- Fixed bug which disabled OpenGL support
- Added additional checks for OpenGL geometry operations which fallback to CPU on special events
- Log.h now private header
- Localization macros now defined only if not defined before so users can provide their own localization methods
- Disabled sound that marked successful detection because it became too fast on faster phones

## 1.2.2
- Added croatian localization for "photopay_camera_too_high" string
- Timer for recognition timeout now starts on first failed recognition and lasts for 15 seconds
- Fixed toast message lifecycle in some situations (weird behavior on partial recognition followed by camera too high)

## 1.2.1
- Fixes to alert view showing partial recognition results

## 1.2.0
- Improved recognition speed with geometry transformation moved to GPU
- Detection algorithm made more robust
- OCR speed increase

## 1.1.2
- Fixed errors in iPhone simulator builds

## 1.1.1
- Improvements to detection algorithm
- Improvements to OCR postprocessing algorithm for recognition of slovenian reference numbers

## 1.1.0
- HUB3, UPN standard support
- Added detection of payment form and perspective correction
- Added UI animation
- Added tap to focus support
- Library now distributed as a framework

## 1.0.0
- Basic functionality
