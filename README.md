# Bookstore API

## 1. Display a list of books
#### Request: `GET /api/books?page=0,pageSize=10`
#### Response:
**200 OK**
```json
{
  "data": [
    {
      "id": 1234,
      "name": "Clean Code",
      "authorId": 9876,
      "isbn": 9780132350884,
      /* ... */
      "category": ["Software", "Programming"]
    },
    {/* ... */}
  ],
  "page": 0,
  "totalPages": 5   
}
```

## 2 Book details
## 2.1 Get book
#### Request: `GET /api/books/{bookId}`
#### Response:
**200 OK**
```json
{
  "id": 1234,
  "name": "Clean Code",
  "authorId": 9876,
  "dateWritten": "17.07.2008",
  "datePublished": "17.07.2008",
  "pagesCount": 533,
  "originalLanguage": "English",
  /* ... */
  "isbn": 9780132350884,
  "category": ["Software", "Programming"] 
}
```
**404 Not Found** - No book found for provided `bookId`

## 2.2 Get author
#### Request: `GET /api/authors/{authorId}`
#### Response:
**200 OK**
```json
{
  "id": 9876,
  "name": "Robert Cecil Martin",
  /* ... */
  "bornOn": "08.10.1952",
  "nationality": "US" 
}
```
**404 Not Found** - No author found for provided `authorId`

## 3. Book creation
#### Request: `POST /api/books` with `Authorization` header 
```json
{
  "name": "", //mandatory
  "authorId": 123, //mandatory
  "dateWritten": "", //optional
  "pagesCount": 123 //optional
  /* other mandatory and optional book properties required for book creation */
}
```
#### Response:
**201** Created with Location header `Location: http://api.bookstore/api/books/1234`
```json
{
  "id": 1234,
  "name": "Clean Code",
  "dateWritten": "17.07.2008",
  "datePublished": "17.07.2008",
  "pagesCount": 533,
  "originalLanguage": "English",
  /* other book properties */
  "authorId": 123,
  "isbn": 9780132350884,
  "category": ["Software", "Programming"] 
}
```
**400** - Bad request
```json
{
  "message": "Request parameters are invalid",
  "errors": [
    {
      "name": "name",
      "reason": "must not be empty"
    },
    {
      "name": "pagesCount",
      "reason": "must be a positive number"
    }
  ]
}
```
**401** Unauthorized - User must authenticate to invoke the endpoint  
**403** Forbidden - User has insufficient rights to invoke the endpoint

## 4. CreatedOn and LastModifiedOn for book resource
As a part of `GET /api/books/{bookId}` we can return additional properties.   
The date-time format should be define and be consistent with other endpoints (e.g. ISO-8601).  
CreatedOn and LastModifiedOn properties should be generated on server side, the client doesn't need to provide them in request body.  
If the client provides these properties in request body, they should be ignored by the server.
```json
{
  /* book properties */
  "createdOn": "2019-05-03T15:35:14+0000",
  "lastModifiedOn": "2019-07-10T11:15:18+0000"
}
```

## 5 Search by author ids
#### Request: `POST /api/authors/search` 
```json
{
  "ids": [1, 2, 3, ...]
}
```
#### Response:
**200 OK**
```json
[
  {
    "id": 1,
    "name": "Robert Cecil Martin",
    /* ... */
    "bornOn": 9780132350884,
    "nationality": "US" 
  },
  {/* ... */}
]
```
My test change