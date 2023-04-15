---
layout : default
title : chapter3
parent : How to make android app with KB api and naver map api
grand_parent : Write a book with ChatGPT 
---

# Chapter 3.  KB부동산 아파트 실거래가 제공 서비스 활용하기
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

## API 응답 받기

## 응답 데이터 가공하기

