# inImageHelperAdvance
Helps organisations to collect pictures of customerID cards &amp; Selfies in a more secure &amp; friendly manner. It is accompanied by sanity check methods &amp; quality of picture assistance  

## Minimum Requirements
- `minSdkVersion 18` 
- `AndroidX`

## Getting Started

Add following lines in your root ```build.gradle```
```
allprojects {
    repositories {
        ...
        maven { url "https://dl.bintray.com/invoidandroid12/android/" }
    }
}
```

Add following lines in your module level ```build.gradle```
```
dependencies {
    ....
    implementation 'co.invoid.android:imagehelperadvance:1.0.0rc3'
}
```

This library also uses some common android libraries. So if you are not already using them then make sure you add these libraries to your module level `build.gradle`
- `androidx.appcompat:appcompat:1.1.0`
- `androidx.constraintlayout:constraintlayout:1.1.3`
- `com.google.android.material:material:1.1.0`

Since this library is using compiled native code so we recommend to split the apk based on the architecture of the processor to reduce your app size. However, if you are using 
App Bundle to publish app then you don't have to do anything.
Use following code to split the apk.
```
android {
    splits {

        // Configures multiple APKs based on ABI.
        abi {

            // Enables building multiple APKs per ABI.
            enable true

            // Specifies that we do not want to also generate a universal APK that includes all ABIs.
            universalApk false
        }
    }
}
```

## Initialize SDK

```
yourinitbutton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                ImageHelperOptions imageHelperOptions = new ImageHelperOptions.Builder()
                  .setPhotoOption(ImageHelperOptions.PhotoOptions.SELFIE_WITH_DOC_PHOTO)
                  .setDocumentType(DocumentType.AADHAAR)
                  .setGalleryOption(ImageHelperOptions.GalleryOption.ALLOW_IN_DOC_ONLY)
                  .build();
              ImageHelper.with(this, "YOUR_AUTH_KEY", imageHelperOptions).start();
            }
        });
```

- Use ```ImageHelperOptions.Builder.enableAadhaarMasking()``` to can get masked aadhaar photo
- Using ```ImageHelperOptions.Builder.setPhotoOptions()``` use can choose whether you want to take just selfie, just document images or both. Use one of the following ```ImageHelperOptions.PhotoOptions.SELFIE_ONLY```, ```ImageHelperOptions.PhotoOptions.DOC_PHOTO_ONLY``` or ```ImageHelperOptions.PhotoOptions.SELFIE_WITH_DOC_PHOTO```
- To set Document type use ```ImageHelperOptions.Builder.setDocumentType()```. Currently we support only these document types ```DocumentType.AADHAAR```, ```DocumentType.PAN```, ```DocumentType.DRIVING_LICENSE```, ```DocumentType.VOTER_ID``` and ```DocumentType.PASSPORT```.
- To enable or disable image from gallery option use ```ImageHelperOptions.Builder.setGalleryOption()```. Use one of the following ```ImageHelperOptions.GalleryOption.ALLOW_IN_DOC_ONLY```, ```ImageHelperOptions.GalleryOption.ALLOW_IN_SELFIE_ONLY``` to enable or disable in either of the screens. If you want to enable in all the screens then use ```ImageHelperOptions.GalleryOption.ALLOW```, by default it is enabled in all screens.

## Authorization 
To Obtain your organisation's authkey, contact us at hello@invoid.co


## Response returned from the SDK
- Selfie file path ```imageHelperResult.getSelfieResult().getImageFilePath()```
- Document front image file path ```imageHelperResult.getDocumentResult().getDocFrontPath()```
- Document back image file path ```imageHelperResult.getDocumentResult().getDocBackPath()```
- Glare in front document image ```imageHelperResult.getDocumentResult().isGlareInFront()```
- Glare in back document image ```imageHelperResult.getDocumentResult().isGlareInBack()```
- Blur in front document image ```imageHelperResult.getDocumentResult().isBlurInFront()```
- Blur in back document image ```imageHelperResult.getDocumentResult().isBlurInBack()```
- Face in document in front document image ```imageHelperResult.getDocumentResult().isFaceInDoc()```
- Doc clarity in front document image ```imageHelperResult.getDocumentResult().isDocFrontClear()```
- Doc clarity in back document image ```imageHelperResult.getDocumentResult().isDocBackClear()```

```
@Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        if(requestCode == ImageHelper.PHOTO_HELPER_REQ_CODE) {
        
            if(resultCode == RESULT_OK) {
                ImageHelperResult imageHelperResult = data.getParcelableExtra(ImageHelper.IMAGE_RESULT);
                Log.d(TAG, "onActivityResult: "+imageHelperResult.toString());
           
           } else if(resultCode == ImageHelper.AUTHORIZATION_RESULT_CODE) {
           
           int authorizationResult = data.getIntExtra(ImageHelper.AUTHORIZATION_RESULT, -1);
                if(authorizationResult == ImageHelper.UNAUTHORIZED) {
                    Log.d(TAG, "onActivityResult: unauthorized");
                } else {
                    Log.d(TAG, "onActivityResult: authorization error");
                }
            }
        } else {
            super.onActivityResult(requestCode, resultCode, data);
        }
    }
```

