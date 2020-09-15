## CMP同意状態取得API仕様書

### Getting Start
```javascript
const api = WebtruCmpApi(version="1.0")  
const serviceId = 23

api.get(serviceId)
```

### 同意状態の取得
#### WebtruCmpApi.getAll()
##### 戻り値: Object
```javascript
{ 22: {permitted: true, reason: 20}, 33: {permitted: false, reason: 40}}
```

#### WebtruCmpApi.get(serviceId)
##### 引数: サービスID
　　→ サービスIDは別ファイルで公開(Github)または個別提供。  

##### 戻り値
```javascript
{permitted: true, reason: 20}
```

#### reason
```javascript
api.DEFAULT_OF_SITE = 20 // ユーザーがチェックボックスを触らなかった
api.CHOSEN_BY_USERS = 40 // 明示的同意。ユーザーが選択した（チェックボックスをクリックして on/off を変えた）
api.FORCED_BY_SITE = 60 // ユーザーに利用許可/禁止の選択許可がないサービス
```

### 使用例
```javascript
const api = window.WebtruCmpApi(version=”1.0”)
if (api.get(23).reason == api.FORCED_BY_SITE) {
  // do something...
}
```
