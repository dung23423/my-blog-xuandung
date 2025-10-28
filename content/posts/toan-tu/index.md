---
title: "Toán tử và so sánh chuỗi"
date: 2025-10-15
draft: false
categories: ["Lập trình"]
tags: ["javascript", "Lập Trình Web"]
showTableOfContents: true
feature: "/posts/toan-tu/images/toantu.png"
---

## 1. Toán Tử

![Ảnh 1](images/anh-nen_toantu.jpg)

### 1.1 Toán tử là gì?

### Toán tử trong JavaScript

Toán tử là ký hiệu hoặc từ khóa thực hiện hành động trên toán hạng (giá trị hoặc biến).
Ví dụ: `=` là toán tử gán, typeof kiểm tra kiểu.

#### Phân loại:

**Theo số lượng toán hạng:** Đơn ngôi (`!a`), Nhị phân (`a + b`), Ba ngôi (`condition ? a : b`).
**Theo vị trí:** Tiền tố (`++a`), Hậu tố (`a++`), Trung tố (`a + b`).
**Theo chức năng:** Gán (`=`), Số học (`+`, `-`), Logic (`&&`, `||`), So sánh (`==`, `===`), Điều kiện (`?:`).

Lưu ý: Ký hiệu như + có thể là phép cộng số hoặc nối chuỗi tùy ngữ cảnh.

### 1.2 Toán tử gán

Hãy bắt đầu với các toán tử gán. Trong nhóm này, có các toán tử cho phép
gán giá trị cho các biến và hằng số. Toán tử gán cơ bản là dấu bằng `=`,
mà chúng ta đã thấy nhiều lần trong các ví dụ. Toán tử này gán giá trị
của toán hạng bên phải cho toán hạng bên trái.

```javascript
const name = "Alice";

console.log(name); // -> Alice
```

Nếu có nhiều toán tử gán xuất hiện trong một chuỗi, thứ tự từ phải sang
trái sẽ được áp dụng. Do đó, chuỗi:

```javascript
let year = 2050;

let newYear = year = 2051;

có nghĩa giống như:

let year = 2050;

year = 2051;

let newYear = year;
```

Ngoài toán tử gán cơ bản, còn có các toán tử gán liên quan đến toán tử
số học, logic và chuỗi. Chúng ta sẽ quay lại với chúng khi thảo luận về
các loại toán tử khác.

### 1.3 Toán tử số học

Toán tử số học dùng để thực hiện các phép tính trên giá trị số hoặc biến.  
Ngoại trừ phép **cộng (`+`)**, tất cả các toán tử khác sẽ **tự chuyển đổi toán hạng sang kiểu Number** trước khi tính.

Riêng `+` sẽ:

- Nếu **một trong hai toán hạng là chuỗi (`String`)** → nối chuỗi.
- Nếu **đều là số** → thực hiện phép cộng số học.

> **Thứ tự ưu tiên** trong JavaScript giống toán học, nên dùng **ngoặc đơn** để đảm bảo rõ ràng.

```javascript
console.log(2 + 2 * 2); // -> 6

console.log(2 + 2 * 2); // -> 6

console.log((2 + 2) * 2); // -> 8
```

Các toán tử số học nhị phân cơ bản là phép cộng `+`, phép trừ `-`, phép
nhân `*`, phép chia `/`, phần dư của phép chia `%` và mũ `**`. Hoạt động
của chúng tương tự như những gì chúng ta biết từ toán học và cách dễ
nhất để theo dõi chúng là sử dụng một ví dụ:

```javascript
const x = 5;

const y = 2;

console.log("addition: ", x + y); // -> 7

console.log("subtraction: ", x - y); // -> 3

console.log("multiplication: ", x * y); // -> 10

console.log("division: ", x / y); // -> 2.5

console.log("division remainder :", x % y); // -> 1

console.log("exponent: ", x ** y); // -> 25
```

### 1.4 Toán tử số học một ngôi

Ngoài ra còn có một số toán tử số học đơn ngôi (hoạt động trên một toán
hạng duy nhất). Trong số đó có phép cộng `+` và trừ `-` người vận hành.

Cả hai toán tử đều chuyển đổi toán hạng thành kiểu Số, trong khi toán tử
trừ còn phủ định nó.

```javascript
let str = "123";

let n1 = +str;

let n2 = -str;

let n3 = -n2;

let n4 = +"abcd";

console.log(`${str} : ${typeof str}`); // -> 123 : string

console.log(`\${n1} : ${typeof n1}`); // -> 123 : number

console.log(`${n2} : ${typeof n2}`); // -> -123 : number

console.log(`${n3} : ${typeof n3}`); // -> 123 : number

console.log(`${n4} : ${typeof n4}`); // -> NaN : number
```

### 1.5 Toán tử tăng và giảm một ngôi

Trong số các toán tử số học, chúng ta cũng có thể sử dụng phép **gia số
đơn** `++` và giảm `--` Toán tử, ở cả phiên bản tiền tố và hậu tố. Chúng
cho phép chúng ta tăng (tăng) hoặc giảm (giảm) giá trị của toán hạng đi

Các toán tử này ở dạng hậu tố (tức là toán tử nằm bên phải toán hạng)
thực hiện phép toán bằng cách thay đổi giá trị của biến, nhưng trả về
giá trị trước khi thay đổi. Phiên bản tiền tố của toán tử (tức là toán
tử được đặt trước toán hạng) thực hiện phép toán và trả về giá trị mới.

**Bảng điều khiển phần cuối** đồng bộ hóa

Điều này xảy ra vì dòng mã:

```javascript
console.log(n1++);
```

được hiểu là:

```javascript
console.log(n1);

n1 = n1 + 1;
```

trong khi dòng:

```javascript
console.log(++n1);
```

có nghĩa giống như:

```javascript
n1 = n1 + 1;

console.log(n1);
```

Hãy nhớ rằng kiểu Số là kiểu **dấu phẩy động**, nghĩa là kết quả của một số
phép toán có thể không chính xác.

```javascript
console.log(0.2 + 0.1); // 0.30000000000000004

console.log(0.2 * 0.1); // 0.020000000000000004

console.log(0.3 / 0.1); // 2.9999999999999996
```

Đây là những hiện vật của phép tính số học dấu phẩy động. Số này sẽ
chính xác với các số nguyên lên đến 252, nhưng phân số có thể không
chính xác bằng, vì nhiều phân số không thể biểu diễn trực tiếp ở định
dạng nhị phân. Chúng ta sẽ thảo luận về cách giảm thiểu vấn đề này ngay
khi giới thiệu các toán tử so sánh.

### 1.6 Toán tử gán hợp chất

**Các toán tử số học nhị phân** có thể được kết hợp với **toán tử
gán** , tạo ra phép gán cộng `+=`, phép trừ `-=`, phép nhân `*=` , phép chia `/=`,
lấy dư `%=` và mũ `**=` .

Mỗi toán tử này lấy một giá trị từ biến cần gán (toán hạng bên trái) và
sửa đổi nó bằng cách thực hiện phép toán số học sử dụng giá trị toán
hạng bên phải. Giá trị mới được gán cho toán hạng bên trái. Ví dụ, đoạn
mã dưới đây:

```javascript
x += 100;
```

có thể được viết dưới dạng:

```javascript
x = x + 100;
```

Do đó, sẽ không khó để hiểu cách hoạt động của ví dụ sau:

```javascript
let x = 10;

x += 2;

console.log(x); // -> 12

x -= 4;

console.log(x); // -> 8

x *= 3;

console.log(x); // -> 24

x = 6;

console.log(x); // -> 4

x **= 3;

console.log(x); // -> 64

x %= 10;

console.log(x); // -> 4
```

### 1.7 Toán tử logic

Các toán tử logic hoạt động với các giá trị kiểu Boolean (`true`
hoặc `false` ). Hiện tại, chúng ta có thể giả định rằng chúng hoạt động
trên các toán hạng thuộc kiểu này và chỉ trả về các giá trị thuộc kiểu
này. JavaScript cung cấp cho chúng ta ba toán tử như vậy:

- một phép nối, tức là phép logic AND (`&&` biểu tượng)

- một giải pháp thay thế, tức là logic OR (ký hiệu `||`)

- một phủ định, tức là logic NOT (ký hiệu `!`)

Ý nghĩa của chúng cũng giống như trong logic toán học, và nếu bạn không
chắc chúng hoạt động như thế nào, cách dễ nhất là giải thích chúng dựa
trên các câu logic.

Hãy bắt đầu với phép nối. Đây là một toán tử nhị phân trả về giá trị
true nếu cả hai toán hạng đều đúng. Sử dụng các câu logic, chúng ta có
thể hình dung một câu bao gồm hai mệnh đề đơn giản được kết nối bằng
toán tử AND, ví dụ:

*London là một thành phố* VÀ _London nằm ở Vương quốc Anh._

Cả hai câu đều đúng trong trường hợp này, và sau khi kết hợp chúng với
AND, một câu cũng đúng sẽ được tạo ra. Nếu bất kỳ câu nào trong số này
sai (hoặc cả hai đều sai), thì toàn bộ câu cũng sẽ sai, ví dụ:

*London là một thành phố* VÀ _London nằm ở Iceland._

Trong mã JavaScript, nó trông đơn giản như thế này:

```javascript
console.log(true && true); // -> true

console.log(true && false); // -> false

console.log(false && true); // -> false

console.log(false && false); // -> false
```

Trong trường hợp một toán tử thay thế cũng là toán tử nhị phân, chỉ cần
một trong hai toán hạng đúng là đủ để toán tử trả về giá trị đúng. Quay
lại ví dụ về các câu logic, hãy sử dụng một câu được tạo thành từ hai
câu lệnh được kết nối bằng toán tử OR, ví dụ:

*London là một thành phố* HOẶC _London nằm ở Iceland._

Câu này có thể trông không được hùng hồn hay hợp lý lắm, nhưng xét về
mặt logic thì nó khá chính xác. Chỉ cần một trong hai mệnh đề đúng là
đủ, thì cả câu cũng đúng. Nếu cả hai mệnh đề đều sai, thì câu cũng sẽ
sai, ví dụ:

*London là một ngôi làng* HOẶC _London nằm ở Iceland._

Hãy kiểm tra xem nó trông như thế nào trong JavaScript:

```javascript
console.log(true || true); // -> true

console.log(true || false); // -> true

console.log(false || true); // -> true

console.log(false || false); // -> false
```

Toán tử phủ định là một toán tử đơn, và nó thay đổi giá trị logic của
toán hạng thành giá trị đối lập của nó, tức là từ false thành true, và
từ true thành false. Sử dụng các câu logic, ta có thể biểu diễn nó với
phép phủ định NOT. Hãy lấy ví dụ một câu đơn giản đúng:

_London là một thành phố._

Bằng cách thêm phủ định vào nó, chúng ta thay đổi giá trị của nó thành
sai:

*London* KHÔNG phải _là một thành phố._

Tương tự như vậy, nó sẽ hoạt động theo chiều ngược lại, biến một câu sai
thành một câu đúng. Trong đoạn mã, nó sẽ trông thậm chí còn đơn giản
hơn:

```javascript
console.log(!true); // -> false

console.log(!false); // -> true
```

Tất nhiên, chúng ta có thể kết nối bao nhiêu toán tử này tùy thích, tạo
ra những "câu" phức tạp hơn. Cũng như trường hợp các toán tử số học,
trình tự các hành động được xác định ở đây. Ưu tiên cao nhất là phủ
định `!`, sau đó kết hợp `&&` và cuối cùng là giải pháp thay thế `||`. Tất
nhiên thứ tự ưu tiên có thể được thay đổi bằng cách sử dụng dấu ngoặc
đơn.

```javascript
const a = false;

const b = true;

const c = false;

const d = true;

console.log((a && b && c) || d); // -> true

console.log(a && b && (c || d)); // -> false
```

### 1.8 Toán tử logic và giá trị không phải Boolean

Miễn là toán hạng thuộc kiểu Boolean, chúng ta có thể dễ dàng thấy kết
quả trả về. Tuy nhiên, các toán tử này cũng có thể được sử dụng với các
kiểu dữ liệu khác nhau. Trường hợp dễ nhất là logic **NOT**. Đầu tiên, toán
hạng được chuyển đổi tạm thời thành giá trị Boolean (theo các quy tắc đã
giải thích trong chương trước) và chỉ sau đó mới được xử lý bằng thao
tác toán tử thích hợp (tức là giá trị **true** được chuyển đổi thành **false**
và ngược lại). Do đó, toán tử _NOT_ sẽ luôn trả về `false` hoặc `true`. Thông
thường, phép phủ định kép được sử dụng để chuyển đổi bất kỳ kiểu dữ liệu
nào sang Boolean.

```javascript
let nr = 0;

let year = 1970;

let name = "Alice";

let empty = "";

console.log(!nr); // -> true

console.log(!year); // -> false

console.log(!name); // -> false

console.log(!empty); // -> true

console.log(!!nr); // -> false

console.log(!!name); // -> true
```

Điều này hơi khác với các toán tử logic nhị phân (ví dụ AND và OR).
Chúng không trả về giá trị Boolean. Thực tế, chúng trả về toán hạng thứ
nhất hoặc thứ hai của toán tử đó. Toán tử AND sẽ trả về toán hạng thứ
nhất nếu nó được đánh giá là false, và toán hạng thứ hai nếu ngược lại.
Toán tử OR sẽ trả về toán hạng thứ nhất nếu nó được đánh giá là true, và
toán hạng thứ hai nếu ngược lại. Đánh giá đơn giản là một nỗ lực để
chuyển đổi một toán hạng thành giá trị Boolean (một lần nữa, theo các
quy tắc đã học ở chương trước).

```javascript
console.log(true && 1991); // -> 1991

console.log(false && 1991); // -> false

console.log(2 && 5); // -> 5

console.log(0 && 5); // -> 0

console.log("Alice" && "Bob"); // -> Bob

console.log("" && "Bob"); // -> empty string

console.log(true || 1991); // -> true

console.log(false || 1991); // -> 1991

console.log(2 || 5); // -> 2

console.log(0 || 5); // -> 5

console.log("Alice" || "Bob"); // -> Alice

console.log("" || "Bob"); // -> Bob
```

Cả hai toán tử đều sử dụng **đánh giá ngắn mạch** .

Vì vậy, nếu toán hạng đầu tiên của `AND` là `false` , nó sẽ được trả lại và
không có kiểm tra nào khác được thực hiện.

Ngược lại, nếu toán hạng đầu tiên của `OR` là `true`, nó sẽ được trả
về và không có kiểm tra nào khác được thực hiện. Điều này giúp tăng tốc
độ thực thi mã, nhưng có một tác dụng phụ có thể thấy trong ví dụ này:

```javascript
let x = 0;

let y = 0;

console.log(x++ && y++); // -> 0

console.log(x); // -> 1

console.log(y); // -> y == 0
```

Hướng dẫn `y++` sẽ không bao giờ được thực hiện, điều này có thể gây nhầm
lẫn.

Toán tử logic thường được sử dụng cùng với **toán tử điều kiện** và đặc
biệt hữu ích trong **các lệnh điều kiện** (ra quyết định) và
trong **vòng lặp** (điều kiện kết thúc vòng lặp). Bạn có thể tìm hiểu về
ứng dụng thực tế của chúng trong các phần về lệnh điều kiện và vòng lặp
vừa được đề cập.

### 1.9 Toán tử gán hợp chất

Giống như các toán tử số học, **các toán tử logic nhị phân** có thể được
sử dụng kết hợp với toán tử gán, tạo ra phép gán logic AND `&& =` và một phép
gán OR logic `|| =`.

Có lẽ bạn sẽ dễ dàng hình dung cách chúng hoạt động. Trong trường hợp
toán tử AND, chúng ta có thể kiểm tra bằng ví dụ sau:

```javascript
let a = true;

console.log(a); // -> true

a &&= false;

console.log(a); // -> false
```

Hướng dẫn `a &&= false` có nghĩa chính xác giống như `a = a && false`.

Chúng ta có thể chuẩn bị một ví dụ tương tự cho phép toán OR:

```javascript
let b = false;

console.log(b); // -> false

b ||= true;

console.log(b); // -> true
```

Lần này, hoạt động `b ||= true` được hiểu là `b = b || true`.

## 2. Chuỗi, so sánh chuỗi và các phép toán khác

### 2.1 Toán tử chuỗi

Toán tử duy nhất trong nhóm này là **phép nối** `+` Toán tử này sẽ chuyển
đổi mọi thứ thành **String** nếu bất kỳ toán hạng nào có kiểu Chuỗi. Cuối
cùng, nó sẽ tạo ra một chuỗi ký tự mới, nối toán hạng bên phải vào cuối
toán hạng bên trái.

```javascript
let greetings = "Hi";

console.log(greetings + " " + "Alice"); // -> Hi Alice*
let sentence = "Happy New Year ";

let newSentence = sentence + 10191;

console.log(newSentence); // -> Happy New Year 10191*
console.log(typeof newSentence); // -> string*
```

#### Toán tử gán hợp chất

Bạn có thể đoán rằng toán tử này cũng có thể được sử dụng kết hợp với
toán tử thay thế. Thao tác của nó rất trực quan nên chúng ta sẽ dừng lại
ở một ví dụ đơn giản:

```javascript
let sentence = "Happy New ";

sentence += "Year ";

sentence += 10191;

console.log(sentence); // -> Happy New Year 10191*
```

### 2.2 Toán tử so sánh

Toán tử so sánh được sử dụng để kiểm tra sự bằng nhau hay bất bằng nhau
của các giá trị. Tất cả các toán tử so sánh đều là nhị phân và đều trả
về một giá trị logic biểu diễn kết quả của phép so sánh. `true`
VẬY hoặc `false`.

Giống như các toán tử khác, JavaScript sẽ cố gắng chuyển đổi các giá trị
đang được so sánh nếu chúng có kiểu dữ liệu khác nhau. Việc kiểm tra
tính bằng nhau, hay giá trị nào lớn hơn, bằng cách sử dụng biểu diễn số,
là hợp lý, và trong hầu hết các trường hợp, JavaScript sẽ chuyển đổi
kiểu dữ liệu thành Số trước khi so sánh. Có hai ngoại lệ cho điều này:
chuỗi và **toán tử đồng nhất (bằng nhau nghiêm ngặt)** . Chuỗi được so
sánh char qua char (chính xác từng ký tự Unicode bằng cách sử dụng giá trị
của chúng).

Để kiểm tra xem các toán hạng có bằng nhau hay không, chúng ta có thể sử
dụng toán tử **đồng nhất (bằng nhau nghiêm ngặt)** `===` hoặc toán
tử **bằng** `==`.

Cách đầu tiên hạn chế hơn và để trả về giá trị true, các toán hạng phải
giống hệt nhau (tức là chúng phải bằng nhau và cùng kiểu).

```javascript
console.log(10 === 5); // -> false
console.log(10 === 10); // -> true
console.log(10 === 10n); // -> false
console.log(10 === "10"); // -> false
console.log("10" === "10"); // -> true
console.log("Alice" === "Bob"); // -> false
console.log(0 === false); //-> false
console.log(undefined === false); // -> false
```

Toán tử bằng yêu cầu chúng chỉ bằng nhau và kiểu dữ liệu của chúng không
được so sánh. Vì vậy, nếu các toán hạng có kiểu dữ liệu khác nhau, trình
thông dịch sẽ cố gắng chuyển đổi chúng thành số, ví dụ: **_False_** sẽ chuyển đổi
thành **_0_**, **_true_** ĐẾN **_1_**, không xác định ĐẾN **_NaN_**,vô giá
trị ĐẾN **_0_**, **_10n_** ĐẾN **_10_** Và **_"123"_** ĐẾN **_123_**, vân vân.

Lưu ý rằng nếu bất kỳ toán hạng nào có **_NaN_** giá trị (hoặc đã được chuyển
đổi thành **_NaN_**, ví dụ với **_undefined_**), toán tử bằng sẽ trả về **_False_**.

```javascript
console.log(10 == 5); // -> false
console.log(10 == 10); // -> true
console.log(10 == 10n); // -> true
console.log(10 == "10"); // -> true
console.log("10" == "10"); // -> true
console.log("Alice" == "Bob"); // -> false
console.log(0 == false); // -> true
console.log(undefined == false); // -> false
console.log(NaN == NaN); // -> false
```

** Hãy nhớ! Sử dụng toán tử danh tính trừ khi bạn cố ý cho phép so sánh
tích cực giữa các kiểu khác nhau. **

Ngoài ra còn có các toán tử bổ sung cho các toán tử vừa trình bày
**_--_** toán tử **không đồng nhất** **_!==_** và toán tử **bất đẳng thức** **_!=_**. Sự trở
lại đầu tiên **true** nếu các toán hạng không giống hệt nhau, nói cách
khác, chúng bằng nhau nhưng có kiểu khác nhau hoặc chúng chỉ đơn giản là
khác nhau. Trả về thứ hai **_true_** nếu các toán hạng khác nhau.

```javascript
console.log(10 !== 5); // -> true
console.log(10 !== 10); // -> false
console.log(10 !== 10n); // -> true
console.log(10 !== "10"); // -> true
console.log("10" !== "10"); // -> false
console.log("Alice" !== "Bob"); // -> true
console.log(0 !== false); // -> true
console.log(undefined !== false); // -> true

console.log(10 != 5); // -> true
console.log(10 != 10); // -> false
console.log(10 != 10n); // -> false
console.log(10 != "10"); // -> false
console.log("10" != "10"); // -> false
console.log("Alice" != "Bob"); // -> true
console.log(0 != false); // -> false
console.log(undefined != false); // -> true
console.log(NaN != NaN); // -> true
```

Chúng tôi cũng có các toán tử cho phép chúng tôi kiểm tra xem một trong
các toán hạng có lớn hơn **_>_**, nhỏ hơn **_<_**, lớn hơn hoặc bằng **_>=_** và nhỏ hơn
hoặc bằng **_<=_**. Các toán tử này có tác dụng với mọi loại toán hạng, nhưng
chỉ nên sử dụng chúng trên các số hoặc giá trị có thể chuyển đổi chính
xác thành số.

```javascript
console.log(10 > 100); // -> false
console.log(101 > 100); // -> true
console.log(101 > "100"); // -> true
console.log(101 < 100); // -> false
console.log(100n < 102); // -> true
console.log("10" < 20n); // -> true
console.log(101 <= 100); // -> false
console.log(10 >= 10n); // -> true
console.log("10" <= 20); // -> true
```

Bạn cũng có thể sử dụng chúng để so sánh các chuỗi không biểu diễn số,
nhưng thuật toán so sánh này khá phức tạp, và bản thân phép so sánh này
không thực sự hữu ích. Để đơn giản hóa, các ký tự riêng lẻ của cả hai
chuỗi được kiểm tra ở cùng một vị trí. Giả định rằng giá trị của các ký
tự riêng lẻ tương ứng với vị trí của chúng trong bảng chữ cái (chữ b có
giá trị cao hơn chữ a). Chữ in hoa có giá trị thấp hơn chữ thường, và
chữ số thậm chí còn có giá trị thấp hơn.

```javascript
console.log("b" > "a"); // -> true
console.log("a" > "B"); // -> true
console.log("B" > "A"); // -> true
console.log("A" > "4"); // -> true
console.log("4" > "1"); // -> true
console.log("ab1" < "ab4"); // -> true
console.log("ab4" < "abA"); // -> true
console.log("abB" < "aba"); // -> true
console.log("aba" < "abb"); // -> true
console.log("ab" < "ab4"); // -> true
```

Lưu ý: biểu tượng `=>` tồn tại trong JavaScript, nhưng không phải là toán
tử `--` chúng ta sử dụng nó trong quá trình xây dựng các hàm mũi tên.

### 2.3 Các nhà điều hành khác

Danh sách các toán tử trong JavaScript dài hơn nhiều, nhưng nhiều toán
tử trong số đó sẽ không thực sự hữu ích ở giai đoạn học này, chẳng hạn
như các toán tử bitwise, hoạt động trên các bit đơn lẻ của toán hạng.
Tuy nhiên, cũng đáng đề cập đến một vài toán tử khác, một số đã xuất
hiện trong các ví dụ trước.

#### 2.3.1 typeof

Chúng tôi đã giới thiệuloại củaToán tử này khi thảo luận về kiểu dữ
liệu. Đây là toán tử một ngôi, kiểm tra kiểu toán hạng (có thể là biến
hoặc số nguyên). Toán tử trả về một chuỗi có tên kiểu, chẳng hạn như
"boolean" hoặc "number".

Nếu bạn muốn làm mới kiến ​​thức về toán tử này, hãy quay lại phần về
kiểu dữ liệu.

```javascript
let year = 10191;

console.log(*typeof* year); // -> number\*

console.log(*typeof* false); // -> boolean\*

```

#### 2.3.2 instanceof

Các trường hợp củaToán tử này xuất hiện khi thảo luận về mảng. Đây là
toán tử nhị phân kiểm tra xem một đối tượng (toán hạng trái) có thuộc
kiểu nào đó (toán hạng phải) hay không. Tùy thuộc vào kết quả kiểm tra,
nó trả về `true` hoặc `false`.

Trong khóa học này, tính hữu ích của toán tử này chỉ giới hạn ở việc
kiểm tra xem một biến có chứa mảng hay không.

```javascript
let names = ["Patti", "Bob"];

let name = names[0];

console.log(names instanceof Array); // -> true
console.log(name instanceof Array); // -> false
```

#### 2.3.3 delete

Đơn vị `delete` Toán tử này được giới thiệu khi thảo luận về các đối tượng.
Nó cho phép bạn xóa một trường đã chọn của đối tượng có tên được chỉ
định bằng một toán hạng.

```javascript
let user = {
  name: "Alice",

  age: 38,
};

console.log(user.age); // -> 38
delete user.age;

console.log(user.age); // -> undefined
```

#### 2.3.4 toán tử 3 ngôi

Toán tử cuối cùng được thảo luận khá bất thường, vì đây là toán tử duy
nhất sử dụng ba toán hạng. Đây là một toán tử điều kiện. Dựa trên giá
trị của toán hạng thứ nhất (true hoặc false), giá trị của toán hạng thứ
hai hoặc thứ ba tương ứng sẽ được trả về. Toán tử này thường được sử
dụng để đặt một trong hai giá trị vào biến tùy thuộc vào một điều kiện
nhất định. Chúng ta sẽ quay lại toán tử này khi thảo luận về toán tử if
có điều kiện, nhưng ở đây chúng tôi chỉ cung cấp một ví dụ đơn giản về
cách sử dụng của nó. Ba toán hạng được phân tách với nhau bằng `?` (toán
hạng thứ nhất cách xa toán hạng thứ hai) và `:` (toán hạng thứ hai cách xa
toán hạng thứ ba).

```javascript
console.log(true ? "Alice" : "Bob"); // -> Alice
console.log(false ? "Alice" : "Bob"); // -> Bob
```

Mỗi toán hạng này có thể là một biểu thức cần được tính toán. Trong ví
dụ sau, toán hạng đầu tiên là phép so sánh hai số bằng toán tử so sánh.
Kết quả của phép so sánh sẽ là false, và toán tử điều kiện (ba ngôi) sẽ
sử dụng kết quả này. Ở đây, chúng ta sẽ đi sâu vào một vấn đề quan trọng
về thứ tự thực hiện và thứ tự ưu tiên của toán tử. Chúng ta sẽ tìm hiểu
thêm về vấn đề này sau.

```javascript
let name = 1 > 2 ? "Alice" : "Bob";

console.log(name); // -> Bob
```
