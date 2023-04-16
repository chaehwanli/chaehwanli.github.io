---
layout : default
title : Chapter 3. KB부동산 아파트 실거래가 제공 서비스 활용하기
parent : How to make android app with KB api and naver map api
grand_parent : Write a book with ChatGPT 
---

# Chapter 3. KB부동산 아파트 실거래가 제공 서비스 활용하기
KB부동산은 부동산 거래 정보와 시세 등의 부동산 정보를 제공하는 대표적인 부동산 정보 제공 업체입니다. KB부동산은 실제 아파트 거래 실적을 바탕으로 한 실거래가 정보를 제공하며, 이러한 정보를 활용해서 안드로이드 앱을 개발할 수 있습니다.

안드로이드 앱에서 KB부동산의 실거래가 정보를 활용하면 사용자들은 원하는 지역과 아파트의 정보를 입력하고 해당 아파트의 실거래가 정보를 확인할 수 있습니다. 이를 통해 부동산 거래 시 참고할 가격 정보를 미리 파악할 수 있으며, 실제 거래 시 현재 시장 가격에 대한 정보를 보다 정확하게 파악할 수 있습니다.

KB부동산 실거래가 제공 서비스를 이용하기 위해서는 API 인증키가 필요하며, 이를 통해 API를 호출하고 원하는 정보를 받아올 수 있습니다. 안드로이드 앱에서는 HTTP 통신을 이용해 KB부동산의 실거래가 정보를 받아와서 원하는 형태로 가공하여 사용자에게 보여줄 수 있습니다.

또한 Naver Map API와 함께 사용하면, 사용자가 원하는 지역의 아파트 위치를 지도상에서 확인할 수 있습니다. 이를 통해 사용자들은 아파트 위치와 가격 정보를 함께 확인하여 자세한 정보를 파악할 수 있습니다.

안드로이드 앱에서 KB부동산의 실거래가 제공 서비스를 활용하면, 부동산 시장에 대한 정확한 정보를 제공할 수 있으며, 사용자들은 이를 통해 부동산 거래에 대한 불확실성을 줄일 수 있습니다.

## KB부동산 아파트 실거래가 제공 서비스 소개
KB부동산은 부동산과 관련된 다양한 정보를 제공하는 웹사이트 및 모바일 앱 서비스를 제공하고 있다. 그중에서도 아파트 실거래가 정보는 매년 12월부터 해당년도 1월까지의 아파트 실거래가를 제공하며, 서울 및 광역시를 비롯한 전국의 아파트 매매 거래 정보를 수집 및 제공하고 있다. 이 서비스를 활용하면 각 지역별, 아파트 종류별로 최근 거래가, 평균 거래가, 거래 추세 등 다양한 정보를 확인할 수 있다. 또한, 실거래가 정보를 바탕으로 한 예측 가격 정보도 제공되므로 부동산 거래와 관련된 정보를 참고하는데 매우 유용하게 활용할 수 있다. 이러한 KB부동산의 아파트 실거래가 정보를 API로 제공하고 있으므로, 이를 활용하여 안드로이드 앱을 개발하여 다양한 부동산 거래 정보를 제공할 수 있다.

## API 인증키 발급받기
KB부동산 아파트 실거래가 제공 서비스 API를 사용하기 위해서는 인증키를 발급 받아야 합니다. 인증키 발급 방법은 다음과 같습니다.

1. KB 부동산 홈페이지에서 회원가입 또는 로그인을 합니다.
1. 메인 페이지에서 [API 서비스]를 클릭합니다.
1. API 서비스 페이지에서 [API 신청]을 클릭합니다.
1. 신청 양식을 작성합니다. [서비스명] 항목에는 사용 목적을 간단히 적습니다.
1. [동의] 버튼을 클릭합니다.
1. [확인] 버튼을 클릭하면 인증키를 발급 받을 수 있습니다.

인증키를 발급받으면, 이를 이용하여 API를 사용할 수 있습니다. 이를 위해 다음과 같은 예제 코드를 참고할 수 있습니다.

``` java
private static final String API_URL = "https://apis.kbland.co.kr/land/comm/api/aptTrade";
private static final String AUTH_KEY = "발급받은_인증키_입력";

public static void main(String[] args) {
    String aptName = "테스트 아파트";
    String lawdCd = "11110";
    String dealYm = "202204";
    String pageNo = "1";
    String numOfRows = "10";

    try {
        URL url = new URL(API_URL + "?aptName=" + aptName + "&lawdCd=" + lawdCd + "&dealYm=" + dealYm + "&pageNo=" + pageNo + "&numOfRows=" + numOfRows);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("GET");
        conn.setRequestProperty("Authorization", AUTH_KEY);

        BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));

        String line;
        StringBuffer sb = new StringBuffer();

        while ((line = br.readLine()) != null) {
            sb.append(line);
        }

        System.out.println(sb.toString());
        br.close();
        conn.disconnect();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

위 예제 코드는 KB부동산 아파트 실거래가 제공 서비스 API를 호출하는 예제입니다. 필요한 인자들을 설정하고, API URL에 인자들을 전달하여 URL 객체를 생성합니다. 그리고 HTTP 연결 객체를 생성한 뒤, 인증키를 설정하여 요청을 보내고 응답을 받습니다. 이후, 응답 결과를 출력하고, 자원을 반환합니다. 이와 같이 인증키를 이용하여 KB부동산 아파트 실거래가 제공 서비스 API를 사용할 수 있습니다.

## API 요청하기
KB부동산 아파트 실거래가 제공 서비스 API를 요청하는 방법은 다음과 같습니다.

1. API 인터페이스 선택하기: 먼저 KB부동산 아파트 실거래가 제공 서비스에서 제공하는 API 인터페이스 중 사용하고자 하는 인터페이스를 선택합니다.
    >KB부동산 아파트 실거래가 제공 서비스에서 제공하는 API 인터페이스는 HTTP 프로토콜을 이용하여 JSON 형태의 응답을 제공합니다. API 인터페이스를 이용하면 서비스에서 제공하는 아파트 매매/전월세/월세 실거래가 정보를 다른 웹/앱 서비스에서 쉽게 활용할 수 있습니다.
    >API 인터페이스는 기본적으로 HTTP GET 방식으로 요청하며, 요청 URL에 필요한 파라미터를 함께 전송하여 요청합니다. API 인증키는 요청 URL 끝에 &apiKey=인증키 형태로 함께 전송합니다.
    >API 인터페이스에서는 아파트 매매/전월세/월세 실거래가 정보를 비롯하여 아파트매매/전월세/월세 실거래가 검색조건, 지역별 실거래가 정보 등 다양한 정보를 제공합니다. 이 정보들을 활용하여 아파트 실거래가 정보를 제공하는 다양한 웹/앱 서비스를 구현할 수 있습니다.
    >API 인터페이스는 대부분 REST API 형태로 제공되어서, 안드로이드 앱 개발을 비롯한 다양한 플랫폼에서 활용할 수 있습니다. 요청 결과는 JSON 형태로 반환되기 때문에 파싱을 통해 쉽게 데이터를 추출할 수 있습니다. 이를 이용하면 서비스의 확장성과 편의성을 높일 수 있습니다.
    >KB부동산 아파트 실거래가 제공 서비스에서 제공하는 API 인터페이스 공식 문서는 아래 링크에서 확인할 수 있습니다.
    >https://www.kbfg.com/kbresearch/APIServiceGuide/ApartmentService

1. 요청 URL 생성하기: 선택한 API 인터페이스에 대한 요청 URL을 생성합니다. 요청 URL은 KB부동산 아파트 실거래가 제공 서비스에서 제공하는 API 문서를 참고하여 작성합니다.
1. HTTP 요청 보내기: 생성한 요청 URL을 기반으로 HTTP 요청을 보냅니다. 안드로이드 앱에서는 보통 HttpUrlConnection, OkHttp, Retrofit 등의 라이브러리를 사용하여 HTTP 요청을 보냅니다.
1. API 응답 받기: 서버에서는 요청을 받아 처리한 후, JSON, XML 등의 형태로 응답을 반환합니다. 안드로이드 앱에서는 응답을 받은 후에는 응답 결과를 처리하기 위한 작업을 수행합니다.

다음은 KB부동산 아파트 실거래가 제공 서비스 API를 요청하는 자바 코드 예제입니다.

``` java
String apiUrl = "요청 URL";
String apikey = "API 인증키";

URL url = new URL(apiUrl);
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
conn.setRequestMethod("GET");
conn.setRequestProperty("Content-type", "application/json");
conn.setRequestProperty("Authorization", apikey);

int responseCode = conn.getResponseCode();
BufferedReader br;
if (responseCode == 200) { // HTTP 요청이 성공한 경우
    br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
} else { // HTTP 요청이 실패한 경우
    br = new BufferedReader(new InputStreamReader(conn.getErrorStream()));
}

String inputLine;
StringBuilder response = new StringBuilder();
while ((inputLine = br.readLine()) != null) {
    response.append(inputLine);
}
br.close();

String result = response.toString();
// 응답 결과를 처리하는 코드 작성
```

위의 예제 코드에서 apiUrl 변수에는 요청할 API 인터페이스의 URL을, apikey 변수에는 API 인증키를 할당합니다. HttpUrlConnection을 사용하여 HTTP 요청을 보낸 후, 응답 결과를 받아서 처리하는 코드를 작성합니다.


## API 응답 받기
KB부동산 아파트 실거래가 제공 서비스 API 요청 후 서버로부터 응답을 받는 방법은 HTTP 프로토콜을 이용해 데이터를 주고 받는 방식으로 이루어진다. API 요청 시 사용한 URL 주소와 요청 방식에 따라 응답 내용의 형식과 구성이 달라질 수 있다. 따라서 응답 데이터를 처리하기 전에 응답 데이터의 형식과 구성을 파악하고 이를 처리하는 방법을 알아야 한다.

응답 데이터는 JSON, XML 등의 형식으로 제공되며, 일반적으로 HTTP 응답 코드와 함께 반환된다. HTTP 응답 코드는 서버가 처리한 결과를 클라이언트에게 전달하기 위한 코드이다. 예를 들어, 200 OK는 요청이 성공적으로 처리되었다는 것을 나타내고, 404 Not Found는 요청한 데이터를 찾을 수 없다는 것을 나타낸다.

응답 데이터를 처리하기 위해서는 먼저 HTTP 응답 코드를 확인하고, 성공적으로 처리되었는지 여부를 판단해야 한다. 그리고 응답 데이터의 형식에 따라 JSON 또는 XML 데이터를 파싱하여 필요한 데이터를 추출하고 활용할 수 있다.

다음은 KB부동산 아파트 실거래가 제공 서비스 API 요청 후 응답 데이터를 JSON 형식으로 받아 처리하는 예제 코드이다.

``` java
// HttpUrlConnection 객체를 이용해 API 요청 후 응답 데이터를 받아온다.
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
conn.setRequestMethod("GET");

BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
StringBuffer sb = new StringBuffer();
String line = "";

while ((line = br.readLine()) != null) {
    sb.append(line);
}

// JSON 데이터 파싱을 위해 JSONObject 객체를 생성한다.
JSONObject jsonObject = new JSONObject(sb.toString());

// JSON 데이터의 key 값을 이용해 필요한 데이터를 추출한다.
String aptName = jsonObject.getString("aptName");
int dealAmount = jsonObject.getInt("dealAmount");

// 추출한 데이터를 활용하여 앱에서 필요한 로직을 구현한다.

```

위의 예제 코드에서는 HttpURLConnection 객체를 이용해 API 요청 후 응답 데이터를 받아왔다. 이후에는 JSON 데이터를 파싱하고 필요한 데이터를 추출하여 활용할 수 있다. 이와 같은 방식으로 API 요청 후 응답 데이터를 처리하면, KB부동산 아파트 실거래가 제공 서비스에서 제공하는 다양한 데이터를 활용할 수 있다.

## 응답 데이터 가공하기

KB부동산 아파트 실거래가 제공 서비스에서 제공하는 API로부터 받은 데이터는 JSON 형식으로 제공됩니다. 이를 파싱하여 필요한 정보를 추출하고 가공하는 과정이 필요합니다.

가장 기본적인 가공 방법은 JSON 데이터를 객체로 변환하는 것입니다. 이를 위해서는 JSON 라이브러리를 사용할 수 있습니다. 안드로이드에서는 Gson 라이브러리를 많이 사용합니다. Gson은 JSON 데이터를 Java 객체로 변환하거나 Java 객체를 JSON 데이터로 변환하는 기능을 제공합니다.

예를 들어, KB부동산 아파트 실거래가 제공 서비스에서 받은 JSON 데이터 중에서 아파트 거래 내역 정보인 "거래금액", "건축년도", "전용면적" 등의 정보를 추출하고자 한다면, Gson 라이브러리의 JsonParser와 JsonObject를 사용하여 아래와 같은 코드로 처리할 수 있습니다.

``` java
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

...

// JSON 데이터를 문자열로 받아온 후 파싱한다.
JsonParser parser = new JsonParser();
JsonObject jsonObject = parser.parse(jsonData).getAsJsonObject();

// 필요한 정보를 추출하여 변수에 저장한다.
int price = jsonObject.get("거래금액").getAsInt();
int year = jsonObject.get("건축년도").getAsInt();
double area = jsonObject.get("전용면적").getAsDouble();
```
위 코드에서 jsonData는 KB부동산 아파트 실거래가 제공 서비스 API로부터 받은 JSON 데이터입니다. JsonParser를 이용하여 이 데이터를 JsonObject로 변환한 후, JsonObject에서 get 메소드를 사용하여 각 필드의 값을 추출합니다. 필드의 값은 해당하는 데이터 형식에 맞게 getAsInt, getAsDouble 등의 메소드를 사용하여 추출할 수 있습니다.

이렇게 추출한 데이터를 이용하여 앱 내에서 적절한 방식으로 활용할 수 있습니다. 예를 들어, 추출한 아파트 거래 내역 정보를 리스트 뷰에 나타내거나 지도에 표시하는 등의 방법으로 활용할 수 있습니다.

### GSON 라이브러리 사용 방법
gson 라이브러리를 사용하기 위해서는 build.gradle 파일에 아래와 같이 dependencies 블록 안에 구현해야 합니다.

``` javascript
dependencies {
    implementation 'com.google.code.gson:gson:2.8.7'
}
```

manifest 파일에는 gson과 관련된 선언은 필요하지 않습니다.

위와 같이 build.gradle 파일을 수정한 후, Android Studio에서 Build > Make Project를 실행하여 프로젝트를 다시 빌드해야합니다. 그러면 gson 라이브러리를 사용할 준비가 완료됩니다.

사용 예시는 아래와 같습니다.

``` java
import com.google.gson.Gson;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Gson gson = new Gson();

        // gson을 이용한 JSON 파싱 및 객체 생성 등의 작업 수행
        // ...
    }
}
```

이제 Gson 라이브러리를 사용하여 JSON을 파싱하거나 객체를 생성하는 등의 작업을 수행할 수 있습니다.

