# google_map

```
앞서 네이버에서 제공하는 Map API를 써봤지만 원작자가 업데이트를 중단하기도 하고 알 수 없는
버그가 많아 픽스가 어려울 것이라 판단하여 구글에서 제공하는 맵 서비스를 이용하고자 한다.
```

### google_maps_flutter 플러그인 설치와 의존성 추가
```
먼저 pub.dev의 google_maps_flutter 2.2.3 버전에 따라 플러그인을 설치하고 의존성을 추가해준다.
```

### 구글 클라우드에서 API 발급
```
네이버 클라우드 플랫폼에서 Client 키를 발급받아서 사용했던 것처럼 구글 역시도 API 키라는 것을
발급받아야 하는 것 같다. google cloud console 에서 프로젝트를 생성한 후에 API 및 서비스 -> 라이브러리에서
Maps SDK for Android 사용 버튼을 눌러준다. 그 후 활성화된 관리버튼->사용자인증정보에서 API키를 수정해준다.
```

### SHA-1 인증서 디지털 지문 얻기
```
Maps API key 수정 페이지에서 안드로이드 앱의 사용량 제한을 추가해줘야한다.
이때 두 가지가 필요한데 하나는 앱의 패키지 이름과 SHA-1 인증서 디지털 지문이 필요하다.
패키지이름은 쉽게 얻을 수 있으나 SHA-1 인증서 디지털 지문은 처음 해보는 것이라 정리해보았다.

cmd 하나를 키고 c:\users\[users]\.android로 이동한다. 
keytool -list -v -keystore debug.keystore를 입력하고 비밀번호에 android를 입력하면
SHA-1 인증서 디버그용 지문을 얻을 수 있다. 확인 버튼을 누르면 정상적으로 API키가 발급되었음을 알 수 있다.
```
![image](https://user-images.githubusercontent.com/58906858/212579135-724b52fb-33d2-4166-8cd3-62deb0f36769.png)

### 안드로이드 빌드 수정
```
앱 프로젝트에서 빌드를 건들어야 할 것이 몇 가지 있다.
앱 수준의 build.gradle 파일을 다음과 같이 수정해준다.
android {
	compileSdkVersion flutter.compileSdkVersion
    
    defaultConfig {
    	minSdkVersion 20
        targetSdkVersion 30
        versionCode flutterVersionCode.toInterger()
        versionName flutterVersionName
   }
 }
 
 그 후 app/src/main/AndroidManifest.xml 파일에서 meta-data를 추가한다.
 <meta-data android:name="com.google.android.geo.API_KEY"
           android:value="발급받은 api 키"/>
 ```
 
