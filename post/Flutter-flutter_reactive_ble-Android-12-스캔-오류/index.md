---
title: "[Flutter] flutter_reactive_ble Android 12 스캔 오류"
date: 2022-05-01T18:38:54+09:00
slug: "flutter-reactive-ble-android-12-scan-failure"
draft: false
---

## TL;DR  

[PR](https://github.com/PhilipsHue/flutter_reactive_ble/pull/554) 해둔 상태인데, 머지가 될지, 얼마나 걸릴지는 모르겠다...  

2022년 5월 1일 기준 [flutter_reactive_ble](https://pub.dev/packages/flutter_reactive_ble) 패키지는 안드로이드 12이상(API >= 31)에서 위치를 위한 블루투스 스캔이 불가능하다.  
패키지에서 안드로이드에서 블루투스 스캔 구현을 위해 사용하는 라이브러리 [RxAndroidBLE](https://github.com/dariuszseweryn/RxAndroidBle)는 기본적으로 'neverForLocation'을 전제로 블루투스 스캔을 하기 때문이다.  

```xml
    <uses-permission
        android:name="android.permission.BLUETOOTH_SCAN"
        android:usesPermissionFlags="neverForLocation"
        tools:targetApi="s" />
```

문제가 되는 해당 라이브러리의 AndroidManifest 파일이다. neverForlocation이 명시되어있고, 이를 해결하기 위해서는 우리 앱의 AndroidManifest에서 해당 플래그를 제외해주면 된다.  

```xml
<uses-permission android:name="android.permission.BLUETOOTH_SCAN" 
                     tools:remove="android:usesPermissionFlags"
                     tools:targetApi="s" />
```

## 주절주절

[inner - 이너](https://apps.apple.com/us/app/inner-%EC%9D%B4%EB%84%88/id1619472927) 앱의 디데이 맞춘 출시를 위해 이거빼고 저거빼서 앱스토어에 올렸더니 심사 지침 4.2에 걸려 반려당했다...  
>앱에는 웹 사이트를 단순히 바꾼 수준을 넘어서는 기능, 콘텐츠, UI가 있어야 합니다. 특별히 유용하거나 고유하지 않은, '앱 같지 않은' 앱은 App Store에 등록할 수 없습니다. 지속적인 오락적 가치 또는 적절한 유용성을 제공하지 못하는 앱은 거부될 수 있습니다. 단순히 노래 한 곡이나 동영상 한 편을 담은 앱은 iTunes Store에 제출해야 합니다. 책이나 게임 설명서는 Apple Books Store에 제출해야 합니다.  

자동차를 만들기전에 보드부터 만들라 하였던가. 그런데 기능을 빼다보니 보드가 아니라 바퀴가 되었나보다. 출시일이 얼마 남지 않았기에 기획분의 마음은 상당히 다급한 상태였고, 우리가 섹터를 보여주는데 사용하는 카드의 갯수를 늘려 해결하자는 주장을 하셨다. 앱스토어 심사 지침이 귀에 걸면 귀걸이 코에 걸면 코걸이라지만, 반려에 대한 해결책으로 삼는다는게 이런거라니?!

우리가 반려당한 이유는 feature의 부재였지, 컨텐츠의 부재가 아니었다. 같은 수준의 컨텐츠가 많아진다고 feature가 생기는 것이 아니다. 여전히 웹사이트 수준의 **보여주는 앱**인 것이고, 여전히 반려의 리스크가 존재할 것이다.

실내 측위 회사로서 [TJLABS](https://tjlabscorp.com)의 가장 큰 특징이자 차별점은 offline hardware와 interaction하는 점이고, 앞으로의 제품들에도 개발력의 한계로 인해 미뤄왔지만 반드시 포함되어야할 기능이기도 하다. 당시 상황에서 판단하기에, 핸드폰의 하드웨어와의 상호작용이 이루어지기 때문에 **앱 같지 않은 앱**이 아니라고 증명하기에 충분한 기능이고, 그런 기능 중 가장 구현이 쉬운 것이라고 생각하였고 이번 릴리즈에 해당 부분을 포함 시킬 것을 강력하게 주장하였다.

이너 앱 개발 중 위치를 위한 블루투스 flutter 관련 라이브러리 서치 후 나온 후보군은 3이었다.

- [flutter_blue](https://pub.dev/packages/flutter_blue) : 패키지가 1.0에 아직 도달하지 못한 상태이고, `Please be fully prepared to deal with breaking changes. This package must be tested on a real device.`라고 하는데, 굳이 위험성을 감수하고 싶지 않았다. 활성화는 잘 되어있고, 그 덕에 이슈도 많은데 선택 당시에는 많은 이슈 또한 단점으로 보이게 했다.
- [flutter_ble_lib](https://pub.dev/packages/flutter_ble_lib) : null safety 적용도 안됐다...  
- [flutter_reactive_ble](https://pub.dev/packages/flutter_reactive_ble) : 당시에는 가장 좋은 솔루션이라고 생각을 했다.  

당시에는 자료 조사를 할 시간도 많지 않았고, `flutter_reactive_ble`가 꽤 괜찮은 패키지라고 생각하고 진행하였다. 우리는 BLE장치와의 별도의 연결 등은 필요 없고, 스캔만 하면 됐기 때문에, Holden에게 작업을 부탁했고 아이폰과 안드로이드폰 양쪽에서 스캔을 구현하는데 오래 걸리지 않았다. 그런데 안드로이드 12 디바이스들에서 우리 비컨이 스캔이 되지 않는 것이다... [Android 12의 새 블루투스 권한](https://developer.android.com/guide/topics/connectivity/bluetooth/permissions) 관련한 문제인 것은 분명했고, 위치를 사용하는 우리 시스템에 맞춰서 필요한 manifest 세팅을 해주었음에도 작동을 안하는 상황이었다.

출시가 되지 않은 것은 `App Store` 쪽 뿐이었기에, `Android`에서만 발생한 해당 이슈는 우선 묻어두고 출시 일정에 맞추어 `iOS`쪽에만 서둘러 해당 기능을 배포하였고, 일정을 지켜서 심사에 통과할 수 있었다.

홀든이 이래저래 고생하면서 찾아낸 원인은, `flutter_reactive_ble` 패키지 속 `RxAndroidBLE`의 `AndroidManifest`에 `neverforlocation`이 이미 선언되어있기 때문에, 밖에서 위치를 위한 사용이 안되는 것이었다. 해당 부분에 대한 권한 설정을 외부 manifest에서 제거하는 방식으로 해결했다. 해당 라이브러리에서는 이 사실을 명시해두었으나, 라이브러리를 사용하는 `flutter_reactive_ble` 패키지에는 이 사실이 명시되어 있지 않아있어, 해당 부분에 대해 [PR](https://github.com/PhilipsHue/flutter_reactive_ble/pull/554) 해두었다.  

`AndroidManifest`는 중복되더라도 자동으로 병합되기 때문에, 안드로이드 라이브러리들이나 플러터 패키지들에서 내부적으로 선언을 다 해둔 것 같다. 그런데 안드로이드 12 위치를 위한 블루투스 스캔의 특수성 때문에 해당 오류가 발생한 것으로 여겨지는데, 이런식의 병합 오류가 발생하도록 새로운 블루투스 권한을 설계한 것이 잘 이해가 가지 않는다.  

버전마다 상당한 부분이 변경되는 안드로이드의 특성을 다시 한번 경험하며, 앞으로 플러터 기반의 크로스플랫폼 기반의 **이너** 앱 개발에 있어서도, 기존에 계획한 것 처럼 하드웨어 관련 부분은 네이티브 기준으로 구성할 필요가 있다는 것을 다시 느낄 수 있었다.  
