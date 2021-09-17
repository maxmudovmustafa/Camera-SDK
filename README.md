# Camera-SDK
### Libs for sdk. This is not published to artifactory yet
```kotlin
// ### Analise ### noinspection GradleDependency
    implementation "com.google.firebase:firebase-analytics:17.5.0"
    implementation "com.google.firebase:firebase-ml-vision:19.0.3"

//  ### Camera ### noinspection GradleDependency
    implementation 'com.camerakit:camerakit:1.0.0-beta3.10'
    implementation 'com.camerakit:jpegkit:0.1.0'
    implementation 'androidx.camera:camera-core:1.0.0-beta07'

//  ### Tread
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.0.0'
//  ### Anim
    implementation 'com.daimajia.androidanimations:library:2.4@aar'
```
---------------------------------------------------------------------------------

### How to use:

```kotlin
  val intent = Intent(context, CameraActivity::class.java)
  intent.putExtra(KEY_CAMERA, key_to_scan)
  resultLauncher.launch(intent)

```

```key_to_scan```
```java
const val PASSPORT = 1 (gets firstName, lastName, MiddleName, birthDate, givenDate, WhereGiven, WhoGave, Passport seriya, passport number, pinfl)
const val E_PASSPORT = 11 (gets only Passport seria, number, pinfl)
const val PASSPORT2 = 5 (gets fio)
const val DRIVING_LICENSE = 2 (works only in new driving lisences)
const val TEX_PASSPORT = 3 (works only new text_passport)
```


### Result will be here
```kotlin
private val resultLauncher = registerActivityWithResultCar { passSeria, passNumber, pinfl ->
        binding.apply {
            etPassSeria.setText(passSeria)
            etPassNumber.setText(passNumber)
            etPassPinfl.setText(pinfl)
        }
    }
```



---------------------------------------------------------------- 
Extention function.
You can write your own method

```kotlin

fun Fragment.registerActivityWithResultCar(function: (String, String, String) -> Unit) =
    registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result ->
        if (result.resultCode == Activity.RESULT_OK) {
               val objectPassport = PrefsCamera(this).mapPassport

                val passportNumber =
                    if (objectPassport.pNumber.isEmpty() && objectPassport.pSeria.length > 7)
                        objectPassport.pSeria.filter { it.isDigit() }
                    else objectPassport.pNumber


                val pinfl = if (objectPassport.lastOption.length > 20)
                    objectPassport.lastOption.substring(
                        objectPassport.lastOption.length - 16,
                        objectPassport.lastOption.length - 2
                    ) else ""

                function(objectPassport.pSeria, passportNumber, pinfl)
                PrefsCamera(this).mapPassport = PrefsCamera.ObjectPassport()
        }
    }
```


If you whant to make it faster to scan you should change camera to SurfaceView.
