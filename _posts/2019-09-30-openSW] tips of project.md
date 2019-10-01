# 몽고DB



### 특정 필드의 데이터만 추가

- 예시상황: 유저마다 메시지 필드를 가지고 있고, 새로운 메시지가 도착하여 해당 유저 도큐먼트의 메시지 필드에 새로운 메시지를 저장하려고 한다.

  ```js
  db.list_user.update(
  	... { "_id" : ObjectId("5d907cbc1aa4a48f1caa04a4") },
  	... { $push: {
  	...     "comu": [{ 
  				"with": "인사과장", 
  				"convs": [{ 
  					"date": "22:10:23", 
  					"imageUrl": "", 
  					"contents": "testMessage"
  				}] 
  			}]
  	...   }
  	... }
  ... )
  
  ```

  

# Mongoose



### update 사용(콜백함수 등록까지)

```js
users.update({
			position : data.header.to
			},
			{ 
				$push: {
					comu: [{ 
						with: data.header.from, 
						convs: [{ 
							id: data.msg.id,
							position: data.msg.position,
							date: data.msg.date, 
							imageUrl: data.msg.imageUrl, 
							contents: data.msg.contents,
							works: data.msg.works
						}]
					}]
				}
			},
			(e, r) => {
				if(!r){
					console.error(e)
				}
				else
					console.log('update succeeded!')
			}

		)
```





# Multer



### req.body에 데이터 전달하는 법

- 싱글 파일 받는 코드는 이렇다

  #### 서버

  ```js
  router.post('/save', upload.single('bin'), function (req, res, next) {
    // req.file is the `avatar` file
    console.log(req)
    res.status(204).send()
    // req.body will hold the text fields, if there were any
  })
  ```

  #### 프론트

  ```js
   const fd = new FormData()
        console.log(document.getElementById('bin').files[0])
        fd.append('bin', document.getElementById('bin').files[0])
        axios.post(`http://webhacker.xyz:8000/apis/db/save`, fd)
        .then(r => {
          console.log('Uploading file successfully')
        })
        .catch(e => console.error(e))
  ```

  - `FormData` 객체만 전달받게 되는데

  - 이 `FormData` 객체에 임의 key-value 값을 `append`하면 req.body에 들어간다.

    ```js
    fd.append('myData', 'hi!!')
    ```

  - `req.file`은 key값이 bin으로 입력된 value로 설정된다.

