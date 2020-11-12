# 概要
googleが提供しているgeolocation apiを用いて端末から現在地を取得する。

js
```ruby:js
function initMap() {
  // Geolocation APIに対応している場合
  if (navigator.geolocation) {
    // 現在地を取得
    navigator.geolocation.getCurrentPosition(
      // 取得成功した場合
      function(position) {
        // 緯度・経度を変数に格納
        let mapLatLng = new google.maps.LatLng(position.coords.latitude, position.coords.longitude);
        // マップオブジェクト作成
        map = new google.maps.Map(document.getElementById("map"), {
          center: mapLatLng,
          zoom: 17,
        })
        // マーカーの作成
        new google.maps.Marker ({
          map: map,
          position: mapLatLng
        })
      },
      // 取得失敗した場合
      function(error) {
        // エラーメッセージを表示
        switch(error.code) {
          case 1: // PERMISSION_DENIED
            alert("位置情報の利用が許可されていません");
            break;
          case 2: // POSITION_UNAVAILABLE
            alert("現在位置が取得できませんでした");
            break;
          case 3: // TIMEOUT
            alert("タイムアウトになりました");
            break;
          default:
            alert("その他のエラー(エラーコード:"+error.code+")");
            break;
        }
      }
    )  
  } else {
    alert("この端末では位置情報が取得できません");
  }
}

```

haml
```ruby:haml
%script{async: "", defer: "defer", src: "https://maps.googleapis.com/maps/api/js?key=#{ENV['API_KEY']}&callback=initMap&libraries=places&v=weekly"}
.map#map
```

css で　heightを指定する（数値は任意）

# 参考記事
https://qiita.com/tiara/items/4a1c98418917a0e74cbb

https://qiita.com/akkey2475/items/81f4f94f17bfe5c7ce42

https://developers.google.com/maps/documentation/geolocation/overview
