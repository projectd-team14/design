# API仕様１（フロントエンドで使用するAPIのみ記載）  
各プロジェクトごとのAPIの内容です。  
※一部外に出せないもの、サーバー間で使用するAPI等はバックエンド担当者に確認をお願いします。
## システム基盤：[bicycle-system](https://github.com/projectd-team14/bicycle-system)  
・駐輪場の全体情報  
GET:http://localhost:8000/api/get_all/100
```
response
[
    {
        "id": 100,
        "name": "文教大学駐輪場A",
        "address": "神奈川県茅ケ崎市行谷1100",
        "latitude": "35.37007",
        "longitude": "139.416526",
        "max": 100,
        "count": 0,
        "overtime": 10,
        "img": "画像のパスが入ります",
        "camera": [
            {
                "id": 100,
                "name": "テスト用カメラA",
                "url": "https://www.youtube.com/watch?v=9plqYTT-3w8"
            },
            {
                "id": 101,
                "name": "テスト用カメラB",
                "url": "https://www.youtube.com/watch?v=9plqYTT-3w8"
            }
        ],
        "situation": [
            {
                "row": "A",
                "bicycle": [
                    {
                        "id": 1,
                        "time": 49,
                        "violatin_status": false,
                        "violatin_img": "null"
                    },
                    {
                        "id": 3,
                        "time": 49,
                        "violatin_status": false,
                        "violatin_img": "null"
                    },
                    {
                        "id": 5,
                        "time": 49,
                        "violatin_status": false,
                        "violatin_img": "null"
                    },
                    {
                        "id": 6,
                        "time": 49,
                        "violatin_status": false,
                        "violatin_img": "null"
                    },
                    {
                        "id": 7,
                        "time": 49,
                        "violatin_status": false,
                        "violatin_img": "null"
                    },
                    {
                        "id": 10,
                        "time": 49,
                        "violatin_status": false,
                        "violatin_img": "null"
                    },
                    {
                        "id": 13,
                        "time": 49,
                        "violatin_status": false,
                        "violatin_img": "null"
                    },
                    {
                        "id": 2,
                        "time": 38,
                        "violatin_status": false,
                        "violatin_img": "null"
                    },
                    {
                        "id": 8,
                        "time": 6,
                        "violatin_status": false,
                        "violatin_img": "null"
                    }
                ]
            }
        ]
    }
]
```
・駐輪場の個別情報  
GET:http://localhost:8000/api/get_spot/100
```
{
	"situationChartData":[
		{
			"label": "1日間",
			"backgroundColor": "#f87979",
			"data": [43,43,43,43,43,43,43,54,73,80,80,77,89,73,80,80,77,60,60,60,60,60,60,40,99],
			"labels": [0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24]
		},
		{
			"label": "1週間",
			"backgroundColor": "#f87979",
			"data": [55,55,55,55,55,55,66],
			"labels":　[1,2,3,4,5,6,7]
		},
		{
			"label": "1か月間",
			"backgroundColor": "#f87979",
			"data": [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31],
			"labels": [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31]
		},
		{
			"label": "3か月間",
			"backgroundColor": "#f87979",
			"data": [0,10,20,30,40,50,60,70,80,90],
			"labels": ["","","","6/19","","","7/19","","","8/19"]
		}
	],
	"numberChartData":[
		{
			"label": "1日間",
			"backgroundColor": "#f87979",
			"data": [10,20,40,45,15,40,30,40,45,15,40,10]
		},
		{
			"label": "1週間",
			"backgroundColor": "#f87979",
			"data": [10,20,40,45,15,40,30,40,45,15,40,10]
		},
		{
			"label": "1か月間",
			"backgroundColor": "#f87979",
			"data": [10,20,40,45,15,40,30,40,45,15,40,10]
		},
		{
			"label": "3か月間",
			"backgroundColor": "#f87979",
			"data": [10,20,40,45,15,40,30,40,45,15,40,10]
		}
	]
}
```
・ユーザー登録  
POST:http://localhost:8000/api/register
```
request
{
    "name" : "test",
    "email" : "test@bunkyo.ac.jp",
    "password" : "test"
}
response
{
    "data": {
        "name": "test",
        "email": "test@bunkyo.ac.jp",
        "updated_at": "2022-10-29T01:39:54.000000Z",
        "created_at": "2022-10-29T01:39:54.000000Z",
        "id": 100
    },
    "message": "User registration success!",
    "error": ""
}
```
・ログイン  
POST:http://localhost:8000/api/login
```
request
{
    "email" : "test@bunkyo.ac.jp",
    "password" : "test"
}
response
{
    "token": "1|p9aRELSLcy3BOEK3a43xejMiy4tX1LYynUBx3lyD",
    "user": {
        "id": 100,
        "name": "test",
        "email": "test@bunkyo.ac.jp",
        "email_verified_at": null,
        "created_at": "2022-10-29T01:39:54.000000Z",
        "updated_at": "2022-10-29T01:39:54.000000Z"
    }
}
```
・駐輪場登録  
POST:http://localhost:8000/api/store_spot/100
```
request
{
    "spots_name" : "文教大学駐輪場A",
    "spots_url" : "https://www.youtube.com/watch?v=9plqYTT-3w8",
    "spots_address" : "神奈川県茅ケ崎市行谷1100",
    "spots_img" : ""
}
response
{
    "spots_name": "文教大学駐輪場A",
    "spots_url": "https://www.youtube.com/watch?v=9plqYTT-3w8",
    "spots_address": "神奈川県茅ケ崎市行谷1100",
    "spots_img": null
}
```
・カメラ登録  
POST:http://localhost:8000/api/store_camera/100
```
request
{
    "cameras_name" : "テスト用カメラA",
    "cameras_url" : "https://www.youtube.com/watch?v=9plqYTT-3w8"
}
response
{
    "cameras_name": "テスト用カメラA",
    "cameras_url": "https://www.youtube.com/watch?v=9plqYTT-3w8"
}
```
・ラベル登録  
POST:http://localhost:8000/api/labels/100
```
request
[
	{
	    "label_mark" : "A",
	    "label_point1X" : 0,
	    "label_point1Y" : 350,
	    "label_point2X" : 0,
	    "label_point2Y" : 600,
	    "label_point3x" : 625,
	    "label_point3y" : 675,
	    "label_point4X" : 700,
	    "label_point4y" : 600
	},
	{
	    "label_mark" : "B",
	    "label_point1X" : 0,
	    "label_point1Y" : 350,
	    "label_point2X" : 0,
	    "label_point2Y" : 600,
	    "label_point3x" : 625,
	    "label_point3y" : 675,
	    "label_point4X" : 700,
	    "label_point4y" : 600
	}
	
]
response
ラベリングデータ A を登録しました
```
・駐輪場削除  
GET:http://localhost:8000/api/delete_spot/100
```
response
削除完了。
```
・カメラ削除  
GET:http://localhost:8000/api/delete_camera/100
```
response
削除完了。
```
・開始  
GET:http://localhost:8000/api/start/100
```
response
処理を開始します。
```
・停止  
GET:http://localhost:8000/api/stop/100
```
response
処理を停止します。
```
## YOLOv5専用サーバー：[yolov5-server](https://github.com/projectd-team14/yolov5-server)  
・ラベル用画像生成  
GET:http://localhost:9000/label/?id=100
```
response
生成された画像がレスポンスされます。
```
・自転車のトリミング画像  
GET:http://localhost:9000/bicycle/?camera_id=100&bicycle_id=100
```
response
各駐輪場（カメラ）ごとに自転車のトリミング画像がレスポンスされます。
```
## 管理者用サーバー：[admine-server](https://github.com/projectd-team14/admin-server)  
※このプロジェクトの開発を行う場合はバックエンド担当者に確認をお願いします。
