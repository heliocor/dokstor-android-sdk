# dokstor-android-sdk
SDK for dokstor KYC workflow and verifications. Contains native Android SDK.
Description of the SDK components + Usage examples

## Independent components 
The SDK contains 2 major and 2 minor packages.
#### Main packages
- DokstorID
- DokstorSDK
#### Secondary packages (dependencies)
- OpenCV
- Tesseract
## Description of packages
#### DokstorID
In this component you can find functionalities to read documents such as passports, identity documents, driving licenses, it is also possible to obtain photos (selfie) where the user must execute actions(smile, move his head, etc.) before the image is captured; all functions will be executed in local mode, all the functions will be provided with views.
##### Example code

```javascript
// Declare a listener
final ProcessIDCardListener processIDCardListener = new ProcessIDCardListener() {
            @Override
            public void onResponse(com.heliocor.dokstorid.model.IdentityCard identityCard) {
                //TODO

            }

            @Override
            public void onError(String error) {
                //TODO
            }
        };

// In the case of identity card type documents
DokstorID.getInstance().processIdentityCardUI(processIDCardListener, getApplicationContext(), Constants.IDENTITYCARD, false);

// In the case of passport type documents
DokstorID.getInstance().processIdentityCardUI(processIDCardListener, getApplicationContext(), Constants.PASSPORT, false);

// In the case of driving licence type documents
DokstorID.getInstance().processIdentityCardUI(processIDCardListener, getApplicationContext(), Constants.DRIVINGLICENCE, false);

```

#### DokstorSDK
In this component you can find functionalities to read documents such as passports, identity documents, driving licenses; capture user images (selfie) or more complex processes such as the OnBoard. The processing of the data (images) is carried out using external services.

These processes can be done using views or their corresponding functions without UI.

##### Example code

###### OnBoard Example code
```javascript
// Declare a listener
final OnBoardProcessListener onBoardProcessListener = new OnBoardProcessListener() {
            @Override
            public void onPassportCaptured(Passport passport) {
                //  TODO Do what you need with the passport obtained
            }

            @Override
            public void onIdentityCardCaptured(IdentityCard identityCard) {
                //  TODO Do what you need with the IdentityCard obtained
            }

            @Override
            public void onSelfieCaptured(Selfie selfie) {
                //  TODO Do what you need with the Selfie obtained
            }

            @Override
            public void onUserDataCaptured(DKSTRUser dkstrUser) {
                //  TODO Do what you need with the User obtained
            }

            @Override
            public void onPersonalAddressCaptured(Address address) {
                //  TODO Do what you need with the Address obtained
            }

            @Override
            public void onCompleted(ApiResponse apiResponse) {
                //  TODO Do what you need with the ApiResponse obtained
            }

            @Override
            public void onError(String error) {
                //  TODO Do what you need with the error
            }
        };

/**
* Call onBoard with UI process & with NFC
*/
DokstorSDK.getInstance().startOnBoardProcess(onBoardProcessListener, getApplicationContext(), true, "john.doe@example.com");

/**
* Call onBoard with UI process & without NFC
*/
DokstorSDK.getInstance().startOnBoardProcess(onBoardProcessListener, getApplicationContext(), false, "john.doe@example.com");

```
###### Process Passport Example code
```javascript
final ProcessPassportListener processPassportListener = new ProcessPassportListener() {
            @Override
            public void onResponse(Passport passport) {
                //TODO

            }

            @Override
            public void onError(String error) {
                //TODO

            }
        };
// In case where no RFID reading is required, using UI
DokstorSDK.getInstance().processPassportUI(processPassportListener, getApplicationContext(), false);

// In case where RFID reading is required, using UI
DokstorSDK.getInstance().processPassportUI(processPassportListener, getApplicationContext(), true);

// When you only want to process an image (Passport), no UI involved
DokstorSDK.getInstance().processPassport(processPassportListener, bitmap);

```

###### Process Identity Card Example code
```javascript
final ProcessIdentityCardListener processIdentityCardListener = new ProcessIdentityCardListener() {
            @Override
            public void onResponse(IdentityCard identityCard) {
                 //TODO
            }

            @Override
            public void onError(String error) {
                 //TODO
            }
        };
// Usin UI        
DokstorSDK.getInstance().processIdentityCardUI(processIdentityCardListener, getApplicationContext());

// When you only want to process images (Identity Card), no UI involved
// You should provide a List<Bitmap> with two images, front and back side of the identity card
DokstorSDK.getInstance().processIdentityCard(processPassportListener, list);

```

#### OpenCV & Tesseract
The case of opencv and tesseract are necessary dependencies for the correct functioning of DokstorSDK, we provide them independently packaged, but if this is the case and you already have these dependencies (both) in your project, then it will not be necessary to include them.

