# Hoisting

![](.gitbook/assets/hoisting-js-cover.png)

> ### **Hoisting** là một cơ chế Javascript nơi mà các biến và function khi khai báo sẽ được đưa lên trên cùng của **scope** trước khi code thực thi.

Điều này có nghĩa là bất kể các hàm và biến được khai báo ở đâu đi nữa thì chúng đều được chuyển lên đầu phạm vi của chúng.

**Lưu ý là nó chỉ di chuyển khai báo**. Còn việc gán giá trị thì vẫn giữ nguyên. Đây là lưu ý quan trọng, nếu bạn chưa hiểu thì hãy đọc tiếp!

### Biến Hoisting

Một vòng đời bình thường của biến trong Javascript là Khai báo =&gt; Gán giá trị =&gt; Sử dụng =&gt; Giải phóng. Ví dụ 

```javascript
var a;
a = 100;
a + 30;
```

Tuy nhiên, vì Javascript cho phép chúng ta vừa khai báo vừa gián giá trị cho biến. Vậy nên đây là cách mà mọi người thường làm :

```javascript
var a = 100;
```

Vì Javascript đưa khai báo biến lên trên cùng nên ta sẽ có hiện tượng như thế này :

```javascript
console.log(hoist); // Output: undefined
var hoist = 'The variable has been hoisted.';
```

Có thể bạn dự đoán kết quả là `ReferenceError: hoist is not defined` vì chúng ta đang cố truy cập đến một biến mà chưa được khai báo trước đó nhưng thay vào đó là `undefined` .

Vậy điều gì đã xảy ra?

Javascript đã nâng khai báo biến lên trên cùng. Đây là những gì xảy ra bên trong đoạn code bên trên khi Javascript chạy :

```javascript
var hoist;

console.log(hoist); // Output: undefined
hoist = 'The variable has been hoisted.';
```

Nếu bạn thắc mắc đưa khai báo lên trên cùng, vậy trên cùng là ở đâu. Trên cùng là vị trí cao nhất của **scope** hiện tại. Ví dụ :

```javascript
function hoist() {
  console.log(message);
  var message='Hoisting is all the rage!'
}

hoist();
```

Thì sẽ tương đương như thế này :

```javascript
function hoist() {
  var message;
  console.log(message);
  message='Hoisting is all the rage!'
}

hoist(); // Ouput: undefined
```

Khai báo `var message` sẽ nằm trên cùng trong **scope** function `hoist()` .

Để tránh sự tự do đến khó hiểu này, hãy khai báo và khởi tạo biến trước khi sử dụng

```javascript
function hoist() {
  var message='Hoisting is all the rage!'
  return (message);
}

hoist(); // Ouput: Hoisting is all the rage!
```

Tất cả các biến không được khai báo sẽ không tồn tại. Nhưng khi gán giá trị cho một biến không được khai báo, nó sẽ trở thành biến **global**.

```javascript
function hoist() {
  a = 20;
  var b = 100;
}
hoist();
console.log(a); 
/* 
Can access "a" as global variable outside function hoist()
Output: 20
*/
console.log(b); 
/*
Because it have been declared, it will push declare on top scope function.
So variable "b" was limited inside scope, we can't access from outside. 
Output: ReferenceError: b is not defined
*/
```

### Strict mode

Một chức năng khá hay của ES5 là strict mode \( tức là chế độ nghiêm ngặt \). Chế độ này yêu cầu chúng ta cần phải khai báo biến trước khi sử dụng.

Để bật tính năng này, chúng ta chỉ cần thêm một đoạn string vào file hoặc function

```javascript
"use strict";

hoist = "Hoisted";
console.log(hoist); // Output: ReferenceError: hoist is not defined
```

Chúng ta có thể thấy nếu quên việc khai báo biến `hoist` thì Javascript sẽ báo lỗi ngay `Reference error` . Nếu không dùng `use strict` thì code vẫn chạy và bạn sẽ log ra là `Hoisted` như bình thường.

Lưu ý là `use strict` mode hoạt động khác nhau ở các trình duyệt khác nhau. Hãy cẩn thận khi dùng `use strict` nhé.

### Hàm Hoisting

Hàm trong Javascript có thể được chia ra làm 2 loại là

1. Khai báo hàm \(Function Declaration\)
2. Biểu thức hàm \(Function Expression\)

#### Function Declaration

Với cách này thì function được **Hoisting**. Và Javascript cho phép chúng ta gọi một hàm trước khi hàm đó được khai báo.

```javascript
hoisted(); // Output: "This function has been hoisted."

function hoisted() {
  console.log('This function has been hoisted.');
};
```

#### Function Expression

Tuy nhiên với cách khai báo function kiểu này thì sẽ không được hoisting

```javascript
expression(); //Output: "TypeError: expression is not a function

var expression = function() {
  console.log('Will this work?');
};
```

**Giải thích:** Biến `var expression` vẫn được **hoisting** và được đẩy lên trên cùng của scope nhưng chỉ là khai báo mà thôi, nó không được gán cho hàm! Vì thế nó sẽ ném ra lỗi `TypeError` .

### Thứ tự Hoisting

Đây là điều quan trọng khi khai báo các hàm và biến trong Javascript. Thứ tự **Hoisting** được sắp xếp giảm dần như dưới đây.

1. Khai báo biến
2. Khai báo hàm
3. Gán biến

Ví dụ `hàm double` được **Hoisting** lên cùng, ngay sau đó là phép gán `biến double`. Vì thế `biến double` sẽ đè lên `hàm double` và cuối cùng `type` của `double` là `number`.

```javascript
var double = 22;

function double(num) {
  return (num*2);
}

console.log(typeof double); // Output: number
```

Ví dụ dưới đây cho thấy khai báo hàm được **Hoisting** dưới khai báo biến. Vì thế `type` của `double` sẽ là `number`

```javascript
var double;

function double(num) {
  return (num*2);
}

console.log(typeof double); // Output: function
```

 Dù cho bạn có đảo ngược vị trí khai báo trong code đi chăng nữa thì trình thông dịch Javascript vẫn cho ra `double` là một `function.`

