# Q1: 設計餐廳論壇 API

## 前台 API

### 和 User 有關的路由

| Description                            | Method | Path                     | Parameter | Query |
| -------------------------------------- | ------ | ------------------------ | --------- | ----- |
| 使用者註冊                             | POS    | /api/users/sing-in       | ---       | ---   |
| 使用者登入                             | POST   | /api/users/login         | ---       | ---   |
| 使用者個人資料                         | GET    | /api/users/:id           | id        | ---   |
| 修改使用者資料                         | PUT    | /api/users/:id           | id        | ---   |
| 使用者收藏餐廳                         | GET    | /api/users/:id/favorites | id        | ---   |
| 使用者評論過的餐廳                     | GET    | /api/users/:id/comments  | id        | ---   |
| 使用者自己追蹤的使用者                 | GET    | /api/users/:id/followers | id        | ---   |
| 使用者自己追蹤的使用者正在追蹤的使用者 | GET    | /api/users/:id/followers | id        | ---   |

### 和 Follower 有關的路由

| Description          | Method | Path               | Parameter | Query |
| -------------------- | ------ | ------------------ | --------- | ----- |
| 使用者追蹤其他使用者 | POST   | /api/followers/    | ---       | ---   |
| 使用者移除追蹤使用者 | DELETE | /api/followers/:id | id        | ---   |

### 和 Restaurant 有關的路由

| Description            | Method | Path                          | Parameter | Query    |
| ---------------------- | ------ | ----------------------------- | --------- | -------- |
| 新增一筆餐廳資料       | POST   | /api/restaurants              | ---       | ---      |
| 取得餐廳資料列表       | GET    | /api/restaurants              | ---       | category |
| 取得單筆餐廳詳細資料   | GET    | /api/restaurants/:id          | id        | ---      |
| 更新單筆餐廳詳細資料   | PUT    | /api/restaurants/:id          | id        | ---      |
| 刪除單筆餐廳詳細資料   | GET    | /api/restaurants/:id          | id        | ---      |
| 最新上架的十筆餐廳資料 | GET    | /api/restaurants/feeds        | ---       | ---      |
| 該筆餐廳的評論         | GET    | /api/restaurants/:id/comments | id        | ---      |

### 和 comment 有關的路由

| Description          | Method | Path                | Parameter | Query |
| -------------------- | ------ | ------------------- | --------- | ----- |
| 使用者對餐廳留下評論 | POST   | /api/comments/      | ---       | ---   |
| 最新十筆評論         | GET    | /api/comments/feeds | ---       | ---   |
| 使用者刪除餐廳評論   | DELETE | /api/comments/:id   | id        | ---   |

### 和 favorite 相關的路由

| Description        | Method | Path               | Parameter | Query |
| ------------------ | ------ | ------------------ | --------- | ----- |
| 使用者加入蒐藏餐廳 | POST   | /api/favorites     | ---       | ---   |
| 使用者移除蒐藏餐廳 | DELETE | /api/favorites/:id | id        | ---   |

## 後台 API

可否使用後台會依據 JWT token 的使用者資料，再向資料庫查詢 table 的 is_admin 是否為 true ，條件成立才能使用後台 API

### 和 User 有關的路由

| Description            | Method | Path                 | Parameter | Query |
| ---------------------- | ------ | -------------------- | --------- | ----- |
| 取得使用者清單         | GET    | /api/admin/users     | ---       | ---   |
| 取得單一使用者詳細資料 | GET    | /api/admin/users/:id | id        | ---   |
| 修改使用者資料         | PUT    | /api/admin/users/:id | id        | ---   |
| 刪除使用者資料         | DELETE | /api/admin/users/:id | id        | ---   |

### 和 Category 有關的路由

| Description          | Method | Path                      | Parameter | Query |
| -------------------- | ------ | ------------------------- | --------- | ----- |
| 取得所有餐廳分類清單 | GET    | /api/admin/categories     | ---       | ---   |
| 取的餐廳單一分類資料 | GET    | /api/admin/categories/:id | id        | ---   |
| 修改餐廳單一分類資料 | PUT    | /api/admin/categories/:id | id        | ---   |
| 刪除餐廳單一分類     | DELETE | /api/admin/categories/:id | id        | ---   |

### 和 Restaurant 有關的路由

| Description      | Method | Path                       | Parameter | Query |
| ---------------- | ------ | -------------------------- | --------- | ----- |
| 取得餐廳清單     | GET    | /api/admin/restaurants     | ---       | ---   |
| 新增一筆餐廳資料 | POST   | /api/admin/restaurants/    | ---       | ---   |
| 取得單一餐廳資料 | GET    | /api/admin/restaurants/:id | id        | ---   |
| 修改單一餐廳資料 | PUT    | /api/admin/restaurants/:id | id        | ---   |
| 刪除單一餐廳資料 | DELETE | /api/admin/restaurants/:id | id        | ---   |

### 路由設計時候的考量

依據之前課程的經驗，以 RESTful API 設計的路由基本上可以將 RUN (Uniform Resource Name) 資源位址分為四個部分，不過大部分是基於我個人經驗的猜測

1. 第一部份的**資源目的**，像是 `/api`, `/api/v1`, `/api/admin` 等，用這種方式已表示這個路由的設計目的 APT 接口、 API 接口第一版、專門設計給管理員使用的 API 接口等等
2. 第二個部分則是**資源模型**，像是後面接的 `restaurants`, `categories`, `users` 皆是表示資源模型，且以複數名詞表示，通常表示對映該模型的 controller ，每個 controller 通常會在對映到一分主要的資料表
3. 第三個部分 Parameter 表示**對資源模型對映的資源，或是資料表操作方式**，如沒有 Parameter 表示是針對整份資料表的操作，如果是 `/:id` 表示對資料表某個主鍵為變數 `:id` 單個資料操作(不過我有看過將這個部分放在 Query 的操作方式)，如果是 Parameter 後面還有接其他 Parameter 參數如果沒有接主鍵變數 `/:id` 通常是對該資料表的進一部資料篩選操作，如 `/top-10-favorites` 表示篩選前十大蒐藏清單、 `/new` 表示要取得以建立日期排序的資料；有接主鍵變數 `/:id` ，通常表示以該主鍵對於其他資料表的關聯查詢，如 `/:id/comments` 即表示查詢該 id 的留言
4. **Query** 這部分基本上是可有可無，對於資料較細節的操作參數，或是其他操作，如帶入追蹤、其他紀錄資訊等，如帶入 `keyword` 關鍵字查詢，或是 `category`, `limit`, `page` 等分類、分頁條件等等，通常是在 controller 上設有預設值，或是條件處理

### 想要請教的部分

我在網路上經常看調 URI 的部分有中文、日文、韓文等非 ASCII 字元，但是把網址複製下來會變成亂碼，如 https://zh.wikipedia.org/wiki/%E7%B5%82%E7%AB%AF ，那是什麼技術？

# Q2: 撰寫 API 文件

請為 Q1 清單中你列出的所有「和 Restaurant 有關」的 API 寫一份對應的文件，文件內容須包含每條 API 的：

## 和 Restaurant 有關的路由

### 新增一筆餐廳資料 /api/admin/restaurants

- HTTP Method: POST
- 路徑： /api/admin/restaurants/
- 中文說明 - 這條 API 是做什麼的： 新增一筆餐廳資料
- Parameters: n/a
- Query: n/a
- Request:
  - http request head
    在 request http headAuthorization 放數使用者的 JWT token ，才能存取資料，其格式範例大概如下：
    _http request head_
  ```
  Authorization: Bearer [Your JWT token]
  ```
  - request body:
  ```json
  {
    "name": "Larry Braun",
    "tel": "(918) 439-0293 x65812",
    "address": "82439 Arthur Place",
    "openingHours": "08:00",
    "description": "non voluptatibus voluptatum",
    "image": "https://loremflickr.com/320/240/restaurant,    food/?random=58.395672127388785",
    "viewCount": 38,
    "createdAt": "2024-07-12T15:09:48.000Z",
    "updatedAt": "2024-09-08T04:50:08.000Z",
    "CategoryId": 15
  }
  ```
- Response:
  - success:
    - http status code: 200
    - http response body:
    ```json
    {
      "status": "success",
      "data": {
        "newRestaurant": {
          "id": 101,
          "name": "Larry Braun",
          "tel": "(918) 439-0293 x65812",
          "address": "82439 Arthur Place",
          "openingHours": "08:00",
          "description": "non voluptatibus voluptatum",
          "image": "https://loremflickr.com/320/240/restaurant,    food/?random=58.395672127388785",
          "viewCount": 38,
          "createdAt": "2024-07-12T15:09:48.000Z",
          "updatedAt": "2024-09-08T04:50:08.000Z",
          "CategoryId": 15
        }
      }
    }
    ```
  - Failure:
    - http status code: 500
    - http response body:
    ```json
    {
      "status": "error",
      "message": "Your error messages"
    }
    ```

### 取得餐廳資料列表 /api/admin/restaurants

- HTTP Method: GET
- 路徑： /api/admin/restaurants
- 中文說明 - 這條 API 是做什麼的： 取得餐廳列表
- Parameters: n/a
- Query: category
  數值為整數
  非必要，沒有的時候可以取得所有種類的餐廳資料，有的時候可以取的特定餐廳種類的資料
- Request:
  - http request head
    在 request http headAuthorization 放數使用者的 JWT token ，才能存取資料，其格式範例大概如下：
    _http request head_
  ```
  Authorization: Bearer [Your JWT token]
  ```
  - request body: n/a
- Response:
  - success:
    - http status code: 200
    - http response body:
    ```json
    {
      "status": "success",
      "data": {
        "restaurants": [
          {
            "id": 101,
            "name": "Larry Braun",
            "tel": "(918) 439-0293 x65812",
            "address": "82439 Arthur Place",
            "openingHours": "08:00",
            "description": "non voluptatibus voluptatum",
            "image": "https://loremflickr.com/320/240/restaurant,    food/?random=58.395672127388785",
            "viewCount": 38,
            "createdAt": "2024-07-12T15:09:48.000Z",
            "updatedAt": "2024-09-08T04:50:08.000Z",
            "CategoryId": 15
          }, ...
        ]
      }
    }
    ```
  - Failure:
    - http status code: 500
    - http response body:
    ```json
    {
      "status": "error",
      "message": "Your error messages"
    }
    ```

### 取得單筆餐廳詳細資料 /api/restaurants/:id

- HTTP Method: GET
- 路徑： /api/admin/restaurants/:id
- 中文說明 - 這條 API 是做什麼的： 取的單筆餐廳的詳細資料
- Parameters: /:id
  其數值為整數
- Query: n/a
  其數值須為整數
- Request:
  - http request head
    在 request http headAuthorization 放數使用者的 JWT token ，才能存取資料，其格式範例大概如下：
    _http request head_
  ```
  Authorization: Bearer [Your JWT token]
  ```
  - request body: n/a
- Response:
  - success:
    - http status code: 200
    - http response body:
    ```json
    {
      "status": "success",
      "data": {
        "restaurant": {
          "id": 101,
          "name": "Larry Braun",
          "tel": "(918) 439-0293 x65812",
          "address": "82439 Arthur Place",
          "openingHours": "08:00",
          "description": "non voluptatibus voluptatum",
          "image": "https://loremflickr.com/320/240/restaurant,    food/?random=58.395672127388785",
          "viewCount": 38,
          "createdAt": "2024-07-12T15:09:48.000Z",
          "updatedAt": "2024-09-08T04:50:08.000Z",
          "CategoryId": 15
        }
      }
    }
    ```
  - Failure:
    - http status code: 500
    - http response body:
    ```json
    {
      "status": "error",
      "message": "Your error messages"
    }
    ```

### 更新單筆餐廳詳細資料 /api/restaurants/:id

- HTTP Method: PUT
- 路徑： /api/admin/restaurants/:id
- 中文說明 - 這條 API 是做什麼的： 更新單筆餐廳資料
- Parameters: /:id
  其數值為整數
- Query: n/a
- Request:
  - http request head
    在 request http headAuthorization 放數使用者的 JWT token ，才能存取資料，其格式範例大概如下：
    _http request head_
  ```
  Authorization: Bearer [Your JWT token]
  ```
  - request body:
  ```json
  {
    "name": "Larry Braun",
    "tel": "(918) 439-0293 x65812",
    "address": "82439 Arthur Place",
    "openingHours": "08:00",
    "description": "non voluptatibus voluptatum",
    "image": "https://loremflickr.com/320/240/restaurant,    food/?random=58.395672127388785",
    "viewCount": 38,
    "createdAt": "2024-07-12T15:09:48.000Z",
    "updatedAt": "2024-09-08T04:50:08.000Z",
    "CategoryId": 15
  }
  ```
- Response:
  - success:
    - http status code: 200
    - http response body:
    ```json
    {
      "status": "success",
      "data": {
        "updateRestaurant": {
          "id": 101,
          "name": "Larry Braun",
          "tel": "(918) 439-0293 x65812",
          "address": "82439 Arthur Place",
          "openingHours": "08:00",
          "description": "non voluptatibus voluptatum",
          "image": "https://loremflickr.com/320/240/restaurant,    food/?random=58.395672127388785",
          "viewCount": 38,
          "createdAt": "2024-07-12T15:09:48.000Z",
          "updatedAt": "2024-09-08T04:50:08.000Z",
          "CategoryId": 15
        }
      }
    }
    ```
  - Failure:
    - http status code: 500
    - http response body:
    ```json
    {
      "status": "error",
      "message": "Your error messages"
    }
    ```

### 刪除單筆餐廳詳細資料 /api/restaurants/:id

- HTTP Method: DELETE
- 路徑： /api/admin/restaurants/:id
- 中文說明 - 這條 API 是做什麼的： 刪除單筆餐廳資料
- Parameters: /:id
  其數值為整數
- Query: n/a
- Request:
  - http request head
    在 request http headAuthorization 放數使用者的 JWT token ，才能存取資料，其格式範例大概如下：
    _http request head_
  ```
  Authorization: Bearer [Your JWT token]
  ```
  - request body: n/a
- Response:
  - success:
    - http status code: 200
    - http response body:
    ```json
    {
      "status": "success",
      "data": {
        "deleteRestaurant": {
          "id": 101,
          "name": "Larry Braun",
          "tel": "(918) 439-0293 x65812",
          "address": "82439 Arthur Place",
          "openingHours": "08:00",
          "description": "non voluptatibus voluptatum",
          "image": "https://loremflickr.com/320/240/restaurant,    food/?random=58.395672127388785",
          "viewCount": 38,
          "createdAt": "2024-07-12T15:09:48.000Z",
          "updatedAt": "2024-09-08T04:50:08.000Z",
          "CategoryId": 15
        }
      }
    }
    ```
  - Failure:
    - http status code: 500
    - http response body:
    ```json
    {
      "status": "error",
      "message": "Your error messages"
    }
    ```

### 最新上架的十筆餐廳資料 /api/restaurants/feeds

- HTTP Method: GET
- 路徑： /api/restaurants/feeds
- 中文說明 - 這條 API 是做什麼的： 取得最新上架的十筆餐廳資料
- Parameters: n/a
- Query: n/a
- Request:
  - http request head
    在 request http headAuthorization 放數使用者的 JWT token ，才能存取資料，其格式範例大概如下：
    _http request head_
  ```
  Authorization: Bearer [Your JWT token]
  ```
  - request body: n/a
- Response:
  - success:
    - http status code: 200
    - http response body:
    ```json
    {
      "status": "success",
      "data": {
        "restaurants": [
          {
            "id": 101,
            "name": "Larry Braun",
            "tel": "(918) 439-0293 x65812",
            "address": "82439 Arthur Place",
            "openingHours": "08:00",
            "description": "non voluptatibus voluptatum",
            "image": "https://loremflickr.com/320/240/restaurant,    food/?random=58.395672127388785",
            "viewCount": 38,
            "createdAt": "2024-07-12T15:09:48.000Z",
            "updatedAt": "2024-09-08T04:50:08.000Z",
            "CategoryId": 15
          }, ... 共計十筆依據日期由新到舊排序的餐廳資料
        ]
      }
    }
    ```
  - Failure:
    - http status code: 500
    - http response body:
    ```json
    {
      "status": "error",
      "message": "Your error messages"
    }
    ```

### 該筆餐廳的評論 /api/restaurants/:id/comments

- HTTP Method: GET
- 路徑： /api/restaurants/:id/comments
- 中文說明 - 這條 API 是做什麼的： 取得該筆餐廳的評論
- Parameters: /:id
  其數值為整數
- Query: n/a
- Request:
  - http request head
    在 request http headAuthorization 放數使用者的 JWT token ，才能存取資料，其格式範例大概如下：
    _http request head_
  ```
  Authorization: Bearer [Your JWT token]
  ```
  - request body: n/a
- Response:
  - success:
    - http status code: 200
    - http response body:
    ```json
    {
      "status": "success",
      "data": {
        "comments": [
          {
            "id": 3,
            "text": "Natus tenetur unde quod fugit perferendis culpa qui debitis.",
            "restaurantId": 107,
            "userId": 7
          }, ...
        ]
      }
    }
    ```
  - Failure:
    - http status code: 500
    - http response body:
    ```json
    {
      "status": "error",
      "message": "Your error messages"
    }
    ```
