## CMP同意状態取得API仕様書 v0.1 (ベータ版)

**この仕様書は現在ベータ版です。今後内容が変更される可能性があります。**

### 使い方の例

下記のように`serviceID`を与えることで、その`serviceID`の同意状態を取得できます。
サービスIDに関しては[サービスID一覧表](https://docs.google.com/spreadsheets/d/1z_80EI7lN1xcmuCcfgz2EVR3oxGzvcUTEpSks7hsZfI/edit)をご確認ください。

```javascript
const api = WebtruCmpApi(version="1.0")  
const serviceId = 23

api.get(serviceId)
```

### 同意状態の取得方法

#### WebtruCmpApi.getAll()

CMPが設置されているウェブサイトにおける全ての外部サービスの同意状態が取得できます。

##### 引数: なし

##### 戻り値: Object
```javascript
{ 22: {permitted: true, reason: 20}, 33: {permitted: false, reason: 40}, ...}
```

#### WebtruCmpApi.get(serviceId)

指定したサービスIDの同意状態が取得できます。

#### WebtruCmpApi.get(serviceId)

##### 引数: サービスID Integer

[サービスID一覧表](https://docs.google.com/spreadsheets/d/1z_80EI7lN1xcmuCcfgz2EVR3oxGzvcUTEpSks7hsZfI/edit)をご確認ください。

##### 戻り値: Object
```javascript
{permitted: true, reason: 20}
```

#### reason (ユーザ操作の種別)
```javascript
api.DEFAULT_OF_SITE = 20 // ユーザーがチェックボックスを触らなかった
api.CHOSEN_BY_USERS = 40 // 明示的同意。ユーザーが選択した（チェックボックスをクリックして on/off を変えた）
api.FORCED_BY_SITE  = 60 // 必須サービス。ユーザーに利用許可/禁止の選択許可がないサービス
```

##### reasonの使用例

```javascript
const api = WebtruCmpApi(version=”1.0”)
if (api.get(23).reason == api.FORCED_BY_SITE) {
  // do something...
}
```

#### イベントリスナー

下記のようにイベントリスナーを利用することで、CMPの同意状態が変更されたときに同意状態を取得できます。

#### 使用例

```javascript
TBD
```

