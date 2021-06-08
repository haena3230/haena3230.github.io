---
title: React Native - Mail
categories: [ReactNative]
comments: true
---

## 앱에서 mail 송신 기능 사용하기

`contestbox` 프로젝트를 진행하며, `error` 발생 시 오류 페이지와 함께 고객의 피드백을 받을 수 있도록 메일 송신 기능을 넣는 방법을 알아보았습니다. 우선 `안드로이드` 버전의 사용 방법을 알아보겠습니다.

### 1. 설치

'react-native-mail' 이라는 라이브러리를 활용했습니다.

```jsx
// npm
npm install --save react-native-mail
// yarn
yarn add react-native-mail
```

### 2. 안드로이드 관련 설정

- 자동 설정

다음의 명령어를 통해 자동으로 안드로이드 설정을 진행할 수 있습니다.

```jsx
react-native link react-native-link
```

- 수동 설정

수동 설정을 위해서는 파일을 변경해 주어야 합니다.

```jsx
// android/setting.gradle
...
include ':RNMail', ':app'
project(':RNMail').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-mail/android')
```

```jsx
// android/app/build.gradle
...
dependencies {
    ...
    compile project(':RNMail')
}
```

```jsx
// MainActivity가 Activity를 확장하는 경우 : MainActivity.java에 모듈 등록
import  com.chirag.RNMail. * ;  // <--- import

public  class  MainActivity  extends  Activity  implements  DefaultHardwareBackBtnHandler {
   ......

  @Override
  protected  void  onCreate ( Bundle  savedInstanceState ) {
     super . onCreate (savedInstanceState);
    mReactRootView =  새로운  ReactRootView ( this );

    mReactInstanceManager =  ReactInstanceManager . 빌더 ()
      .setApplication (getApplication ())
      .setBundleAssetName ( " index.android.bundle " )
      .setJSMainModuleName ( " index.android " )
      .addPackage ( new  MainReactPackage ())
      .addPackage ( new  RNMail ())               // <---- -여기에 추가하십시오.
      .setUseDeveloperSupport ( BuildConfig . DEBUG )
      .setInitialLifecycleState ( LifecycleState . RESUMED )
      .build ();

    mReactRootView .startReactApplication (mReactInstanceManager, " ExampleRN " , null );

    setContentView (mReactRootView);
  } ......
}
```

```jsx
// MainActivity가 ReactActivity를 확장하는 경우 : MainApplication.java에 모듈 등록
import  com.chirag.RNMail. * ; // <--- import

public  class  MainApplication  extends  Application  implements  ReactApplication {
     ....

    @Override
    protected  List < ReactPackage >  getPackages () {
       return  Arrays . < ReactPackage > asList (
           new  MainReactPackage (),
           new  RNMail ()       // <------ 여기에 추가
      );
    }
  };
```

### 3. 함수 작성

함수 작성 시 다음과 같은 설정이 가능합니다.

- subject: 이메일의 제목
- recipients: 이메일을 수신하는 이메일의 리스트
- ccRecipients: cc로 수신하는 이메일의 리스트
- bccRecipients: bcc로 수신하는 이메일의 리스트
- body: 이메일의 본문
- isHTML: 이메일의 본문이 HTML 형식인지 여부
- attachment: 첨부 파일이 있는 경우 사용
  - path: 파일의 위치
  - type: 파일의 mime type
  - name(Optional): 사용자 정의 파일 이름

첨부가 가능한 파일의 mime type은 다음과 같습니다.

- jpg
- png
- doc
- docx
- ppt
- pptx
- html
- csv
- pdf
- vcard
- json
- zip
- text
- mp3
- wav
- aiff
- flac
- ogg
- xls
- xlsx

```jsx
// 함수 작성 예제
handleEmail = () => {
  Mailer.mail(
    {
      subject: "need help",
      recipients: ["support@example.com"],
      ccRecipients: ["supportCC@example.com"],
      bccRecipients: ["supportBCC@example.com"],
      body: "<b>A Bold Body</b>",
      isHTML: true,
      attachment: {
        path: "", // The absolute path of the file from which to read data.
        type: "", // Mime Type: jpg, png, doc, ppt, html, pdf, csv
        name: "", // Optional: Custom filename for attachment
      },
    },
    (error, event) => {
      Alert.alert(
        error,
        event,
        [
          {
            text: "Ok",
            onPress: () => console.log("OK: Email Error Response"),
          },
          {
            text: "Cancel",
            onPress: () => console.log("CANCEL: Email Error Response"),
          },
        ],
        { cancelable: true }
      );
    }
  );
};
```

### 사용 예제

[ReactNative_MAil_ex](https://youtu.be/aHD2YGEiw-s)

### [공식 문서](https://www.npmjs.com/package/react-native-mail](https://www.npmjs.com/package/react-native-mail)
