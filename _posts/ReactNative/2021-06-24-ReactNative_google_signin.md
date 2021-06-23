---
title: React Native - Mail
categories: [ReactNative]
comments: true
---

### Firebase Auth && Google Signin

`contestbox` 프로젝트를 진행하면서, 구글 이메일 통한 로그인을 통해 앱 진입 시 로그인 과정을 선택할 수 있도록 했습니다. 해당 로그인 관리는 `Firebase` 에서 제공하는 `Athentication` 기능을 활용했습니다. 이 기능을 적용해보면서 과정을 작성해 보겠습니다.

## 1. 필요한 라이브러리 설치

firebase와 연동하기 위해 먼저 프로젝트에 두 가지 라이브러리를 설치해 주어야 합니다.

```jsx
yarn add react-native-firebase/auth
yarn add react-native-firebase/app
```

## 2. firebase 콘솔에 앱 등록

firebase 콘솔에 앱을 등록해 주어야 합니다. 앱을 등록하기 위해서는 다음과 같은 단계를 거치게 됩니다.

1. 앱 등록

![https://user-images.githubusercontent.com/57908055/121779014-92b87700-cbd4-11eb-8ae8-618546587ebc.png](https://user-images.githubusercontent.com/57908055/121779014-92b87700-cbd4-11eb-8ae8-618546587ebc.png)

이때 패키지 이름은 /android/app/src/main/AndroidManifest.xml 파일에서 확인할 수 있으며, 디버그 서명 인증서를 등록하려면 다음과 같은 명령어로 확인할 수 있습니다. android로 구글 로그인 기능을 사용하기 위해서는 서명 인증서가 필요합니다.

```jsx
cd android/app
gradlew signingReport
```

2. 구성파일 다운로드

![https://user-images.githubusercontent.com/57908055/121779111-022e6680-cbd5-11eb-9bc3-b1b3cf5fbfc1.png](https://user-images.githubusercontent.com/57908055/121779111-022e6680-cbd5-11eb-9bc3-b1b3cf5fbfc1.png)

사진과 같이 파일을 다운로드하여 app 디렉토리에 넣어줍니다.

3. Firebase SDK 추가

```jsx
// 프로젝트 수준의 build.gradle 파일 수정

buildscript {
  repositories {
    // Check that you have the following line (if not, add it):
    google()  // Google's Maven repository
  }
  dependencies {
    ...
    // Add this line
    classpath 'com.google.gms:google-services:4.3.8'
  }
}

allprojects {
  ...
  repositories {
    // Check that you have the following line (if not, add it):
    google()  // Google's Maven repository
    ...
  }
}
```

```jsx
// app 수준의 build.gradle 파일 수정

apply plugin: 'com.android.application'
// Add this line
apply plugin: 'com.google.gms.google-services'

dependencies {
  // Import the Firebase BoM
  implementation platform('com.google.firebase:firebase-bom:28.1.0')

  // Add the dependency for the Firebase SDK for Google Analytics
  // When using the BoM, don't specify versions in Firebase dependencies
  implementation 'com.google.firebase:firebase-analytics'

  // Add the dependencies for any other desired Firebase products
  // https://firebase.google.com/docs/android/setup#available-libraries
}
```

### 3. Firebase 콘솔에 Authentication 시작하기

![https://user-images.githubusercontent.com/57908055/121779341-1d4da600-cbd6-11eb-8c86-84444138452a.png](https://user-images.githubusercontent.com/57908055/121779341-1d4da600-cbd6-11eb-8c86-84444138452a.png)

Firebase 콘솔에서 google 로그인 사용을 켜주면 준비가 완료됩니다.

여기까지 파이어베이스와 앱 연결이 완료되었으며, 이후로 구글 로그인을 ReactNative 프로젝트에 활성화하는 과정입니다.

### 4. 구글로그인 라이브러리 설치

구글 로그인을 위한 라이브러리는 다음과 같이 설치할 수 있습니다.

```jsx
yarn add @react-native-google-signin/google-signin

// @react-native-community/google-signin 라이브러리는 현재 deprecated 입니다.
```

### 5. 구글 로그인을 위한 Android파일 설정

해당 과정은 위의 과정에서 진행된 부분도 있고 추가해야 하는 부분도 있습니다. 구글 로그인 라이브러리를 사용하기에 앞서 다시한번 파일을 확인해 줍니다.

```jsx
// android/build.gradle
buildscript {
    ext {
        buildToolsVersion = "27.0.3"
        minSdkVersion = 16
        compileSdkVersion = 27
        targetSdkVersion = 26
        supportLibVersion = "27.1.1"
        googlePlayServicesAuthVersion = "16.0.1" // <--- use this version or newer
    }
...
    dependencies {
        classpath 'com.android.tools.build:gradle:3.1.2' // <--- use this version or newer
        classpath 'com.google.gms:google-services:4.1.0' // <--- use this version or newer
    }
...
allprojects {
    repositories {
        mavenLocal()
        google() // <--- make sure this is included
        jcenter()
        maven {
            // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
            url "$rootDir/../node_modules/react-native/android"
        }
    }
}

// android/app/build.gradle
...
dependencies {
    implementation fileTree(dir: "libs", include: ["*.jar"])
    implementation "com.facebook.react:react-native:+"
    implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.0.0' // <-- add this; newer versions should work too
}

apply plugin: 'com.google.gms.google-services' // <--- this should be the last line

```

이후 link를 한 경우(version <= 0.59) 추가적인 과정이 필요합니다.

[공식 문서 참고](<[https://github.com/react-native-google-signin/google-signin/blob/master/docs/android-guide.md](https://github.com/react-native-google-signin/google-signin/blob/master/docs/android-guide.md)>)

### 6. React Native 예제 작성

1. WebClientId 초기화

   다운받은 json 파일에서 확인하거나 파이어베이스 콘솔의 구글로그인 메뉴에서 확인 할 수 있습니다.

   **해당 과정에서 공식 문서에서 web client id를 google-services.json의 oauth-client를 통해 확인하라고 했는데, 계속해서 Developer Error가 발생하여 혹시나 하고 other_platform_oauth_client id를 적용해 보니 error를 해결할 수 있었습니다.**

   ```jsx
   import { GoogleSignin } from "@react-native-google-signin/google-signin";

   GoogleSignin.configure({
     webClientId: "",
   });
   ```

2. App.tsx

```jsx
const App =()=>{
  const [initializing, setInitializing] = useState<boolean>(true);
  const [user, setUser] = useState();

  // 로그인상태 확인
  function onAuthStateChanged(user) {
    setUser(user);
    if (initializing) setInitializing(false);
  }
  useEffect(()=>{
    ...
    // webclientId 초기화
    GoogleSignin.configure({
      webClientId :'------',
      offlineAccess:true
    });
    // login상태 확인
    const subscriber = auth().onAuthStateChanged(onAuthStateChanged);
    return subscriber; // unsubscribe on unmount

  },[])
  if (initializing) return null;
  if (!user) {
    return (
      ...
			// signin 이전 화면

    );
  }
  return (
    // signin 이후 화면
  );
};
```

3. SignIn / SignOut 함수

```python
export const SignIn = async () => {

  try {
    await GoogleSignin.hasPlayServices();
    const { idToken } = await GoogleSignin.signIn();
    const googleCredential = auth.GoogleAuthProvider.credential(idToken);
    return auth().signInWithCredential(googleCredential);
  } catch (error) {
    if (error.code === statusCodes.SIGN_IN_CANCELLED) {
      console.log('로그인 취소')
    } else if (error.code === statusCodes.IN_PROGRESS) {
      console.log('이미 로그인 상태')
    } else if (error.code === statusCodes.PLAY_SERVICES_NOT_AVAILABLE) {
      console.log('서비스 이상')
    } else {
      console.log('다른 문제')
      console.log(error)
    }
  }
};

export const SignOut = async () => {
    auth()
  .signOut()
  .then(() => console.log('User signed out!'));
};
```

### 실행 영상

[![Google_Signin_ex](https://youtu.be/e-y4J7Ln_1E)](https://youtu.be/e-y4J7Ln_1E)

## 참고 공식 문서

[react-native-google-signin/google-signin](<[https://github.com/react-native-google-signin/google-signin](https://github.com/react-native-google-signin/google-signin)>)

[React Native Firebase Auth](<[https://rnfirebase.io/auth/usage](https://rnfirebase.io/auth/usage)>)
