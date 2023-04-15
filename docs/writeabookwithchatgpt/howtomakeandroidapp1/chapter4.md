---
layout : default
title : chapter4
parent : How to make android app with KB api and naver map api
grand_parent : Write a book with ChatGPT 
---

# Chapter 4. 안드로이드 앱에서 KB부동산 API와 Naver Map API 활용하기
안드로이드 앱에서 KB부동산 API와 Naver Map API를 활용하여 부동산 정보와 지도 정보를 효과적으로 제공할 수 있습니다. KB부동산 API는 아파트 실거래가 정보를 제공하며, 이를 활용하여 앱 사용자에게 부동산 시장 동향을 제공할 수 있습니다. Naver Map API는 다양한 지도와 위치 정보 서비스를 제공하며, 이를 활용하여 부동산 매물 위치를 지도 상에 표시하고, 앱 사용자에게 부동산 주변 시설 정보를 제공할 수 있습니다. 이러한 API를 활용하여 부동산 앱을 개발하면, 사용자가 보다 쉽고 편리하게 부동산 정보를 검색하고, 매물 위치와 주변 시설 정보를 확인할 수 있습니다. 또한, 앱 사용자들이 부동산 시장 동향을 쉽게 파악할 수 있기 때문에 부동산 시장에 대한 전반적인 이해도 높일 수 있습니다. 따라서, 부동산 앱 개발 시 KB부동산 API와 Naver Map API의 활용은 필수적인 요소 중 하나입니다.

## Retrofit을 이용한 KB부동산 API 호출
안드로이드 앱에서 Retrofit을 이용해 KB부동산 API를 호출하는 방법을 단계별로 설명하고, 자바로 예제 코드를 만들어 보겠습니다.

1. Retrofit 라이브러리 추가하기
먼저 Retrofit 라이브러리를 추가해야 합니다. 이를 위해서는 build.gradle 파일에 아래와 같이 추가해줍니다.

``` javascript
dependencies {
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
}
```
2. 인터페이스 정의하기
KB부동산 API는 REST API를 제공합니다. 이를 호출하기 위해서는 인터페이스를 정의해야 합니다. 예를 들어 아파트 실거래가 조회 API를 호출하기 위해서는 아래와 같이 인터페이스를 정의할 수 있습니다.

``` java
public interface AptTradeApi {
    @GET("realty/GetCodeList")
    Call<ApiResponse<CodeList>> getCodeList(
            @Query("service") String service,
            @Query("numOfRows") int numOfRows,
            @Query("pageNo") int pageNo,
            @Query("LAWD_CD") String laWdCd
    );
}
```
위 인터페이스에서는 @GET 어노테이션을 이용해 호출할 API의 HTTP 메서드와 API 경로를 정의합니다. 이후 메서드 인자로 @Query 어노테이션을 이용해 쿼리 파라미터를 추가합니다.

3. Retrofit 객체 생성하기
Retrofit 객체를 생성합니다. 이때 Retrofit.Builder를 이용해 객체를 생성하고, .baseUrl()을 이용해 API 서버의 주소를 정의합니다.

``` java
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://openapi.kbstar.com/")
    .addConverterFactory(GsonConverterFactory.create())
    .build();
```

4. API 호출하기
인터페이스를 이용해 API를 호출합니다. 이때 Retrofit 객체를 이용해 인터페이스의 구현체를 생성하고, 해당 구현체를 이용해 API를 호출합니다. 호출 결과는 Call 객체에 담겨 반환됩니다.
``` java
AptTradeApi aptTradeApi = retrofit.create(AptTradeApi.class);
Call<ApiResponse<CodeList>> call = aptTradeApi.getCodeList("AptTradeDaejeon", 10, 1, "30110");
```

5. 응답 처리하기
API를 호출한 결과를 처리합니다. 이때 enqueue() 메서드를 이용해 비동기적으로 처리하거나, execute() 메서드를 이용해 동기적으로 처리할 수 있습니다. 예를 들어 아래 코드는 enqueue() 메서드를 이용해 비동기적으로 호출 결과를 처리하는 예제입니다.

``` java
call.enqueue(new Callback<ApiResponse<CodeList>>() {
    @Override
    public void onResponse(Call<ApiResponse<CodeList>> call, Response<ApiResponse<CodeList>> response) {
        if (response.isSuccessful()) {
            ApiResponse<CodeList> apiResponse = response.body();
```

## 응답 데이터 처리하기
안드로이드 앱에서 Retrofit을 이용해 KB부동산 API를 호출하고 응답받은 데이터를 처리하는 방법은 다음과 같습니다.

1. API 인터페이스 생성
- KB부동산 API에서 제공하는 요청 API에 따라 인터페이스를 생성합니다.
- 예를 들어 아파트매매 실거래가 조회 API의 경우 다음과 같은 인터페이스를 생성할 수 있습니다.

``` java
public interface AptTradeApiService {
    @GET("/realty/GetTradeInfo")
    Call<AptTradeResponse> getTradeInfo(
        @Query("serviceKey") String apiKey,
        @Query("date") String date,
        @Query("region") String region,
        @Query("sigunguCode") String sigunguCode,
        @Query("bjdongCode") String bjdongCode,
        @Query("aptNo") String aptNo,
        @Query("pageNo") int pageNo,
        @Query("numOfRows") int numOfRows,
        @Query("fromAmt") int fromAmt,
        @Query("toAmt") int toAmt
    );
}
```

2. Retrofit 라이브러리를 이용해 API 호출하기
- Retrofit 객체를 생성하고, API 인터페이스를 구현하는 객체를 생성합니다.
- API 호출에 필요한 매개변수를 설정하고, enqueue() 메서드를 이용해 비동기적으로 요청합니다.

``` java
Retrofit retrofit = new Retrofit.Builder()
        .baseUrl("https://api.kbstar.com")
        .addConverterFactory(GsonConverterFactory.create())
        .build();

AptTradeApiService service = retrofit.create(AptTradeApiService.class);

Call<AptTradeResponse> call = service.getTradeInfo(
        "API_KEY",
        "20220401",
        "A",
        "11",
        "1168010300",
        "A11752002",
        1,
        10,
        0,
        99999999
);

call.enqueue(new Callback<AptTradeResponse>() {
    @Override
    public void onResponse(Call<AptTradeResponse> call, Response<AptTradeResponse> response) {
        AptTradeResponse result = response.body();
        // 응답 데이터 처리
    }

    @Override
    public void onFailure(Call<AptTradeResponse> call, Throwable t) {
        // API 호출 실패 처리
    }
});
```

3. 응답 데이터 처리하기
- onResponse() 메서드에서 응답 받은 데이터를 처리합니다.
- Gson 라이브러리를 이용해 JSON 형태의 응답 데이터를 자바 객체로 변환할 수 있습니다.

``` java
public class AptTradeResponse {
    private Body body;

    // getter, setter
}

public class Body {
    private Items items;

    // getter, setter
}

public class Items {
    private List<Item> item;

    // getter, setter
}

public class Item {
    private String dealAmount; // 실거래가액
    private String buildYear; // 건축년도
    private String area; // 전용면적
    private String dong; // 법정동명
    private String sigungu; // 시군구명
    private String bunji; // 지번
    private String aptName
```

## Naver Map API와 연동하기
안드로이드 앱에서 Retrofit을 이용하여 KB부동산 API를 호출하고, 응답 데이터를 받아와서 Naver Map API를 활용해 지도에 표시하는 방법은 다음과 같습니다.

1. 먼저 Retrofit과 Gson 라이브러리를 build.gradle 파일에 추가합니다.

``` javascript
dependencies {
    // Retrofit 라이브러리
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'

    // Gson 라이브러리
    implementation 'com.google.code.gson:gson:2.8.6'
}
```

2. KB부동산 API 인증키를 발급받고, Retrofit 인터페이스를 정의합니다.

``` java
public interface KBApiService {
    String BASE_URL = "http://openapi.kbstar.com/";
    String API_KEY = "인증키 입력";

    @GET("openapi/service/KamcoAPTTradeInfoSvc/getAPTTradeList")
    Call<ApiResponse> getAptTradeList(
            @Query("ServiceKey") String apiKey,
            @Query("DEAL_YMD") String dealYmd,
            @Query("LAWD_CD") String lawdCd,
            @Query("DEAL_TYPE") String dealType,
            @Query("START_ROW") int startRow,
            @Query("END_ROW") int endRow
    );
}
```

3. Retrofit 객체를 생성하고, API 인터페이스를 이용해 API를 호출합니다.

``` java
private void callKbApi() {
    Retrofit retrofit = new Retrofit.Builder()
            .baseUrl(KBApiService.BASE_URL)
            .addConverterFactory(GsonConverterFactory.create())
            .build();

    KBApiService apiService = retrofit.create(KBApiService.class);
    Call<ApiResponse> call = apiService.getAptTradeList(
            KBApiService.API_KEY,
            "202110",
            "11680",
            "A1",
            1,
            10
    );

    call.enqueue(new Callback<ApiResponse>() {
        @Override
        public void onResponse(Call<ApiResponse> call, Response<ApiResponse> response) {
            if (response.isSuccessful() && response.body() != null) {
                ApiResponse apiResponse = response.body();
                List<Trade> tradeList = apiResponse.getTradeList();
                // Trade 데이터를 활용하여 Naver Map API 호출 및 지도에 표시
                showOnMap(tradeList);
            }
        }

        @Override
        public void onFailure(Call<ApiResponse> call, Throwable t) {
            // API 호출 실패 시 처리
        }
    });
}
```

4. 받아온 데이터를 활용하여 Naver Map API를 호출하고, 지도에 마커와 인포윈도우를 표시합니다.

``` java
private void showOnMap(List<Trade> tradeList) {
    NaverMapSdk.getInstance(this).setClient(
            new NaverMapSdk.NaverCloudPlatformClient("NAVER MAPS API 클라이언트 아이디"));
    setContentView(R.layout.activity_main);
    MapView mapView = findViewById(R.id.map_view);
    mapView.onCreate(savedInstanceState);
    mapView.getMapAsync(naverMap -> {
        for (Trade trade : tradeList) {
            Marker marker = new Marker();
            LatLng position = new LatLng(trade.getLat(), trade.get
```

## 지도에 아파트 실거래가 정보 표시하기