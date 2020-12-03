# 概要
クリックした際にmarker上に情報窓を表示する

# 解説
```ruby
// 作成したマーカーの情報を格納
let markers = []
// infopanelを代入する変数
let activeInfoWindow;

function createInfo(name, url, j) {
  const contentString =
    '<div id="content">' +
    '<div id="siteNotice">' +
    "</div>" +
    '<h3 id="firstHeading" class="firstHeading">' + name + '</h3>' +
    '<div id="bodyContent">' +
    "<p><b>Uluru</b>, txit</p>" +
    "<a href=\"https://maps.google.co.jp/maps?q=" + url + "&z=15&iwloc=A\"target=\"_blank\">" + "詳細" +
    "</a><br />"
    "</div>" +
    "</div>";

  j = j + 1
  const infowindow = new google.maps.InfoWindow({
    content: contentString,
  });

  if (activeInfoWindow) { activeInfoWindow.close();}
  infowindow.open(map, markers[j]);
  activeInfoWindow = infowindow;
}
```

ここには記載していませんが、aタグをクリックするとcreateInfo関数が呼び出されるようになっています。
引数には、左から、「店名」、「googlemapのurl」、「配列markersの要素を呼び出すための数字」を指定しています。

```ruby
const contentString =
    '<div id="content">' +
    '<div id="siteNotice">' +
    "</div>" +
    '<h3 id="firstHeading" class="firstHeading">' + name + '</h3>' +
    '<div id="bodyContent">' +
    "<p><b>Uluru</b>, txit</p>" +
    "<a href=\"https://maps.google.co.jp/maps?q=" + url + "&z=15&iwloc=A\"target=\"_blank\">" + "詳細" +
    "</a><br />"
    "</div>" +
    "</div>";
```

infowindowで表示するためのhtmlを作成して、変数に代入しています。

```ruby
j = j + 1
const infowindow = new google.maps.InfoWindow({
  content: contentString,
});

if (activeInfoWindow) { activeInfoWindow.close();}
infowindow.open(map, markers[j]);
activeInfoWindow = infowindow;
```

配列markersには表示されているマーカーの情報が全て格納されています。
infowindowはマーカーの上に出現させるので、位置を指定する際にmarkers配列から任意の情報を取り出します。
変数jはmarkers配列の要素を指定するためのものです。（実装の都合上変数jを使用していますが、複雑になるため、理由は割愛します。）

const infowindow = new google.maps.InfoWindow()でinfowindowサービスにアクセスし、変数に格納しています。

