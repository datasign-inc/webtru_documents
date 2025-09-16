## CMP 同意状態取得 API 仕様書 v0.1 (ベータ版)

**この仕様書は現在ベータ版です。今後内容が変更される可能性があります。**

### 使い方の例

下記のように`serviceID`を与えることで、その`serviceID`の同意状態を取得できます。
サービス ID に関しては[サービス ID 一覧表](https://docs.google.com/spreadsheets/d/1z_80EI7lN1xcmuCcfgz2EVR3oxGzvcUTEpSks7hsZfI/edit)をご確認ください。

```javascript
const api = new WebtruCmpApi((version = "1.0"));
const serviceId = 23;

api.get(serviceId);
```

### 同意状態の取得方法

#### WebtruCmpApi.getAll()

CMP が設置されているウェブサイトにおける全ての外部サービスの同意状態が取得できます。

##### 引数: なし

##### 戻り値: Object

```javascript
{ 22: {permitted: true, reason: 20}, 33: {permitted: false, reason: 40}, ...}
```

#### WebtruCmpApi.get(serviceId)

指定したサービス ID の同意状態が取得できます。

#### WebtruCmpApi.get(serviceId)

##### 引数: サービス ID Integer

[サービス ID 一覧表](https://docs.google.com/spreadsheets/d/1z_80EI7lN1xcmuCcfgz2EVR3oxGzvcUTEpSks7hsZfI/edit)をご確認ください。

##### 戻り値: Object

```javascript
{permitted: true, reason: 20}
```

#### reason (ユーザ操作の種別)

```javascript
WebtruCmpApi.DEFAULT_OF_SITE = 20; // ユーザーがチェックボックスを触らなかった
WebtruCmpApi.CHOSEN_BY_USERS = 40; // 明示的同意。ユーザーが選択した（チェックボックスをクリックして on/off を変えた）
WebtruCmpApi.FORCED_BY_SITE = 60; // 必須サービス。ユーザーに利用許可/禁止の選択許可がないサービス
```

##### reason の使用例

```javascript
const api = new WebtruCmpApi(version=”1.0”)
if (api.get(23).reason == api.FORCED_BY_SITE) {
  // do something...
}
```

#### イベントリスナー

下記のようにイベントリスナーを利用することで、ウィジェットの操作状況または同意状態が変更されたときの同意状態を取得できます。

##### `WebtruCmpApi.on(eventName, fn)`

ユーザがウィジェットを操作した際に呼び出されるイベントリスナーを登録します。

###### 引数: 
  - `eventName` `{String}`: 下記eventName一覧に記載の値のみ許容
  - `fn` `{Function}`: 
    - `e` `{CustomEvent}`: 下記eventNameの場合、第一引数としてCustomEventが渡されます
      - `WebtruCmpApi.EVENT_CMP_SAVE` の場合: CustomEventの `detail.detail` プロパティにて最新の全ての外部サービスの同意状態(`WebtruCmpApi.getAll()` で得られるもの)が渡されます。
      - `WebtruCmpApi.EVENT_LIST_OPENED` の場合: CustomEventの `detail.ui` プロパティにてどのボタンでウィジェットの一覧画面を開いたのかが渡されます。uiで渡される値は以下のいずれかとなります。
        - `dialog`: ダイアログ内のボタン
        - `textButton`: 公表モード（ボタン方式） 及び オプトアウトモード（ボタン方式） のボタン
        - `button`: ダイアログやボタン方式のボタンを閉じた際に表示される歯車及びインフォメーションマークのボタン
        - `link`: 公表モード（リンク方式）にて設置いただいたリンク
      - `WebtruCmpApi.EVENT_OPT_OUT_LINK_CLICKED` の場合: CustomEventの `detail.id` プロパティにてオプトアウトリンクがクリックされたサービスのIDが渡されます。サービスのIDは [サービス ID 一覧表](https://docs.google.com/spreadsheets/d/1z_80EI7lN1xcmuCcfgz2EVR3oxGzvcUTEpSks7hsZfI/edit)をご確認ください。

###### 戻り値: なし

###### eventName一覧
  - `WebtruCmpApi.EVENT_CMP_SAVE`: ウィジェットの一覧画面で `選択した内容を反映する` ボタンを押す
  - `WebtruCmpApi.EVENT_LIST_OPENED`: ウィジェットの一覧画面を開く
  - `WebtruCmpApi.EVENT_LIST_CLOSE`: ウィジェットの一覧画面を閉じる（×ボタンまたは一覧画面外をクリック）
  - `WebtruCmpApi.EVENT_DIALOG_OPENED`: ウィジェットのダイアログが表示される
  - `WebtruCmpApi.EVENT_DIALOG_ACCEPTED`: ウィジェットのダイアログで `同意する` ボタンを押す
  - `WebtruCmpApi.EVENT_DIALOG_DENIED`: ウィジェットのダイアログで `拒否する` ボタンを押す
  - `WebtruCmpApi.EVENT_DIALOG_CLOSED`: ウィジェットのダイアログを閉じる（閉じるボタンまたは×ボタンをクリック）
  - `WebtruCmpApi.EVENT_OPT_OUT_LINK_CLICKED`: ウィジェットのオプトアウトリンクを押す

##### 使用例
```javascript
const api = new WebtruCmpApi(version=”1.0”)
api.on(WebtruCmpApi.EVENT_CMP_SAVE, function (e) {
  // do something... eg: console.log(e.detail)
})
api.on(WebtruCmpApi.EVENT_DIALOG_ACCEPTED, function () {
  // do something...
})
```

### ウィジェット表示時のイベント

ウィジェットの表示が完了すると `cmpWidget:rendered` イベントが発生します。

このイベントは同意管理プロには未実装です。

#### 使用例

```javascript
document.addEventListener("cmpWidget:rendered", () => {
  // do something...
});
```

### CSP挿入時のイベント

ウィジェットによるCSPの挿入が完了すると `cmpWidget:cspInserted` （同意管理プロでは `cmp:cspInserted`） イベントが発生します。

#### 使用例

```javascript
document.addEventListener("cmpWidget:cspInserted", () => {
  // do something... eg: insert script tags
});
```
