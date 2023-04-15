---
layout : default
title : chapter5
parent : How to make android app with KB api and naver map api
grand_parent : Write a book with ChatGPT 
---

# Chpater5. 안드로이드 앱 기능 추가하기
안드로이드 앱에 기능을 추가하려고 할 때 고려해야 하는 것은 다양합니다. 

- 첫째, 사용자가 필요로 하는 기능과 편의성입니다. 사용자 요구사항을 파악하고 그에 맞게 기능을 구현해야 합니다. 
- 둘째, 성능과 안정성입니다. 새로운 기능을 추가할 때 앱 전체의 성능과 안정성이 영향을 받을 수 있습니다. 이러한 문제를 방지하기 위해 코드 최적화 및 안정성 보장을 위한 테스트를 진행해야 합니다. 
- 셋째, 보안성입니다. 새로운 기능 추가로 인해 앱 내부에 새로운 취약점이 생길 가능성이 있습니다. 이러한 취약점을 사전에 파악하고 대응해야 합니다. 
- 마지막으로, 호환성입니다. 새로운 기능을 추가할 때 이전 버전의 안드로이드 OS에서도 동작하도록 호환성을 고려해야 합니다. 

이러한 요소들을 고려하여 새로운 기능을 추가하면 안드로이드 앱이 보다 더 효율적이고 안정적인 앱이 될 수 있습니다.

## 검색 기능 추가하기
KB부동산 아파트 실거래가 검색 기능을 추가하는 안드로이드 앱을 만들기 위해서는 Retrofit과 함께 검색 API를 호출하고, 이를 통해 받은 데이터를 가공하여 화면에 표시해주어야 합니다.

1. 레이아웃 구성
검색 기능을 위한 레이아웃을 구성합니다. 검색어 입력을 받을 EditText와 검색 버튼 등을 포함한 레이아웃을 구성합니다.

2. Retrofit으로 API 호출
검색 버튼을 클릭하면 Retrofit을 이용하여 KB부동산 아파트 실거래가 검색 API를 호출합니다. 이 때, 검색어를 입력한 EditText에서 값을 가져와 API 호출에 이용합니다. 예제 코드는 아래와 같습니다.

``` java
// Retrofit 객체 생성
Retrofit retrofit = new Retrofit.Builder()
        .baseUrl("https://openapi.kbstar.com/")
        .addConverterFactory(GsonConverterFactory.create())
        .build();

// API 호출 인터페이스 생성
KbApiService kbApiService = retrofit.create(KbApiService.class);

// 검색어 입력값 가져오기
String keyword = editText.getText().toString();

// API 호출
Call<KbApiResponse> call = kbApiService.searchApart(keyword);
call.enqueue(new Callback<KbApiResponse>() {
    @Override
    public void onResponse(Call<KbApiResponse> call, Response<KbApiResponse> response) {
        // API 호출 성공 시 처리할 코드
    }

    @Override
    public void onFailure(Call<KbApiResponse> call, Throwable t) {
        // API 호출 실패 시 처리할 코드
    }
});
```

3. API 응답 데이터 가공
API 호출에 성공하면 onResponse() 메서드가 호출되고, 이때 API 응답 데이터가 전달됩니다. 전달받은 데이터를 가공하여 화면에 표시해줍니다. 예제 코드는 아래와 같습니다.

``` java
@Override
public void onResponse(Call<KbApiResponse> call, Response<KbApiResponse> response) {
    if (response.isSuccessful()) {
        KbApiResponse kbApiResponse = response.body();
        List<KbApiResult> kbApiResults = kbApiResponse.getData();

        // 검색 결과를 RecyclerView로 표시하기
        recyclerView.setLayoutManager(new LinearLayoutManager(context));
        recyclerView.setAdapter(new MyAdapter(kbApiResults));
    } else {
        Log.e(TAG, "API 호출 실패");
    }
}
```

4. 추가 기능 구현
검색 결과를 표시한 후, 추가적인 기능을 구현할 수 있습니다. 예를 들어, RecyclerView의 아이템을 클릭하면 해당 아파트의 위치를 Naver Map API를 이용하여 지도에 표시하는 등의 기능을 추가할 수 있습니다.

## 검색 결과 목록 표시하기
KB부동산 아파트 실거래가 검색하는 안드로이드 앱에서 추가한 검색 기능을 통해 검색 결과를 목록으로 표시하는 방법은 다음과 같습니다.

1. 검색 결과를 보여줄 RecyclerView 위젯을 레이아웃에 추가합니다.
``` xml
<androidx.recyclerview.widget.RecyclerView
    android:id="@+id/result_list"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

2. 검색 결과를 담을 데이터 클래스를 만듭니다.
``` java
public class AptTradeItem {
    public String aptName;
    public String dealAmount;
    public String buildYear;
    // ...
}
```

3. 검색 결과를 처리할 Adapter 클래스를 만듭니다.
``` java
public class AptTradeListAdapter extends RecyclerView.Adapter<AptTradeListAdapter.ViewHolder> {
    private List<AptTradeItem> mItems;

    public AptTradeListAdapter(List<AptTradeItem> items) {
        mItems = items;
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        Context context = parent.getContext();
        LayoutInflater inflater = LayoutInflater.from(context);
        View view = inflater.inflate(R.layout.item_apt_trade, parent, false);
        ViewHolder viewHolder = new ViewHolder(view);
        return viewHolder;
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        AptTradeItem item = mItems.get(position);
        holder.aptNameTextView.setText(item.aptName);
        holder.dealAmountTextView.setText(item.dealAmount);
        holder.buildYearTextView.setText(item.buildYear);
        // ...
    }

    @Override
    public int getItemCount() {
        return mItems.size();
    }

    public static class ViewHolder extends RecyclerView.ViewHolder {
        public TextView aptNameTextView;
        public TextView dealAmountTextView;
        public TextView buildYearTextView;
        // ...

        public ViewHolder(View itemView) {
            super(itemView);
            aptNameTextView = itemView.findViewById(R.id.apt_name);
            dealAmountTextView = itemView.findViewById(R.id.deal_amount);
            buildYearTextView = itemView.findViewById(R.id.build_year);
            // ...
        }
    }
}
```

4. 검색 결과를 받아와 Adapter에 데이터를 넘겨주는 작업을 수행합니다.
``` java
// Retrofit을 이용하여 API 호출 후 응답을 받아옴
Call<List<AptTradeItem>> call = apiService.searchAptTrade(...);
call.enqueue(new Callback<List<AptTradeItem>>() {
    @Override
    public void onResponse(Call<List<AptTradeItem>> call, Response<List<AptTradeItem>> response) {
        if (response.isSuccessful()) {
            List<AptTradeItem> items = response.body();
            AptTradeListAdapter adapter = new AptTradeListAdapter(items);
            mRecyclerView.setAdapter(adapter);
        } else {
            // API 호출 실패 시 처리
        }
    }

    @Override
    public void onFailure(Call<List<AptTradeItem>> call, Throwable t) {
        // API 호출 실패 시 처리
    }
});
```

위와 같이 RecyclerView와 Adapter를 이용하여 검색 결과를 목록으로 표시할 수 있습니다. 데이터를 받아오는 API 호출과정은 Retrofit을 이용하여 수행하며, 검색 기능은 KB부동산 API의 쿼리 파라미터를 설정하여 호출합니다.

## 검색 결과 지도에 표시하기
KB부동산 아파트 실거래가 검색하는 안드로이드 앱에서 검색 결과를 지도에 표시하는 방법은 Naver Map API를 활용하는 것이 일반적입니다.

먼저, 검색 결과를 받아오는 Retrofit 인터페이스에 적절한 처리 로직을 추가해 검색된 아파트 정보를 가지고 있는 리스트를 얻어옵니다. 그리고 이 리스트를 반복문으로 돌면서 각각의 아파트 정보에서 위도와 경도 값을 가져와 지도에 마커를 표시하면 됩니다.

아래는 검색 결과를 지도에 표시하는 예제 코드입니다.

먼저, NaverMap 객체를 생성하고 onMapReady() 콜백 메서드에서 생성된 NaverMap 객체를 받아옵니다.

``` java
public class MapActivity extends AppCompatActivity implements OnMapReadyCallback {
    
    private NaverMap naverMap;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_map);
        
        MapView mapView = findViewById(R.id.map_view);
        mapView.getMapAsync(this);
    }

    @Override
    public void onMapReady(@NonNull NaverMap naverMap) {
        this.naverMap = naverMap;
    }
}
```

다음으로, 검색 결과를 받아올 Retrofit 인터페이스를 정의합니다.

``` java
public interface ApartmentApiService {
    @GET("apartment")
    Call<List<Apartment>> getApartments(
        @Query("address") String address,
        @Query("type") String type,
        @Query("min_price") int minPrice,
        @Query("max_price") int maxPrice,
        @Query("min_area") int minArea,
        @Query("max_area") int maxArea,
        @Query("min_year") int minYear,
        @Query("max_year") int maxYear,
        @Query("page") int page,
        @Query("number") int number,
        @Header("Authorization") String token
    );
}
```

다음으로, getApartments() 메서드를 호출하면 검색 결과를 받아옵니다.

``` java
private void searchApartments(String keyword) {
    ApartmentApiService service = retrofit.create(ApartmentApiService.class);
    Call<List<Apartment>> call = service.getApartments(
        keyword, "A1", 0, 99999999, 0, 9999, 1900, 2100, 1, 10, "인증키"
    );
    call.enqueue(new Callback<List<Apartment>>() {
        @Override
        public void onResponse(Call<List<Apartment>> call, Response<List<Apartment>> response) {
            if (response.isSuccessful()) {
                List<Apartment> apartments = response.body();
                for (Apartment apartment : apartments) {
                    Marker marker = new Marker();
                    marker.setPosition(new LatLng(apartment.getLat(), apartment.getLng()));
                    marker.setMap(naverMap);
                }
            }
        }

        @Override
        public void onFailure(Call<List<Apartment>> call, Throwable t) {
            Toast.makeText(getApplicationContext(), "Error: " + t.getMessage(), Toast.LENGTH_SHORT).show();
        }
    });
}
```

getApartments() 메서드의 호출 결과에 대한 응답 코드는 HTTP 상태 코드로 나타나며, 이 코드는 API 요청이 성공했는지 실패했는지를 나타낸다. 일반적으로 성공적인 요청에 대한 HTTP 상태 코드는 200 OK이며, 실패한 요청에 대한 상태 코드는 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found 등이다.

예제 코드에서는 getApartments() 메서드에서 Retrofit으로 KB부동산 API에 요청을 보내고, 응답을 처리하는 코드가 작성되어 있다. 이 코드에서는 response.isSuccessful() 메서드를 통해 HTTP 상태 코드가 200 OK인지 확인하고, 성공적인 요청일 경우 response.body()를 통해 API 응답 데이터를 가져온다. 이후 가져온 데이터를 처리하는 코드가 작성될 수 있다. 만약 응답이 실패한 경우, response.code() 메서드를 통해 실패한 상태 코드를 가져올 수 있으며, 실패에 대한 예외 처리 코드가 작성될 수 있다.

``` java
private void getApartments(String query) {
    // Retrofit 객체 생성
    Retrofit retrofit = new Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create())
            .build();

    // API 서비스 인터페이스 생성
    ApartmentService apartmentService = retrofit.create(ApartmentService.class);

    // API 요청
    Call<List<Apartment>> call = apartmentService.getApartments(API_KEY, query);

    // API 응답 처리
    call.enqueue(new Callback<List<Apartment>>() {
        @Override
        public void onResponse(Call<List<Apartment>> call, Response<List<Apartment>> response) {
            if (response.isSuccessful()) {
                List<Apartment> apartments = response.body();
                // 지도에 마커 추가하는 로직
                addMarkersToMap(apartments);
            } else {
                // API 요청이 실패한 경우
                Log.d(TAG, "API Request Failed: " + response.errorBody());
            }
        }

        @Override
        public void onFailure(Call<List<Apartment>> call, Throwable t) {
            // API 요청 자체가 실패한 경우
            Log.d(TAG, "API Request Failed: " + t.getMessage());
        }
    });
}
```

위 코드에서 getApartments() 메서드는 API를 호출하고, 호출 결과에 따라 지도에 마커를 추가하는 기능을 담당합니다.

Retrofit 객체를 생성하고, ApartmentService 인터페이스를 통해 API를 호출합니다. API 요청은 Call 객체를 통해 생성하고, call.enqueue() 메서드를 통해 비동기적으로 실행됩니다.

API 응답 처리는 Callback 인터페이스를 통해 수행됩니다. onResponse() 메서드는 API 요청이 성공적으로 수행된 경우 호출되며, response.isSuccessful() 메서드를 통해 응답이 성공적인지 확인할 수 있습니다. 성공적인 경우 응답 데이터를 response.body()를 통해 얻어올 수 있으며, 이를 기반으로 지도에 마커를 추가하는 기능을 수행합니다.

만약 API 요청이 실패한 경우에는 onResponse() 메서드 대신 onFailure() 메서드가 호출되며, 이를 통해 실패한 이유를 확인할 수 있습니다. 이 경우 로그를 찍어서 확인합니다.

## 사용자 위치 기반 검색 기능 추가하기

KB부동산 아파트 실거래가 검색하는 안드로이드 앱에 사용자 위치 기반 검색 기능을 추가하려면, 안드로이드에서 제공하는 LocationManager를 이용해서 현재 위치를 얻어오는 작업이 필요합니다.

AndroidManifest.xml에 위치 권한 추가
``` xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

LocationManager를 이용하여 현재 위치 얻어오기
``` java
private void getCurrentLocation() {
    LocationManager locationManager = (LocationManager) getSystemService(Context.LOCATION_SERVICE);
    if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) == PackageManager.PERMISSION_GRANTED) {
        Location location = locationManager.getLastKnownLocation(LocationManager.GPS_PROVIDER);
        if (location != null) {
            double lat = location.getLatitude();
            double lon = location.getLongitude();
            // 위치 정보를 이용해서 API 호출
            getApartments(lat, lon);
        } else {
            // 위치 정보를 얻어올 수 없는 경우
            Toast.makeText(this, "현재 위치를 얻어올 수 없습니다.", Toast.LENGTH_SHORT).show();
        }
    } else {
        // 위치 권한이 없는 경우
        ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.ACCESS_FINE_LOCATION}, REQUEST_LOCATION_PERMISSION);
    }
}
```

API 호출 시 현재 위치 정보를 파라미터로 전달
``` java
private void getApartments(double lat, double lon) {
    Call<ApartmentListResponse> call = apiService.getApartments(API_KEY, "json", lat, lon, ...);
    ...
}
```

위와 같은 방식으로 사용자의 현재 위치 정보를 얻어와서 KB부동산 API에 전달하면 됩니다. 이때, 위치 권한을 사용자로부터 받아오기 위해 ActivityCompat.requestPermissions() 메서드를 이용해서 권한 요청 대화상자를 띄우는 작업이 필요합니다. 또한, 위치 정보를 얻어올 수 없는 경우에 대한 예외 처리도 필요합니다.

사용자의 현재 위치를 가져오기 위해서는 위치 권한이 필요합니다. 따라서 위치 권한을 사용자로부터 받아오는 코드를 작성해야 합니다.

먼저, Manifest 파일에 위치 권한을 추가해줍니다.
``` xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

그 다음, 위치 정보를 받아오기 위한 LocationManager 객체와 위치 권한 체크를 위한 checkSelfPermission 메서드를 사용합니다.

``` java
// 위치 정보를 받아올 때 사용할 LocationManager 객체 생성
LocationManager locationManager = (LocationManager) getSystemService(Context.LOCATION_SERVICE);

// 위치 권한 체크
if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
    // 권한이 없을 경우 권한 요청 다이얼로그를 띄웁니다.
    ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.ACCESS_FINE_LOCATION}, PERMISSIONS_REQUEST_LOCATION);
} else {
    // 권한이 있을 경우, requestLocationUpdates 메서드를 통해 위치 정보를 받아옵니다.
    locationManager.requestLocationUpdates(LocationManager.GPS_PROVIDER, 0, 0, locationListener);
}
```

위 코드에서는 checkSelfPermission 메서드를 사용하여 위치 권한이 있는지 확인합니다. 만약 권한이 없다면 requestPermissions 메서드를 사용하여 권한 요청 다이얼로그를 띄워줍니다.

위 코드에서 requestLocationUpdates 메서드를 호출하여 위치 정보를 받아오는데, 이 때 위치 정보를 받아올 때마다 호출되는 LocationListener 객체를 등록해야 합니다.

``` java
// 위치 정보가 변경될 때마다 호출되는 LocationListener 객체 생성
LocationListener locationListener = new LocationListener() {
    @Override
    public void onLocationChanged(Location location) {
        // 위치 정보를 받아왔을 때 처리하는 코드 작성
    }

    @Override
    public void onStatusChanged(String provider, int status, Bundle extras) {

    }

    @Override
    public void onProviderEnabled(String provider) {

    }

    @Override
    public void onProviderDisabled(String provider) {

    }
};
```

안드로이드에서 위치 정보를 얻어오는 과정에서 사용자의 위치 정보를 가져올 수 없는 경우에는 SecurityException이 발생할 수 있습니다. 이 경우에는 예외 처리를 통해 사용자에게 알림을 제공하거나 다른 처리를 수행할 수 있습니다.

예를 들어, 위치 정보를 얻어오는 코드 블록에서 try-catch 문을 이용하여 SecurityException을 처리할 수 있습니다. 예외 처리에서는 사용자에게 알림을 제공할 수도 있고, 기본 위치 정보를 사용하거나 이전에 저장된 위치 정보를 사용할 수도 있습니다.

다음은 예외 처리 코드의 예시입니다.
``` java
private void getLocation() {
    try {
        locationManager = (LocationManager) getSystemService(Context.LOCATION_SERVICE);
        locationManager.requestLocationUpdates(LocationManager.NETWORK_PROVIDER, 0, 0, this);
    } catch (SecurityException e) {
        e.printStackTrace();
        // 위치 정보를 가져올 수 없는 경우에 대한 처리
    }
}
```

위 코드에서 requestLocationUpdates 메서드를 호출할 때 SecurityException이 발생할 수 있습니다. 이를 예외 처리해줍니다. 예외가 발생할 경우 printStackTrace() 메서드를 이용하여 로그를 출력하거나, Toast를 이용하여 사용자에게 메시지를 띄워줄 수 있습니다.

위치 정보를 가져올 수 없는 경우에는 사용자가 앱의 기능을 이용하지 못할 수 있으므로, 이에 대한 예외 처리는 중요합니다.

GPS 신호가 약해서 위치정보를 얻어올 수 없는 경우에는 LocationManager 클래스의 requestLocationUpdates() 메서드로 위치 정보를 업데이트할 때 LocationListener 객체를 등록하고, onLocationChanged() 메서드에서 위치 정보를 얻어와서 처리할 수 있습니다.

하지만 GPS 신호가 약한 경우, onLocationChanged() 메서드가 호출되지 않을 수 있습니다. 이 경우에 대비해서 LocationListener 인터페이스에는 onProviderDisabled()와 onProviderEnabled() 메서드가 있습니다. 이를 활용하여 GPS 신호가 약한 경우를 대비할 수 있습니다.

예를 들어, 아래와 같이 LocationListener 객체를 구현하고 onProviderDisabled() 메서드에서 GPS 신호가 약해진 경우를 처리할 수 있습니다.

``` java
private final LocationListener mLocationListener = new LocationListener() {
    @Override
    public void onLocationChanged(Location location) {
        // 위치 정보가 업데이트되면 호출되는 메서드
        double latitude = location.getLatitude();
        double longitude = location.getLongitude();
        // 위치 정보를 활용한 작업 수행
    }

    @Override
    public void onProviderDisabled(String provider) {
        // 위치 제공자가 비활성화될 때 호출되는 메서드
        // GPS 신호가 약해진 경우 등에 대비하여 처리 가능
        Toast.makeText(getApplicationContext(), "GPS 신호가 약합니다", Toast.LENGTH_LONG).show();
    }

    @Override
    public void onProviderEnabled(String provider) {
        // 위치 제공자가 활성화될 때 호출되는 메서드
    }

    @Override
    public void onStatusChanged(String provider, int status, Bundle extras) {
        // 위치 제공자의 상태가 변경될 때 호출되는 메서드
    }
};
```

위와 같이 onProviderDisabled() 메서드에서 GPS 신호가 약한 경우에 대한 처리를 하면, 앱 사용자들이 약한 GPS 신호로 인한 불편을 최소화할 수 있습니다.