# 몽고DB

#### 특정 필드의 데이터만 추가

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

#### update 사용(콜백함수 등록까지)

- ```js
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

  



