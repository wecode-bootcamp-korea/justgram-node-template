# JUSTGRAM Node Mission
| ๐ก ์์์ ๋ง๊ฒ Node.js ํ๊ฒฝ์์ ์คํ๋ฒ์ค ํ๋ก์ ํธ์ ํ์ํ API๋ฅผ ์์ฑํ๋ ๊ณผ์ ์๋๋ค. ๊ตฌ๊ธ ํด๋์ค๋ฃธ์ผ๋ก ์ ์ถํ๋ ๊ณผ์ ์, github์ push ํ๋ ๊ณผ์ ๊ฐ ์์ฌ์์ผ๋ ์ ์ํด์ฃผ์ธ์.

## [Mission 1] | SQL์ ํ์ฉํ์ฌ ์ธ์คํ๊ทธ๋จ ๋ฐ์ดํฐ๋ฒ ์ด์ค ๊ตฌ์ถํ๊ธฐ
**SQL๋ฌธ์ ์ด์ฉํด, ๋ชจ๋ธ๋งํ ๊ฒฐ๊ณผ๋ฅผ ํ ๋๋ก ์ธ์คํ๊ทธ๋จ ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ฅผ ๊ตฌ์ถํด๋ด๋๋ค.**

### STEP 1
- MySQL์ justgram ์ด๋ผ๋ ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ฅผ ์์ฑํด์ฃผ์ธ์.

### STEP 2

**`justgram`์๋ ์ด 4๊ฐ์ ํ์ด๋ธ์ด ์์ต๋๋ค.**

- ์ ์ ๋ฅผ ์ ์ฅํ๋ ํ์ด๋ธ : `users(id, email, nickname, password, profile_image, created_at)`
- ํฌ์คํ์ ์ ์ฅํ๋ ํ์ด๋ธ : `postings(id, user_id, contents, created_at)`
- ํฌ์คํ ์ด๋ฏธ์ง๋ฅผ ์ ์ฅํ๋ ํ์ด๋ธ : `posting_images(id, posting_id, image_url, created_at)`
- ํฌ์คํ์ ํฌํจ๋ ๋๊ธ์ ์ ์ฅํ๋ ํ์ด๋ธ : `comments(id, comment, posting_id, user_id, created_at)`

**๊ฐ ํ์ด๋ธ๊ณผ ์ปฌ๋ผ์ ์ธ๋ถ ์์ฑ์ ์๋์ ๊ฐ์ต๋๋ค.**

```sql
Table users {
  id int [pk, increment] // auto-increment
  email varchar(100) [unique, not null]
	nickname varchar(50)
	password varchar(300) [not null]
  profile_image varchar(3000)
  created_at datetime [default: `now()`]
}

Table postings {
  id int [pk, increment] // auto-increment
  user_id int [not null]
  contents varchar(2000) [null]
  created_at datetime [default: `now()`]
}
Ref: postings.user_id > users.id

Table posting_images {
	id int [pk, increment]
	posting_id int [not null]
	image_url varchar(3000)
	created_at datetime [default: `now()`]
}
Ref: posting_images.posting_id > postings.id

Table comments {
	id int [pk, increment]
	comment varchar(2000)
	posting_id int [not null]
	user_id int [not null]
  created_at datetime [default: `now()`]
}
Ref: comments.posting_id > postings.id
Ref: comments.user_id > users.id
```

### STEP 3

**SQL `INSERT INTO` ๋ฌธ์ ํ์ฉํ์ฌ ๊ฐ๊ฐ์ ํ์ด๋ธ ์์ ์๋ ๋ฐ์ดํฐ๋ฅผ ์ถ๊ฐํด์ฃผ์ธ์.**

- users
    
    
    | id | email | nickname | password | profile_image |
    | --- | --- | --- | --- | --- |
    | 1 | codeKim@justcode.co.kr | codeKim | c0DeK!m | http://profile_image_1.jpeg |
    | 2 | codeLee@justcode.co.kr | codeLee | C0dEL22 | http://profile_image_2.jpeg |
    | 3 | codePark@justcode.co.kr | codePark | P@rkCOdE! | http://profile_image_3.jpeg |
    | 4 | codeChoi@justcode.co.kr | codeChoi | ChoiCodDE | http://profile_image_4.jpeg |
- postings
    
    
    | id | user_id | contents |
    | --- | --- | --- |
    | 1 | 1 | ์์ ์ฑ๊ณต์ด ์ฃผ๋ ์ฑ์ทจ๊ฐ์ผ๋ก ํ๋ฃจ๋ฅผ ๋ง๋ฌด๋ฆฌ ํ  ์ ์๊ธฐ๋ฅผ โฃ๏ธ |
    | 2 | 1 | ์ค๋์ ์ฝ๋ฉํ๊ธฐ ์ข์๋  ๐ท |
    | 3 | 2 | db ๋ชจ๋ธ๋ง์ด ์ ๋ง ์ฌ๋ฐ์ด์ ใใ |
    | 4 | 2 | ์ด๋ ค์๋ ์ฐ๋ฆฐ ๊ฒฐ๊ตญ ํด๋ผ๊ฒ๋๋ค. |
- posting_images
    
    
    | id | posting_id | image_url |
    | --- | --- | --- |
    | 1 | 1 | http://posting_1_image_1.jpeg |
    | 2 | 1 | http://posting_1_image_2.jpeg |
    | 3 | 2 | http://posting_2_image_1.jpeg |
    | 4 | 3 | http://posting_3_image_1.jpeg |
    | 5 | 3 | http://posting_3_image_2.jpeg |
    | 6 | 3 | http://posting_3_image_3.jpeg |
    | 7 | 4 | http://posting_4_image_1.jpeg |
- comments
    
    
    | id | posting_id | comment | user_id |
    | --- | --- | --- | --- |
    | 1 | 1 | ์์ ์ฑ๊ณต์ด ๋ชจ์ฌ ํฐ ์ฑ์ทจ๊ฐ ๋๋๊น์! | 3 |
    | 2 | 4 | ๊ฐ์ด ํ๋์๋ค | 1 |
    | 3 | 2 | ํ์ด์ด ๋๋ฌด ์ข์์ ์ฝ๋ฉํ๊ณ  ์ถ์ง ์์์ | 2 |
    | 4 | 2 | ์ง๊ธ ๋น์ค๋๊ฑธ์ | 4 |
    | 5 | 3 | ๋ชจ๋ธ๋ง ์ต๊ณ ! ์๊ฐํ๋๊ฑฐ ๋๋ฌด ์ฌ๋ฐ์ฃ  | 3 |
    | 6 | 4 | ์ธ์ ๋ ๊ธธ์ด ์๊ธฐ ๋ง๋ จ์๋๋ค. | 1 |
    | 7 | 1 | ์๋ค๊ฐ ์ฝ๋ฉํ๋ ๊ฟ๊ฟ ๊ฒ ๊ฐ์์ | 4 |
    
### STEP 4


**์๋์ ๊ฒฐ๊ณผ๋ฅผ ํ์ธํด๋ณด๊ณ , JOIN ๋ฌธ์ ์๋ฏธ์ ๋ํด ํด์ํด์ฃผ์ธ์.**

```sql
SELECT users.nickname, users.profile_image, postings.contents
FROM users
JOIN postings ON postings.user_id = users.id
```

```sql
SELECT users.name, users.profile_image, postings.contents, comments.comment
FROM users
JOIN postings ON postings.user_id = users.id
JOIN comments ON comments.posting_id = postings.id
```

๊ทธ๋ ๋ค๋ฉด, 

1๋ฒ **users์ ์ด๋ฆ๊ณผ, 1๋ฒ users์ profile_image, 1๋ฒ user๊ฐ ๋จ๊ธด posting, ํด๋น posting์ ํฌํจ๋ ๋๊ธ, ๊ทธ๋ฆฌ๊ณ  ํฌํจ๋ posting_images๊น์ง ๋ถ๋ฌ์ค๊ณ ์ ํ๋ค๋ฉด ์ด๋ค SQL๋ฅผ ์์ฑํด์ผํ ๊น์?**

### STEP 5


**์ ๊ณผ์ ์ ์งํํ๋ฉฐ ๊ณต๋ถํ ๋ค์ํ join ๋ฌธ๋ค์ ์ฐจ์ด์ ๊ณผ ๊ฐ๊ฐ ์ฌ์ฉ๋๋ ์ํฉ์ ๊ตฌ์ฒด์ ์ธ ์์์ ํจ๊ป ๋ธ๋ก๊นํด์ฃผ์ธ์.**


## [Mission 2] typeORM์ ํ์ฉํ์ฌ ์๋ฒ์ ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ฐ๊ฒฐํ๊ธฐ
- ๊ณผ์  ์ค๋ช
        
    Express ํ๊ฒฝ์์ ์ธ์คํ๊ทธ๋จ๊ณผ ๊ฐ์ ์์ ๋ฏธ๋์ด ํ๋ซํผ ์ ์์ ํ์ํ API๋ฅผ ์์ฑํ๋ ๊ณผ์ ์๋๋ค.  ๊ฐ Mission์ ์์ ๋๋ก ์งํํ์ฌ Github์ ํธ์ํด์ฃผ์ธ์.
    
    - Express ์ด๊ธฐ ํ๊ฒฝ ์ค์ 
    
    ํ์ต์๋ฃ ์ค Express ์ด๊ธฐ ํ๊ฒฝ ์ธํ ๊ฐ์ด๋์ ์ ๋ฆฌ๋ ๋ด์ฉ๊ณผ ๋์ผํ๊ฒ ํ๋ก์ ํธ ํ๊ฒฝ์ ์ค์ ํด์ฃผ์ธ์.
    
    - Express ์ด๊ธฐ ํ๊ฒฝ์ค์  ๋ค์์ ๋ด์ฉ์ ์ด๊ธฐ ํ๊ฒฝ ์ค์ ์ ์ถ๊ฐํ์ฌ์ ์ ์ฉํด์ฃผ์ธ์.
        1. `Express` ์ค์น / ์ ์ฉ
        2. `nodemon` ์ค์น / ์ ์ฉ
        3. `cors ์ค์น` / ์ ์ฉ
        4. `dotenv` ์ค์น / ํ๊ฒฝ ๋ณ์ ์ ์ฉ
        5. `morgan` ์ค์น / ์ ์ฉ
    
    - TypeORM / dbConnection ์ค์ 
        
        ํ์ต์๋ฃ ์ค TypeORM ํํธ์ ์ ๋ฆฌ๋ ๋ด์ฉ๊ณผ ๋์ผํ๊ฒ ํ๋ก์ ํธ ํ๊ฒฝ์ ์ค์ ํด์ฃผ์ธ์.
        
    - Express ์ด๊ธฐ ํ๊ฒฝ์ค์  ๋ค์์ ๋ด์ฉ์ ์ด๊ธฐ ํ๊ฒฝ ์ค์ ์ ์ถ๊ฐํ์ฌ์ ์ ์ฉํด์ฃผ์ธ์.
        1. `TypeORM` ์ค์น ๋ฐ ์ ์ฉ
        2. `dbConnection` ์ ์ ์๋ ํ์ธ
 
 ### [Mission 3] ์ ์  ํ์๊ฐ์ ํ๊ธฐ

- ๊ณผ์  ์ค๋ช
    - ๊ณผ์  ์ค๋ช
        
        http ํต์ ์ ์ด์ฉํ์ฌ, ํ ๋ช ์ด์์ ์ ์ ๋ฅผ ํ์๊ฐ์ ํด๋ด๋๋ค.ย **`Express`**ย ๋ฅผ ์ด์ฉํด ๊ฐ์์ justgram ์๋น์ค์ ํ์๊ฐ์์ ์๋ฃํด์ฃผ์ธ์. ์ด ๋ ๋ณ๋์ ๋ชจ๋ํ ์์ด ํ๋์ ํ์ผ (server.js) ์์ ๋ชจ๋  ์ฝ๋๋ฅผ ์์ฑํฉ๋๋ค.
        
    - ํ์๊ฐ์ API
        1. ์๋ง์ API ํธ์ถ URL์ ์ค์ ํ์ฌ์ ํด๋ผ์ด์ธํธ์(httpie/postman) ํต์ ์ ์ฑ๊ณตํด์ฃผ์ธ์.
        2. ์๋ง์ http ๋ฉ์๋๋ฅผ ์ ์ ํ์ฌ์ ์ ์ ์ ์ ๋ณด๋ฅผ ๋ฐฑ์๋ ์๋ฒ์ ์ ๋ฌํด์ฃผ์ธ์.
        3. ๋ฐ์ดํฐ๊ฐ ์์ฑ๋ฌ์ ๋์ ์๋ง๋ http ์ํ์ฝ๋๋ฅผ ๋ฐํํด์ฃผ์ธ์.
        4. ๋ฐ์ดํฐ๊ฐ ์ db์ ์ ์ฅ๋์๋์ง ์ฌ๋ถ๋ฅผ ํ์ด๋ธ ์คํฌ๋ฆฐ์ท์ ์ฐ์ด์ PR์ ๊ฐ์ด ์ฒจ๋ถํ์ฌ ์ฌ๋ ค์ฃผ์ธ์.
        5. http response๋ก ๋ฐํํ๋ JSON ๋ฐ์ดํฐ์ ํํ๋ ๋ค์๊ณผ ๊ฐ์ต๋๋ค.
            
            ```
            {
              "message" : "userCreated"
            }
            
            ```
            
### [Mission 4] ๊ฒ์๊ธ C.R.U.D

- ๊ณผ์  ์ค๋ช
    
    ### Create
    
    - http ํต์ ์ ์ด์ฉํ์ฌ, ๋ ๊ฑด ์ด์์ ๊ฒ์๊ธ์ ๋ฑ๋กํด๋ด๋๋ค. ์ด ๋ ์์ ์์ฑํ์๋ ํ์ผ (app.js) ์์ ์ด์ด์ ์ฝ๋๋ฅผ ์์ฑํฉ๋๋ค.
        - ๊ฒ์๊ธ ๋ฑ๋ก API
            1. ์๋ง์ API ํธ์ถ URL์ ์ค์ ํ์ฌ์ ํด๋ผ์ด์ธํธ์(httpie/postman) ํต์ ์ ์ฑ๊ณตํด์ฃผ์ธ์.
            2. ์๋ง์ http ๋ฉ์๋๋ฅผ ์ ์ ํ์ฌ์ ๊ฒ์๊ธ ๋ด์ฉ ๋ฐ ์ ์ ์ id ๊ฐ์ ๋ฐฑ์๋ ์๋ฒ์ ์ ๋ฌํด์ฃผ์ธ์.
            3. ๋ฐ์ดํฐ๊ฐ ์์ฑ๋ฌ์ ๋์ ์๋ง๋ http ์ํ์ฝ๋๋ฅผ ๋ฐํํด์ฃผ์ธ์.
            4. ๋ฐ์ดํฐ๊ฐ ์ db์ ์ ์ฅ๋์๋์ง ์ฌ๋ถ๋ฅผ ํ์ด๋ธ ์คํฌ๋ฆฐ์ท์ ์ฐ์ด์ PR์ ๊ฐ์ด ์ฒจ๋ถํ์ฌ ์ฌ๋ ค์ฃผ์ธ์.
            5. http response๋ก ๋ฐํํ๋ JSON ๋ฐ์ดํฐ์ ํํ๋ ๋ค์๊ณผ ๊ฐ์ต๋๋ค.
                
                ```json
                {
                  "message" : "postCreated"
                }
                ```
                
        
        ### Read
        
        - http ํต์ ์ ์ด์ฉํ์ฌ, ๊ฒ์๊ธ ์ ์ฒด ๋ฆฌ์คํธ๋ฅผ ๋ถ๋ฌ์๋ด๋๋ค. ์ด ๋ ์์ ์์ฑํ์๋ ํ์ผ (app.js) ์์ ์ด์ด์ ์ฝ๋๋ฅผ ์์ฑํฉ๋๋ค.
        - ๊ฒ์๊ธ ๋ฆฌ์คํธ ๋ถ๋ฌ์ค๊ธฐ API
            1. ์๋ง์ API ํธ์ถ URL์ ์ค์ ํ์ฌ์ ํด๋ผ์ด์ธํธ์(httpie/postman) ํต์ ์ ์ฑ๊ณตํด์ฃผ์ธ์.
            2. ๋ฐ์ดํฐ๊ฐ ํธ์ถ๋  ๋์ ์๋ง๋ http ์ํ์ฝ๋๋ฅผ ๋ฐํํด์ฃผ์ธ์.
            3. http response๋ก ๋ฐํํ๋ ๋ฐ์ดํฐ์ ํํ๋ ๋ค์๊ณผ ๊ฐ์ต๋๋ค.
                
                ```json
                {
                    "data" : [
                	{
                	    "userId"           : 1,
                	    "userProfileImage" : "userProfileImage1",
                      "postingId"        : 1,
                      "postingImageUrl"  : "imageUrlSample1",
                	    "postingContent"   : "sampleContent1"
                	},
                	{
                	    "userId"           : 2,
                	    "userProfileImage" : "userProfileImage2",
                      "postingId"        : 2,
                      "postingImageUrl"  : "imageUrlSample2",
                	    "postingContent"   : "sampleContent2"
                	},
                	{
                	    "userId"           : 3,
                	    "userProfileImage" : "userProfileImage3",
                      "postingId"        : 3,
                      "postingImageUrl"  : "imageUrlSample3",
                	    "postingContent"   : "sampleContent3"
                	},
                	{
                	    "userId"           : 4,
                	    "userProfileImage" : "userProfileImage4",
                      "postingId"        : 4,
                      "postingImageUrl"  : "imageUrlSample4",
                	    "postingContent"   : "sampleContent4"
                	}
                ]}
                
                ```
                
    
    ### Read 2
    
    - http ํต์ ์ ์ด์ฉํ์ฌ, ํ๋ช์ ์ ์ ๊ฐ ์์ฑํ ๊ฒ์๊ธ์ ๋ถ๋ฌ์ต๋๋ค. ์ด ๋ ์์ ์์ฑํ์๋ ํ์ผ (app.js) ์์ ์ด์ด์ ์ฝ๋๋ฅผ ์์ฑํฉ๋๋ค.
    - ํน์  ์ ์ ๊ฐ ์์ฑํ ๊ฒ์๊ธ ์์ธ API
        1. ์๋ง์ API ํธ์ถ URL์ ์ค์ ํ์ฌ์ ํด๋ผ์ด์ธํธ์(httpie/postman) ํต์ ์ ์ฑ๊ณตํด์ฃผ์ธ์.
        2. ํ๋์ ์ ์ ๊ฐ ์ฌ๋ฆฐ ์ฌ๋ฌ๊ฐ์ ๊ฒ์๊ธ์ ๋ณด์ฌ์ฃผ๋ ์์ธ ํ์ด์ง์ฉ API๋ฅผ ์์ฑํด์ฃผ์ธ์.
        3. ๋ฐ์ดํฐ๊ฐ ํธ์ถ๋  ๋์ ์๋ง๋ http ์ํ์ฝ๋๋ฅผ ๋ฐํํด์ฃผ์ธ์.
        4. Http response๋ก ๋ฐํํ๋ ๋ฐ์ดํฐ์ ํํ๋ ๋ค์๊ณผ ๊ฐ์ต๋๋ค.
            
            ```json
            {
                "data" : {
                    "userId"           : 1,
            				"userProfileImage" : "userProfileImage1",
            				"postings" : [{
            		        "postingId"        : 1,
            		        "postingImageUrl"  : "postingImageUrlSample1",
            		        "postingContent"   : "samplePostingContent1"
            		     },{
            		        "postingId"        : 2,
            		        "postingImageUrl"  : "postingImageUrlSample2",
            		        "postingContent"   : "samplePostingContent2"
            		     },{
            		        "postingId"        : 3,
            		        "postingImageUrl"  : "postingImageUrlSample3",
            		        "postingContent"   : "samplePostingContent3"
                    }]
                }
            }
            
            ```
            
    
    ### Update
    
    - ๊ฒ์๊ธ ์ ๋ณด ์์  ์๋ํฌ์ธํธ ์๊ตฌ ์ฌํญ
        1. ์๋ง์ API ํธ์ถ URL์ ์ค์ ํ์ฌ์ ํด๋ผ์ด์ธํธ์(httpie/postman) ํต์ ์ ์ฑ๊ณตํด์ฃผ์ธ์.
        2. ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ ์ฅ๋์ด์๋ 1๋ฒ id ์ ์ ์ 1๋ฒ id ๊ฒ์๊ธ์ ํ์ธํฉ๋๋ค. ์ดํ ํด๋น ๋ด์ญ์ content๋ฅผ ๊ธฐ์กด์ ๋ฐ์ดํฐ์ ๋ค๋ฅธ ๋ด์ฉ์ผ๋ก ์์ ํ์ฌ ์ ์ฅํฉ๋๋ค.
        3. postingId๊ฐ 1๋ฒ์ธ ๊ฒ์๋ฌผ์ ๋ด์ฉ์ โ๊ธฐ์กด๊ณผ ๋ค๋ฅด๊ฒ ์์ ํ ๋ด์ฉ์๋๋ค.โ๋ก ์์ ํ๋ค๋ฉด, ๋ฐ์ดํฐ๋ฒ ์ด์ค์์ ๋์ด์ http response๋ก ๋ฐํํ๋ ๋ฐ์ดํฐ์ ํํ๋ ๋ค์๊ณผ ๊ฐ์ต๋๋ค.
            
            ```json
            {
            	"data" : {
            	    "userId"           : 1,
            	    "userName"         : "weCode",
                  "postingId"        : 1,
                  "postingTitle"     : "๊ฐ๋จํ HTTP API ๊ฐ๋ฐ ์์!",
            	    "postingContent"   : "๊ธฐ์กด๊ณผ ๋ค๋ฅด๊ฒ ์์ ํ ๋ด์ฉ์๋๋ค."
            	}
            }
            ```
            
    
    ### Delete
    
    - ๊ณผ์  ์ค๋ช
        
        http ํต์ ์ ์ด์ฉํ์ฌ, ํ ๊ฑด์ ๊ฒ์๊ธ์ ์ญ์ ํด๋ด๋๋ค. ์ด ๋ ์์ ์์ฑํ์๋ ํ์ผ (app.js) ์์ ์ด์ด์ ์ฝ๋๋ฅผ ์์ฑํฉ๋๋ค.
        
    - ๊ฒ์๊ธ ์ญ์  API
        1. ์๋ง์ API ํธ์ถ URL์ ์ค์ ํ์ฌ์ ํด๋ผ์ด์ธํธ์(httpie/postman) ํต์ ์ ์ฑ๊ณตํด์ฃผ์ธ์.
        2. ๋ฐ์ดํฐ๊ฐ ์ญ์  ๋  ๋์ ์๋ง๋ http ์ํ์ฝ๋๋ฅผ ๋ฐํํด์ฃผ์ธ์.
        3. http response๋ก ๋ฐํํ๋ JSON ๋ฐ์ดํฐ์ ํํ๋ ๋ค์๊ณผ ๊ฐ์ต๋๋ค.
            
            ```json
            {
            	"message" : "postingDeleted"
            }
            ```
            
    
    ### ์ ํ๊ณผ์  - ์ข์์ ๋๋ฅด๊ธฐ
    
    - ๊ณผ์  ์ค๋ช
        - http ํต์ ์ ์ด์ฉํ์ฌ, ํ ๊ฑด์ ์ข์์๋ฅผ ์์ฑํด๋ด๋๋ค. ์ด ๋ ์์ ์์ฑํ์๋ ํ์ผ (app.js) ์์ ์ด์ด์ ์ฝ๋๋ฅผ ์์ฑํฉ๋๋ค.
    - ๊ฒ์๊ธ ์ข์์ ๋๋ฅด๊ธฐ API
        1. ์๋ง์ API ํธ์ถ URL์ ์ค์ ํ์ฌ์ ํด๋ผ์ด์ธํธ์(httpie/postman) ํต์ ์ ์ฑ๊ณตํด์ฃผ์ธ์.
        2. ๋ฐ์ดํฐ๊ฐ ์์ฑ ๋ฌ์ ๋ (์ข์์๊ฐ ๋๋ ธ์ ๋์) ์๋ง๋ http ์ํ์ฝ๋๋ฅผ ๋ฐํํด์ฃผ์ธ์.
        3. http response๋ก ๋ฐํํ๋ JSON ๋ฐ์ดํฐ์ ํํ๋ ๋ค์๊ณผ ๊ฐ์ต๋๋ค.
        
 ### [Mission 5] ๋น๋ฐ ๋ฒํธ ์ํธํ

- ๊ณผ์  ์ค๋ช
    - ๊ณผ์  ์ค๋ช
        
        http ํต์ ์ ์ด์ฉํ์ฌ, ํ ๋ช ์ด์์ ์ ์ ๋ฅผ ํ์๊ฐ์ ํด๋ด๋๋ค.ย **`Express`**ย ๋ฅผ ์ด์ฉํด ๊ฐ์์ justgram ์๋น์ค์ ํ์๊ฐ์์ ์๋ฃํด์ฃผ์ธ์. ์ด ๋ ๋ณ๋์ ๋ชจ๋ํ ์์ด ํ๋์ ํ์ผ (server.js) ์์ ๋ชจ๋  ์ฝ๋๋ฅผ ์์ฑํฉ๋๋ค.
        
    - ํ์๊ฐ์ API
        1. ์๋ง์ API ํธ์ถ URL์ ์ค์ ํ์ฌ์ ํด๋ผ์ด์ธํธ์(httpie/postman) ํต์ ์ ์ฑ๊ณตํด์ฃผ์ธ์.
        2. ์๋ง์ http ๋ฉ์๋๋ฅผ ์ ์ ํ์ฌ์ ์ ์ ์ ์ ๋ณด๋ฅผ ๋ฐฑ์๋ ์๋ฒ์ ์ ๋ฌํด์ฃผ์ธ์.
        3. ๋ฐฑ์๋์์ ํด๋น ๋น๋ฐ๋ฒํธ๋ฅผ ์ํธํํ์ฌ ์ ์ฅํด์ฃผ์ธ์.
        4. ๋ฐ์ดํฐ๊ฐ ์ db์ ์ ์ฅ๋์๋์ง ์ฌ๋ถ๋ฅผ ํ์ด๋ธ ์คํฌ๋ฆฐ์ท์ ์ฐ์ด์ PR์ ๊ฐ์ด ์ฒจ๋ถํ์ฌ ์ฌ๋ ค์ฃผ์ธ์.
        5. http response๋ก ๋ฐํํ๋ JSON ๋ฐ์ดํฐ์ ํํ๋ ๋ค์๊ณผ ๊ฐ์ต๋๋ค.
            
            ```
            {
              "message" : "userCreated"
            }
            
            ```
            
 ### [Mission 6] ์ ์  ๋ก๊ทธ์ธํ๊ธฐ

- ๊ณผ์  ์ค๋ช
    - ๊ณผ์  ์ค๋ช
        
        http ํต์ ์ ์ด์ฉํ์ฌ, ํ ๋ช ์ด์์ ์ ์ ๋ฅผ ํ์๊ฐ์ ํด๋ด๋๋ค.ย **`Express`**ย ๋ฅผ ์ด์ฉํด ๊ฐ์์ justgram ์๋น์ค์ ๋ก๊ทธ์ธ์ ์๋ฃํด์ฃผ์ธ์. ์ด ๋ ๋ณ๋์ ๋ชจ๋ํ ์์ด ํ๋์ ํ์ผ (server.js) ์์ ๋ชจ๋  ์ฝ๋๋ฅผ ์์ฑํฉ๋๋ค.
        
    - ํ์๊ฐ์ API
        1. ์๋ง์ API ํธ์ถ URL์ ์ค์ ํ์ฌ์ ํด๋ผ์ด์ธํธ์(httpie/postman) ํต์ ์ ์ฑ๊ณตํด์ฃผ์ธ์.
        2. ์๋ง์ http ๋ฉ์๋๋ฅผ ์ ์ ํ์ฌ์ ์ ์ ์ ์ ๋ณด๋ฅผ ๋ฐฑ์๋ ์๋ฒ์ ์ ๋ฌํด์ฃผ์ธ์.
        3. ๋ฐฑ์๋์์๋ ๊ฐ์๋์ด ์๋ ์ ์ ์ธ์ง ํ์ธ ํ, ๊ฐ์๋์ด ์์ง ์์ ์ ์ ๋ผ๋ฉด ์๋ฌ๋ฅผ ์ ๋ฌํฉ๋๋ค. 404, userDoesNotExist
            
            ```
            {
              "message" : "userDoesNotExist"
            }
            
            ```
            
        4. ๊ฐ์๋์ด ์๋ ์ ์ ๋ผ๋ฉด ํด๋น ์ ์ ์ id๋ก ๋ง๋ค์ด์ง jwt๋ฅผ ๋ฐํํด์ฃผ์ธ์.
        5. http response๋ก ๋ฐํํ๋ JSON ๋ฐ์ดํฐ์ ํํ๋ ๋ค์๊ณผ ๊ฐ์ต๋๋ค.
            
            ```
            {
              "message" : "loginSuccess",
              "token": "ey_________"
            }
            
            ```
            
  ---
  
  ### [์ถ๊ฐ Mission 1] CRUD - Create & Delete (์๋ฃ Like ๊ธฐ๋ฅ)

- ์ ์ ๊ฐ ํน์  ์๋ฃ์ ์ข์์๋ฅผ ๋๋ฅผ ์ ์์ต๋๋ค.
- ์ข์์๋ฅผ ๋๋ ๋ ์ํ์์ ๋ค์ ์ข์์๋ฅผ ๋๋ฅด๋ฉด ์ข์์๊ฐ ์ฌ๋ผ์ง๋๋ค.
- Hint
    - `์ข์์`๋ฅผ ๊ด๋ฆฌํ๋ ํ์ด๋ธ์ด ํ์ํฉ๋๋ค.
    
 ### [์ถ๊ฐ Mission 2]  CRUD - comments or reviews

- ํน์  ์ ์ ๋ ํน์  ์๋ฃ์ ๋๊ธ (๋ฆฌ๋ทฐ) ๋ฅผ ๋ฌ ์ ์์ต๋๋ค.
- ๋ฆฌ๋ทฐ๋ฅผ ๋จ๊ธด ์๊ฐ์ ๊ธฐ๋กํฉ๋๋ค.
- ๋ด ๋ฆฌ๋ทฐ๋ ๋ด๊ฐ ์์ ํ  ์ ์์ต๋๋ค.
- ๋ด ๋ฆฌ๋ทฐ๋ ๋ด๊ฐ ์ญ์ ํ  ์ ์์ต๋๋ค.
- ๋ด ๋ฆฌ๋ทฐ๋ ๋ค๋ฅธ ์ฌ๋์ด ์ญ์ ํ๊ฑฐ๋ ์์ ํ  ์ ์์ต๋๋ค.
- ์ฌํ
    - ๋๋๊ธ์ ๋ฌ ์ ์์ต๋๋ค.
    - ๋ด ๋๊ธ์ ๋ฐ๋ผ๋ณด๊ณ  ์๋ ์ถ๊ฐ ๋๊ธ์ ๋ฌ ์ ์์ต๋๋ค.
    
