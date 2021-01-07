# BookBlog_Doc
>个人博客地址: https://blog.csdn.net/qq_38781909/article/details/112072404
Client地址: https://github.com/fentender/book-blog-client
Server地址: https://github.com/fentender/book-blog-server

本仓库为Book Blog博客的项目文档。本项目制作了一个前后端独立开发与部署的Book Blog博客。

## 项目简介
本次设计的博客包含资源类型：Book，User，Bookshelf，Review，Token。用户可以浏览博客内的书籍项目，并且查看书籍的相应评论。同时本次博客包含JWT验证，因此通过token来验证个人信息。而每个账号都有自己的书架，书架可以创建多个。在用户登录博客并进入书籍页面后，页面中会增加一个“加入书架”的选项，用户可以点击以将书籍加入书架当中。而Bookshelf资源只能在登录后使用，使用JWT验证。

## API设计
本次项目API如下
```
{
    //Book
    "getBook" : GET "/books/{bookId}",
    "getBooks" : GET "/books",
    
    //Review
    "getReview" : GET "/review/{reviewId}",
    "getReviews" : GET "/reviews",

    //User
    "getUser" : GET "/users/user",

    //Bookshelf
    "createBookshelf" : POST "/user/{username}/bookshelfs",
    "getBookshelfs" : GET "/users/{username}/bookshelfs",
    "getBookshelf" : GET "/users/{username}/bookshelfs/{bookshelfName}",
    "deleteBookshelf" : DELETE "/users/{username}/bookshelfs/{bookshelfName}",

    //添加书籍到书架中
    "addBookInBookshelf" : POST "/users/{username}/bookshelfs/{bookshelfName}/{bookId}",
    "deleteBookInBookshelf" : DELETE "/users/{username}/bookshelfs/{bookshelfName}/{bookId}",

    //token相关资源，用于账户JWT验证
    "signIn" : GET "/token",
    "signOut" : DELETE "/token",
    "signUp" : POST "/token"
}
```
### Get the books
得到一个书籍列表，使用pageNumber来选择对应的页数
```
GET /books
```
代码示例
```
//curl
curl -X GET "http://localhost:8080/books?pageNumber="

//javascript
var BookBlogApi = require('book_blog_api');

var api = new BookBlogApi.BookApi()

var opts = { 
  'pageNumber': 56 // {Integer} Page number
};

var callback = function(error, data, response) {
  if (error) {
    console.error(error);
  } else {
    console.log('API called successfully. Returned data: ' + data);
  }
};
api.getBooks(opts, callback);
```
Parameters:
|Name|type|
| - | - |
|pageNumber| Integer|

Response:
```
Status: 200 - A list of Book
Schema:
{
  num:	
    integer
  books:	
    [
      {
        bookId:	
          integer
        bookName:	
          string
        autor:	
          string
        info:	
          string
      }
    ]
}
```
### Delete the Bookshelf
删除用户的一个书架
```
DELETE /users/{username}/bookshelfs/{bookshelfName}
```
代码示例:
```
//curl
curl -X DELETE "http://localhost:8080/users/{username}/bookshelfs/{bookshelfName}"

//javascript
var BookBlogApi = require('book_blog_api');

var api = new BookBlogApi.UserBookshelfApi()

var username = username_example; // {String} User's ID

var bookshelfName = bookshelfName_example; // {String} Bookshelf's name


var callback = function(error, data, response) {
  if (error) {
    console.error(error);
  } else {
    console.log('API called successfully.');
  }
};
api.deleteBookshelf(username, bookshelfName, , callback);
```
Parameters:
|Name|type|
| - | - |
|username| string |
|bookshelfName| string |

Response:
```
Status: 204 - Bookshelf succesfully deleted.

Status: 403 - Bookshelf cannot be deleted.

Status: 404 - Bookshelf does not exists.
```
