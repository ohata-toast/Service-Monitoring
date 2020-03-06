## Management > Service Monitoring > APIガイド

### 基本情報
```
API Endpoint: https://api-service-monitoring.cloud.toast.com
```

## 単一バッチモニタリング

### データ転送
- バッチモニタリングサーバーに検証が必要なデータを転送します。
- バッチモニタリングに入力した検証情報に応じたJSONタイプのデータを転送することができます。バッチモニタリングの検証に失敗すると障害に登録されます。

[URL]
```http
POST /v1.0/monitoring/batchmon/appkey/{appKey}/scenarios/{scenarioId}
Content-Type: application/json
```

[Path Variables]

| 値 |	タイプ | 必須かどうか |	説明 |
|---|---|---|--|
| appKey | String | Required | サービスアプリケーションキー(サービス管理タブで確認可能) |
| scenarioId | String | Required | サービスID |

[Request Body]
```json
{
    "issueDescription": "This is test message."
}
```


#### レスポンス
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "pk": {
            "serviceId": "d781dafc-2d5e-3d11-a87e-529b0868ea4f",
            "requestId": "3f136280-9d6a-11e9-83b4-ff39d67ddecf"
        },
        "scenarioId": "b00699c0-96f4-11e9-a68b-a7aaea9ae346",
        "ipaddr": "127.0.0.1",
        "requestTime": "2099-12-31T00:00:00.000",
        "requestData": {
            "body": "{\"issueDescription\": \"This is test message.\"}"
        },
        "serviceCode": 0
    }
}
```

| 値 | タイプ | 説明 |
|---|---|---|
| header.isSuccessful | Boolean | 成否 |
| header.resultCode | Integer | 失敗コード(0は正常) |
| header.resultMessage | String | 失敗メッセージ |
| body.pk.serviceId | String | サービス固有ID |
| body.pk.requestId | String | リクエスト固有ID |
| body.scenarioId | String | シナリオ固有ID |
| body.requestData.body | Object | リクエストデータ |
| body.ipaddr | String | リクエスト者のIPアドレス |
| body.requestTime | String | リクエスト時刻(ISO 8601フォーマット) |
| body.serviceCode | Integer | サービス固有コード |


## 多重バッチモニタリング
- 1回のリクエストで複数のサービス、シナリオを検証できます。
- 異常なアプリケーションキー、シナリオIDが存在する場合は検証を進行しません。

### データ転送
- バッチモニタリングサーバーに検証が必要なデータを転送します。
- バッチモニタリングに入力した検証情報に応じたJSONタイプのデータを転送することができます。バッチモニタリングの検証に失敗すると障害に登録されます。

[URL]
```http
POST /v1.0/monitoring/batchmon
Content-Type: application/json
```

[Request Body]
```json
{
    "target" : [
        {
            "appkey" : "81713f1cc96b4eae834f7535ec4fae7a",
            "scenarioIdList" : [
                "12962600-5c39-11ea-bd8f-0bbd7856015c"
            ]
        },
        {
            "appkey" : "2aac90b8f3c64fcb8083d1ed0218192c",
            "scenarioIdList" : [
                "1b648d20-5c39-11ea-bd8f-0bbd7856015c"
            ]
        }
    ],
    "data" : "{\"key\": \"value\"}"
}
```


#### レスポンス
```json
{
    "body": [
        {
            "ipaddr": "127.0.0.1",
            "pk": {
                "requestId": "1958be80-5c3b-11ea-bd8f-0bbd7856015c",
                "serviceId": "128248e0-ff3b-34cd-be87-2b5bd173febb"
            },
            "requestData": {
                "body": "{\"key\": \"value\"}"
            },
            "requestTime": "2099-12-31T00:00:00.000",
            "scenarioId": "12962600-5c39-11ea-bd8f-0bbd7856015c",
            "serviceCode": 0
        },
        {
            "ipaddr": "127.0.0.1",
            "pk": {
                "requestId": "1959a8e0-5c3b-11ea-bd8f-0bbd7856015c",
                "serviceId": "1ab4e137-a158-346b-bacc-7e0c76466ac0"
            },
            "requestData": {
                "body": "{\"key\": \"value\"}"
            },
            "requestTime": "2099-12-31T00:00:00.000",
            "scenarioId": "1b648d20-5c39-11ea-bd8f-0bbd7856015c",
            "serviceCode": 1
        }
    ],
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```
| 値 | タイプ | 説明 |
|---|---|---|
| header.isSuccessful | Boolean | 成否 |
| header.resultCode | Integer | 失敗コード(0は正常) |
| header.resultMessage | String | 失敗メッセージ |
| body.pk.serviceId | String | サービス固有ID |
| body.pk.requestId | String | リクエスト固有ID |
| body.scenarioId | String | シナリオ固有ID |
| body.requestData.body | Object | リクエストデータ |
| body.ipaddr | String | リクエスト者のIPアドレス |
| body.requestTime | String | リクエスト時刻(ISO 8601フォーマット) |
| body.serviceCode | Integer | サービス固有コード |
