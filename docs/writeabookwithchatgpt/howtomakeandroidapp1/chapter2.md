---
layout : default
title : chapter2
parent : How to make android app with KB api and naver map api
grand_parent : Write a book with ChatGPT 
---

# Chapter 2. Naver Map API 기본 개념

Naver Map API는 Naver 지도 서비스를 웹과 모바일 어플리케이션에서 사용할 수 있도록 제공하는 API이다. Naver Map API를 사용하면, 개발자는 지도를 표시하고, 지도 위에 마커나 인포윈도우와 같은 요소를 추가할 수 있다.

API를 사용하기 위해서는 먼저 API 키를 발급해야 한다. 발급된 API 키는 인증된 사용자만이 API를 사용할 수 있도록 보안을 제공한다.

API를 사용하여 지도를 표시하기 위해서는 먼저 해당 영역의 좌표와 화면 크기를 설정해야 한다. 이후, API를 사용하여 Naver 지도를 호출하고, 해당 좌표에 대한 지도를 표시한다.

API를 사용하여 지도를 제어하기 위해서는 지도의 줌 레벨과 중심 좌표를 조절할 수 있다. 또한, API를 사용하여 지도의 보기 모드, 위성 지도 및 건물 이름 등의 여러 옵션을 설정할 수 있다.

마지막으로, API를 사용하여 마커와 인포윈도우를 추가할 수 있다. 마커는 지도 위의 특정 위치를 나타내며, 인포윈도우는 마커를 클릭할 때 나타나는 정보 창을 의미한다.

Naver Map API를 사용하면, 쉽게 지도를 표시하고 제어할 수 있으며, 마커와 인포윈도우와 같은 요소를 추가하여 지도를 더욱 풍부하게 표현할 수 있다.

## Naver Map API 소개

Naver Map API는 네이버에서 제공하는 지도 API로, 다양한 기능을 제공하여 안드로이드 앱 개발에서 지도를 쉽게 구현할 수 있습니다. 네이버 Map API를 이용하여 지도, 위치 검색, 경로 검색 등 다양한 기능을 구현할 수 있습니다.

Naver Map API는 REST API 형태로 제공되며, JSON 형식으로 응답을 받습니다. 개발자는 API를 호출하여 서버로부터 반환된 JSON 형식의 응답을 파싱하여 앱에서 사용할 수 있습니다.

Naver Map API는 안드로이드 스튜디오와 같은 개발 도구에서 쉽게 사용할 수 있습니다. 안드로이드 앱에서 지도를 구현할 때는 MapView를 이용하여 지도를 화면에 표시하며, 다양한 제어 기능과 이벤트를 제공하여 지도의 동작을 제어할 수 있습니다.

또한, Naver Map API는 안드로이드 앱 개발을 위한 SDK를 제공하여 개발자가 쉽게 지도 기능을 추가할 수 있도록 지원합니다. 이 SDK를 사용하면 개발자는 지도를 표시하거나 이벤트를 처리하는 등의 작업을 쉽게 할 수 있습니다.

Naver Map API는 다양한 기능과 문서, 예제 코드를 제공하여 안드로이드 앱 개발자들이 쉽게 사용할 수 있도록 지원합니다. 또한, 사용 시 발생하는 문제나 질문에 대해서는 개발자 포럼을 통해 지원하여 안정적인 지도 기능 제공을 지향하고 있습니다.

## API 키 발급하기
Naver Map API를 사용하려면 먼저 API Key를 발급 받아야 합니다. API Key는 해당 앱에서 Naver Map API를 호출할 수 있는 인증 키입니다. API Key를 발급 받는 방법은 다음과 같습니다.

>1. Naver 개발자센터에 가입한다.
>1. 프로젝트를 생성하고, '지도/로컬 API'를 선택한다.
>1. '지도/로컬 API'의 하위 항목인 '네이버 지도 API'를 선택한다.
>1. '사용 신청' 버튼을 클릭한다.
>1. '애플리케이션 등록'을 한다.
>1. 'Client ID'와 'Client Secret'을 발급 받는다.
>1. 발급 받은 'Client ID'와 'Client Secret'을 사용하여 앱에서 API Key를 발급 받는다.

아래는 Python 코드로 API Key를 발급 받는 예시입니다.

``` python
import requests
import json

client_id = "발급받은 Client ID"
client_secret = "발급받은 Client Secret"

url = "https://nid.naver.com/oauth2.0/token?grant_type=client_credentials&client_id=" + client_id + "&client_secret=" + client_secret

response = requests.get(url)
response_dict = json.loads(response.text)

access_token = response_dict['access_token']
```
위의 예시에서 발급 받은 'Client ID'와 'Client Secret'을 변수로 지정한 후, API Key를 발급 받기 위한 URL을 만들어 GET 요청을 보내고 응답에서 발급 받은 'access_token'을 추출합니다. 이렇게 발급 받은 API Key는 Naver Map API를 호출할 때 사용됩니다.

### Java code
Naver Map API Key를 발급 받는 방법은 다음과 같습니다.

>1. Naver Cloud Platform에서 로그인하거나 회원가입을 합니다.
>1. 로그인 후, Console 페이지에서 '애플리케이션 추가' 버튼을 클릭합니다.
>1. 애플리케이션 이름과 설명을 입력하고, '애플리케이션 추가' 버튼을 클릭합니다.
>1. 애플리케이션이 추가되면 '서비스 추가' 버튼을 클릭합니다.
>1. '네이버 지도'를 선택하고, 'API 설정' 버튼을 클릭합니다.
>1. '사용 설정' 스위치를 켜고, '서비스 키 생성' 버튼을 클릭합니다.
>1. API Key가 생성되면 '복사' 버튼을 클릭하여 복사합니다.

이제 API Key를 이용하여 Naver Map API를 사용할 수 있습니다. 아래는 자바 코드 예제입니다.

``` java
import com.naver.maps.map.NaverMapSdk;
import android.app.Application;

public class MyApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        NaverMapSdk.getInstance(this).setClient(
            new NaverMapSdk.NaverCloudPlatformClient("{YOUR_API_CLIENT_ID}"));
    }
}
```

위 코드에서 {YOUR_API_CLIENT_ID}를 발급 받은 API Key로 대체하면 됩니다. 이제 Naver Map API를 사용할 수 있는 환경이 구성되었습니다.

## 지도 표시하기

안드로이드 앱에서 Naver Map API를 이용하여 지도를 표시하는 방법에 대해 설명하겠습니다. 먼저, 프로젝트 수준의 build.gradle 파일에 아래의 코드를 추가해주어야 합니다.

``` javascript
allprojects {
    repositories {
        google()
        jcenter()
        maven { url 'https://naver.jfrog.io/artifactory/maven' }
    }
}
```

그리고, app 수준의 build.gradle 파일에서는 아래와 같은 의존성을 추가해주어야 합니다.

``` javascript
dependencies {
    implementation 'com.naver.maps:map-sdk:3.13.0'
}
```

이제 Naver Map API key를 발급받아야 합니다. 이전에 설명드린 "Naver Map API Key 발급 받는 방법"에서 앱 등록과 key 발급을 완료하였다면, AndroidManifest.xml 파일에 아래와 같이 메타데이터를 추가합니다.

``` xml
<application>
    ...
    <meta-data
        android:name="com.naver.maps.map.CLIENT_ID"
        android:value="YOUR_CLIENT_ID" />
    ...
</application>
```

이제 코드에서 MapView를 생성하고, NaverMap 객체를 얻어와 지도를 표시합니다. 아래는 예시 코드입니다.

``` java
public class MainActivity extends AppCompatActivity implements OnMapReadyCallback {

    private MapView mapView;
    private NaverMap naverMap;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mapView = findViewById(R.id.map_view);
        mapView.onCreate(savedInstanceState);
        mapView.getMapAsync(this);
    }

    @Override
    public void onMapReady(@NonNull NaverMap naverMap) {
        this.naverMap = naverMap;
    }

    @Override
    protected void onStart() {
        super.onStart();
        mapView.onStart();
    }

    @Override
    protected void onResume() {
        super.onResume();
        mapView.onResume();
    }

    @Override
    protected void onPause() {
        super.onPause();
        mapView.onPause();
    }

    @Override
    protected void onStop() {
        super.onStop();
        mapView.onStop();
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        mapView.onDestroy();
    }

    @Override
    public void onLowMemory() {
        super.onLowMemory();
        mapView.onLowMemory();
    }
}
```

위 코드에서 R.layout.activity_main은 MapView가 추가된 레이아웃 파일입니다. MapView를 생성하고, getMapAsync() 메서드를 호출하여 NaverMap 객체를 얻어옵니다. onMapReady() 메서드에서 얻어온 NaverMap 객체를 통해 지도를 제어할 수 있습니다. 마지막으로 액티비티의 생명주기에 따라 MapView의 상태를 관리하기 위한 코드를 추가하였습니다.

이제 앱을 실행하면 Naver Map API를 이용하여 지도가 표시됩니다.


## 지도 제어하기

Naver Map API를 이용해 안드로이드 어플리케이션에서 지도를 제어하는 방법에 대해 알아보겠습니다.

지도를 제어하는 방법에는 다양한 기능이 있습니다. 대표적으로는 지도 이동, 확대/축소, 마커 추가/삭제, 인포윈도우 표시/숨기기 등이 있습니다.

우선, 지도를 제어하기 위해서는 지도 객체를 생성해야 합니다. 이때, NaverMap 클래스의 객체를 생성하면 됩니다.

``` java
NaverMap naverMap = naverMapFragment.getMap();
```

다음으로, 지도를 제어하기 위한 여러 메소드들을 사용할 수 있습니다. 지도 이동을 제어하는 moveCamera 메소드를 예로 들면 다음과 같습니다.

``` java
CameraUpdate cameraUpdate = CameraUpdate.scrollTo(new LatLng(37.5670135, 126.9783740));
naverMap.moveCamera(cameraUpdate);
```

이 코드는 지도의 위치를 (37.5670135, 126.9783740)으로 이동시킵니다. 마커를 추가하는 addMarker 메소드를 사용하면 다음과 같습니다.

``` java
Marker marker = new Marker();
marker.setPosition(new LatLng(37.5670135, 126.9783740));
marker.setMap(naverMap);
```

이 코드는 (37.5670135, 126.9783740) 위치에 마커를 추가합니다.

이와 같은 방식으로, 다양한 기능들을 사용하여 지도를 제어할 수 있습니다. 다만, API의 사용에 따라 서비스 요금이 부과될 수 있으므로 사용 전에 이에 대해 충분한 확인이 필요합니다.

## 마커와 인포윈도우 추가하기

안드로이드 어플리케이션에서 Naver Map API를 이용해 지도에 마커와 인포윈도우를 추가하는 방법에 대해 알아보자.

먼저, 마커를 추가하기 위해서는 NMap 객체의 addMarker() 메소드를 이용한다. 이때, addMarker() 메소드는 다음과 같은 매개변수를 받는다.

NMapPOIdata : 마커를 추가할 위치 정보 데이터 객체
NMapOverlayItem : 마커를 추가할 위치 정보
따라서, 우선 마커를 추가할 위치 정보를 생성해야 한다. 위치 정보는 위도(latitude)와 경도(longitude) 값을 가지는 NGeoPoint 객체로 표현된다.

다음으로, 인포윈도우를 추가하기 위해서는 NMapPOIdataOverlay 객체를 생성하고 이를 NMapView에 추가해야 한다. NMapPOIdataOverlay 객체는 NMapPOIdata 객체와 NMapView 객체를 인자로 받아 생성된다. 인포윈도우는 해당 마커의 위치 정보와 함께 표시되므로, 마커의 위치 정보를 통해 인포윈도우의 위치를 지정할 수 있다.

아래는 자바 코드 예시이다.

``` java
NMapPOIdata poiData = new NMapPOIdata(1, mMapViewerResourceProvider);
poiData.beginPOIdata(1);
poiData.addPOIitem(new NGeoPoint(37.5670135, 126.9783740), "마커 테스트", NMapPOIflagType.FROM, null);
poiData.endPOIdata();

NMapPOIdataOverlay poiDataOverlay = mMapOverlayManager.createPOIdataOverlay(poiData, null);
poiDataOverlay.setOnStateChangeListener(onStateChangeListener);

NMapPOIitem item = poiData.getPOIitem(0);
if (item != null) {
    NMapPOIdataOverlay.MarkerOverlay marker = poiDataOverlay.getOverlayItem(item.getId());
    if (marker != null) {
        marker.showInfoWindow();
    }
}
```

위 코드에서, 첫 번째 줄에서는 NMapPOIdata 객체를 생성하고 POI 아이템을 추가한다. 두 번째 줄에서는 NMapPOIdataOverlay 객체를 생성하고 POI 데이터와 NMapView 객체를 인자로 전달한다. 마지막으로 인포윈도우를 표시하기 위해 마커의 위치 정보를 얻어 해당 마커에 대한 NMapPOIdataOverlay.MarkerOverlay 객체를 얻은 후 showInfoWindow() 메소드를 호출하여 인포윈도우를 표시한다.

이렇게 마커와 인포윈도우를 추가하면 지도 위에 위치 정보를 시각적으로 나타내고 사용자에게 추가 정보를 제공할 수 있다.