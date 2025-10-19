---
title: "JavaScript Closures: Từ 'Hả?' đến 'À ra thế!' - Bí mật về phạm vi biến"
date: 2025-10-14
draft: false
tags: ["JavaScript", "Closures", "Scope", "Nâng cao", "Cơ bản"]
categories: ["JavaScript", "Cơ Bản"]
description: "Closure là gì? Tại sao quan trọng? Từ bug counter không hoạt động đến hiểu sâu về phạm vi biến, ngữ cảnh thực thi và các mẫu thiết kế với closure."
summary: "Tôi code JavaScript 6 tháng mới biết closures tồn tại. Thêm 3 tháng nữa mới thực sự hiểu nó. Đây là con đường ngắn hơn để bạn hiểu."
feature: "https://images.unsplash.com/photo-1555952494-efd681c7e3f9?w=800&h=600&fit=crop"
showTableOfContents: true
---

Tháng thứ 6 học JavaScript, tôi tự tin là đã "biết" ngôn ngữ này. Biến, hàm, đối tượng, mảng - dễ ợt.

Cho đến khi tôi gặp đoạn code này trong buổi phỏng vấn:
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
}

// Người phỏng vấn: "Kết quả là gì?"
// Tôi: "0, 1, 2 chứ còn gì!"
// Thực tế: 3, 3, 3
```

*"Hả? Sao lại thế?"*

Người phỏng vấn mỉm cười: *"Bạn có biết closure không?"*

*"Clo... gì cơ?"*

Trượt phỏng vấn. Nhưng tò mò. Closure là gì mà quan trọng thế?

3 tháng nghiên cứu, thực hành, và vô số lần "à ra thế!" sau, tôi cuối cùng cũng hiểu. Và nhận ra: **Closure là một trong những tính năng quan trọng nhất của JavaScript.**

![JavaScript Closures - Bối rối](https://images.unsplash.com/photo-1555952494-efd681c7e3f9?w=1200&h=600&fit=crop)

Đây là lộ trình để bạn hiểu closure - không cần mất 3 tháng như tôi.

---

## 🎯 Trước khi hiểu Closure: Phạm Vi (Scope)

### **Phạm vi là gì?**

Phạm vi = Nơi một biến có thể được truy cập.

Hãy tưởng tượng như tầng nhà:
```javascript
// Tầng trệt (toàn cục)
const bienToanCuc = "Tôi ở tầng trệt";

function hamNgoai() {
    // Tầng 1
    const bienTang1 = "Tôi ở tầng 1";
    
    function hamTrong() {
        // Tầng 2
        const bienTang2 = "Tôi ở tầng 2";
        
        console.log(bienToanCuc);  // ✅ Nhìn xuống tầng trệt được
        console.log(bienTang1);    // ✅ Nhìn xuống tầng 1 được
        console.log(bienTang2);    // ✅ Trong phòng mình
    }
    
    console.log(bienToanCuc);  // ✅ Nhìn xuống được
    console.log(bienTang1);    // ✅ Trong phòng mình
    console.log(bienTang2);    // ❌ Không nhìn lên trên được!
}

console.log(bienToanCuc);  // ✅ Trong phòng mình
console.log(bienTang1);    // ❌ Không lên được tầng 1
console.log(bienTang2);    // ❌ Không lên được tầng 2
```

**Quy tắc vàng:** Bên trong có thể "nhìn ra ngoài", nhưng bên ngoài không "nhìn vào trong".

![Chuỗi phạm vi JavaScript](https://images.unsplash.com/photo-1509228468518-180dd4864904?w=1200&h=600&fit=crop)

*Sơ đồ: Chuỗi phạm vi - hàm bên trong có thể truy cập biến bên ngoài*

### **var vs let vs const - Khác nhau về phạm vi**
```javascript
// var - Phạm vi hàm
function kiemTraVar() {
    if (true) {
        var x = 10;
    }
    console.log(x); // ✅ 10 - var "thoát ra" khỏi khối if
}

// let/const - Phạm vi khối {}
function kiemTraLet() {
    if (true) {
        let y = 10;
        const z = 20;
    }
    console.log(y); // ❌ Lỗi: y không tồn tại
    console.log(z); // ❌ Lỗi: z không tồn tại
}

// Vấn đề kinh điển của var
for (var i = 0; i < 3; i++) {
    // var i có phạm vi cả hàm, không chỉ trong vòng lặp!
}
console.log(i); // 3 - i "rò rỉ" ra ngoài!

// let giải quyết vấn đề này
for (let j = 0; j < 3; j++) {
    // let j chỉ tồn tại trong vòng lặp
}
console.log(j); // ❌ Lỗi - tốt!
```

**Bài học:** Luôn dùng `let`/`const`, không bao giờ dùng `var`.

---

## 💡 Closure: Định nghĩa

### **Định nghĩa kỹ thuật:**

> "Closure là một hàm có thể truy cập biến ở phạm vi bên ngoài (bao quanh nó), ngay cả khi hàm bên ngoài đã kết thúc."

**Nói người thường hiểu:**

> "Một hàm 'nhớ' môi trường nơi nó được sinh ra, kể cả khi môi trường đó không còn tồn tại nữa."

**Giống như:** Con mang gen của cha mẹ, dù cha mẹ không còn ở bên.

### **Ví dụ đầu tiên:**
```javascript
function taoLoiChao(loiChao) {
    // loiChao = biến bên ngoài
    
    return function(ten) {
        // Hàm này "nhớ" loiChao
        console.log(`${loiChao}, ${ten}!`);
    };
}

const chaoHello = taoLoiChao("Hello");
const chaoXinChao = taoLoiChao("Xin chào");

chaoHello("Minh");      // "Hello, Minh!"
chaoXinChao("Hương");   // "Xin chào, Hương!"

// Hàm taoLoiChao đã chạy xong, nhưng loiChao vẫn còn!
```

**Chuyện gì đã xảy ra?**

1. `taoLoiChao("Hello")` chạy
2. Tạo hàm bên trong nhớ `loiChao`
3. Trả về hàm đó
4. `taoLoiChao` kết thúc, bình thường `loiChao` nên "chết"
5. **Nhưng không!** Hàm bên trong đã "đóng gói" `loiChao` lại
6. Sau đó, `chaoHello("Minh")` vẫn dùng được `loiChao = "Hello"`

**Đây chính là closure!**

![Sơ đồ Closure](https://images.unsplash.com/photo-1607799279861-4dd421887fb3?w=1200&h=600&fit=crop)

*Minh họa: Hàm "mang theo" môi trường của nó*

---

## 🔍 Closure Trong Thực Tế

### **Ví dụ 1: Bộ đếm (Kinh điển)**
```javascript
// ❌ Tệ: Biến toàn cục
let bodem = 0;

function tang() {
    bodem++;
    return bodem;
}

// Vấn đề: Ai cũng có thể sửa bodem
bodem = 100; // Oái!
```
```javascript
// ✅ Tốt: Closure bảo vệ biến riêng tư
function taoBoDem() {
    let dem = 0; // Biến riêng tư
    
    return {
        tang: function() {
            dem++;
            return dem;
        },
        giam: function() {
            dem--;
            return dem;
        },
        layGiaTri: function() {
            return dem;
        }
    };
}

const bodem = taoBoDem();

console.log(bodem.tang());     // 1
console.log(bodem.tang());     // 2
console.log(bodem.giam());     // 1
console.log(bodem.layGiaTri());// 1

// dem thực sự là riêng tư
console.log(bodem.dem);  // undefined
bodem.dem = 100;         // Không ảnh hưởng gì
console.log(bodem.layGiaTri()); // Vẫn là 1!
```

**Lợi ích:**
- ✅ Đóng gói dữ liệu
- ✅ Biến riêng tư
- ✅ Kiểm soát truy cập

![Bộ đếm Closure](https://images.unsplash.com/photo-1434626881859-194d67b2b86f?w=1200&h=600&fit=crop)

### **Ví dụ 2: Xử lý sự kiện**
```javascript
// ❌ Vấn đề: Tất cả nút bấm hiện cùng giá trị
function cauHinhNutBam() {
    const cacNutBam = document.querySelectorAll('button');
    
    for (var i = 0; i < cacNutBam.length; i++) {
        cacNutBam[i].addEventListener('click', function() {
            console.log('Nút ' + i + ' được bấm');
            // Luôn hiện "Nút 3 được bấm"! (nếu có 3 nút)
        });
    }
}

// Tại sao? var i có phạm vi hàm, tất cả handler dùng chung i
// Khi click, vòng lặp đã xong, i = 3
```
```javascript
// ✅ Giải pháp 1: Dùng let (phạm vi khối)
function cauHinhNutBam() {
    const cacNutBam = document.querySelectorAll('button');
    
    for (let i = 0; i < cacNutBam.length; i++) {
        // Mỗi lần lặp tạo i mới trong phạm vi riêng
        cacNutBam[i].addEventListener('click', function() {
            console.log('Nút ' + i + ' được bấm');
            // Đúng: Nút 0, Nút 1, Nút 2
        });
    }
}

// ✅ Giải pháp 2: Hàm ngay lập tức (IIFE)
function cauHinhNutBam() {
    const cacNutBam = document.querySelectorAll('button');
    
    for (var i = 0; i < cacNutBam.length; i++) {
        (function(chiSo) {
            // Tạo closure với chiSo
            cacNutBam[chiSo].addEventListener('click', function() {
                console.log('Nút ' + chiSo + ' được bấm');
            });
        })(i); // Truyền i làm tham số
    }
}
```

**Câu hỏi phỏng vấn đã giải quyết!** ✅

### **Ví dụ 3: Nhà máy sản xuất hàm**
```javascript
function taoBoNhan(soNhan) {
    return function(so) {
        return so * soNhan;
    };
}

const nhanDoi = taoBoNhan(2);
const nhanBa = taoBoNhan(3);
const nhanBon = taoBoNhan(4);

console.log(nhanDoi(5));     // 10
console.log(nhanBa(5));      // 15
console.log(nhanBon(5));     // 20

// Mỗi hàm "nhớ" soNhan riêng của nó!
```

**Ứng dụng thực tế:**
```javascript
// Nhà máy tạo API client
function taoAPIClient(urlGoc, apiKey) {
    return {
        get: function(duongDan) {
            return fetch(`${urlGoc}${duongDan}`, {
                headers: { 'Authorization': `Bearer ${apiKey}` }
            });
        },
        post: function(duongDan, duLieu) {
            return fetch(`${urlGoc}${duongDan}`, {
                method: 'POST',
                headers: { 
                    'Authorization': `Bearer ${apiKey}`,
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(duLieu)
            });
        }
    };
}

// Tạo các client khác nhau
const githubAPI = taoAPIClient('https://api.github.com', 'github_token');
const stripeAPI = taoAPIClient('https://api.stripe.com', 'stripe_token');

// Mỗi cái nhớ urlGoc và apiKey riêng
githubAPI.get('/user/repos');
stripeAPI.post('/charges', { amount: 1000 });
```

![Nhà máy hàm](https://images.unsplash.com/photo-1581291518633-83b4ebd1d83e?w=1200&h=600&fit=crop)

---

## 🎯 Mẫu thiết kế nâng cao với Closure

### **Mẫu 1: Module (Đóng gói)**
```javascript
const MayTinh = (function() {
    // Biến và hàm riêng tư
    let ketQua = 0;
    
    function ghiNhatKy(thongBao) {
        console.log(`[Máy tính] ${thongBao}`);
    }
    
    // API công khai
    return {
        cong: function(x) {
            ketQua += x;
            ghiNhatKy(`Cộng ${x}, kết quả: ${ketQua}`);
            return this; // Cho phép gọi chuỗi
        },
        tru: function(x) {
            ketQua -= x;
            ghiNhatKy(`Trừ ${x}, kết quả: ${ketQua}`);
            return this;
        },
        nhan: function(x) {
            ketQua *= x;
            ghiNhatKy(`Nhân ${x}, kết quả: ${ketQua}`);
            return this;
        },
        layKetQua: function() {
            return ketQua;
        },
        datLai: function() {
            ketQua = 0;
            ghiNhatKy('Đặt lại');
            return this;
        }
    };
})();

// Sử dụng
MayTinh
    .cong(5)
    .nhan(2)
    .tru(3)
    .layKetQua(); // 7

// Không thể truy cập các thành phần riêng tư
MayTinh.ketQua;  // undefined
MayTinh.ghiNhatKy('test'); // Lỗi
```

**Lợi ích:**
- ✅ Đóng gói
- ✅ Tách biệt public/private
- ✅ Tránh làm ô nhiễm namespace

### **Mẫu 2: Ghi nhớ (Caching)**
```javascript
function ghiNho(ham) {
    const boNho = {}; // Bộ nhớ cache riêng tư qua closure
    
    return function(...thamSo) {
        const khoa = JSON.stringify(thamSo);
        
        if (khoa in boNho) {
            console.log('Lấy từ bộ nhớ');
            return boNho[khoa];
        }
        
        console.log('Tính toán kết quả');
        const ketQua = ham.apply(this, thamSo);
        boNho[khoa] = ketQua;
        
        return ketQua;
    };
}

// Hàm tốn kém
function fibonacci(n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

const fibGhiNho = ghiNho(fibonacci);

console.log(fibGhiNho(40)); // Tính toán... 102334155 (chậm)
console.log(fibGhiNho(40)); // Lấy từ bộ nhớ: 102334155 (tức thì!)
```

**Tăng hiệu suất:** Nhanh hơn 1000 lần cho các lần gọi lặp lại!

### **Mẫu 3: Currying (Gọi từng phần)**
```javascript
// Hàm thông thường
function cong(a, b, c) {
    return a + b + c;
}

cong(1, 2, 3); // 6

// Phiên bản curry
function congCurry(a) {
    return function(b) {
        return function(c) {
            return a + b + c;
        };
    };
}

congCurry(1)(2)(3); // 6

// Hoặc với arrow function
const congCurry = a => b => c => a + b + c;

// Áp dụng từng phần
const cong5 = congCurry(5);
const cong5Va10 = cong5(10);

console.log(cong5Va10(3)); // 18
```

**Ví dụ thực tế:**
```javascript
// Tạo xử lý sự kiện tổng quát
const taoXuLySuKien = loaiSuKien => phanTu => callback => {
    phanTu.addEventListener(loaiSuKien, callback);
};

const khiClick = taoXuLySuKien('click');
const khiDiChuot = taoXuLySuKien('mouseenter');

// Xử lý có thể tái sử dụng
const clickNutBam = khiClick(document.querySelector('button'));
const diChuotVaoDiv = khiDiChuot(document.querySelector('div'));

clickNutBam(() => console.log('Nút được bấm'));
diChuotVaoDiv(() => console.log('Chuột vào div'));
```

![Mẫu Currying](https://images.unsplash.com/photo-1558618666-fcd25c85cd64?w=1200&h=600&fit=crop)

---

## ⚠️ Cạm bẫy Closure & Giải pháp

### **Cạm bẫy 1: Rò rỉ bộ nhớ**
```javascript
// ❌ Rò rỉ bộ nhớ
function phepToanNang() {
    const duLieuLon = new Array(1000000).fill('dữ liệu');
    
    return function() {
        // Closure giữ tham chiếu đến duLieuLon
        console.log(duLieuLon.length);
    };
}

const ham = phepToanNang();
// duLieuLon vẫn trong bộ nhớ dù không dùng!
```
```javascript
// ✅ Giải pháp: Chỉ "đóng gói" những gì cần
function phepToanNang() {
    const duLieuLon = new Array(1000000).fill('dữ liệu');
    const doDai = duLieuLon.length; // Chỉ lấy giá trị cần thiết
    
    return function() {
        // Chỉ đóng gói doDai, không phải toàn bộ duLieuLon
        console.log(doDai);
    };
}
```

### **Cạm bẫy 2: Vòng lặp + Bất đồng bộ**

Đây là bug kinh điển từ đầu bài:
```javascript
// ❌ Vấn đề
for (var i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i); // 3, 3, 3
    }, 1000);
}

// ✅ Giải pháp 1: let
for (let i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i); // 0, 1, 2
    }, 1000);
}

// ✅ Giải pháp 2: IIFE
for (var i = 0; i < 3; i++) {
    (function(chiSo) {
        setTimeout(function() {
            console.log(chiSo); // 0, 1, 2
        }, 1000);
    })(i);
}

// ✅ Giải pháp 3: Truyền vào setTimeout
for (var i = 0; i < 3; i++) {
    setTimeout(function(chiSo) {
        console.log(chiSo); // 0, 1, 2
    }, 1000, i);
}
```

### **Cạm bẫy 3: Biến toàn cục vô tình**
```javascript
function taoBoDem() {
    // ❌ Quên 'let' - tạo biến toàn cục!
    dem = 0;
    
    return function() {
        return ++dem;
    };
}

const bodem1 = taoBoDem();
const bodem2 = taoBoDem();

console.log(bodem1()); // 1
console.log(bodem2()); // 2 (dùng chung biến toàn cục dem!)
```
```javascript
// ✅ Luôn dùng let/const
function taoBoDem() {
    let dem = 0; // Phạm vi đúng
    
    return function() {
        return ++dem;
    };
}
```

**Dùng chế độ nghiêm ngặt để bắt lỗi:**
```javascript
'use strict';

function taoBoDem() {
    dem = 0; // Lỗi: dem chưa được khai báo
    return function() {
        return ++dem;
    };
}
```

![Cảnh báo rò rỉ bộ nhớ](https://images.unsplash.com/photo-1563986768609-322da13575f3?w=1200&h=600&fit=crop)

---

## 🏗️ Dự án thực tế: Giỏ hàng với Closure
```javascript
function taoGioHang() {
    // Trạng thái riêng tư
    let cacMonHang = [];
    let tyLeGiamGia = 0;
    
    // Hàm hỗ trợ riêng tư
    function tinhTongMonHang(monHang) {
        return monHang.gia * monHang.soLuong;
    }
    
    // API công khai
    return {
        themMonHang: function(sanPham, soLuong = 1) {
            const monHangCoSan = cacMonHang.find(mh => mh.id === sanPham.id);
            
            if (monHangCoSan) {
                monHangCoSan.soLuong += soLuong;
            } else {
                cacMonHang.push({
                    ...sanPham,
                    soLuong
                });
            }
            
            return this;
        },
        
        xoaMonHang: function(idSanPham) {
            cacMonHang = cacMonHang.filter(mh => mh.id !== idSanPham);
            return this;
        },
        
        capNhatSoLuong: function(idSanPham, soLuong) {
            const monHang = cacMonHang.find(mh => mh.id === idSanPham);
            if (monHang) {
                monHang.soLuong = soLuong;
            }
            return this;
        },
        
        apDungGiamGia: function(tyLe) {
            tyLeGiamGia = tyLe;
            return this;
        },
        
        tinhTongTien: function() {
            const tongTam = cacMonHang.reduce((tong, mh) => {
                return tong + tinhTongMonHang(mh);
            }, 0);
            
            return tongTam * (1 - tyLeGiamGia);
        },
        
        layDanhSach: function() {
            // Trả về bản sao để tránh sửa đổi từ bên ngoài
            return cacMonHang.map(mh => ({ ...mh }));
        },
        
        demSoLuong: function() {
            return cacMonHang.reduce((dem, mh) => dem + mh.soLuong, 0);
        },
        
        xoaTrang: function() {
            cacMonHang = [];
            tyLeGiamGia = 0;
            return this;
        }
    };
}

// Sử dụng
const gioHang = taoGioHang();

gioHang
    .themMonHang({ id: 1, ten: 'Laptop', gia: 20000000 }, 1)
    .themMonHang({ id: 2, ten: 'Chuột', gia: 500000 }, 2)
    .apDungGiamGia(0.1); // Giảm 10%

console.log('Tổng tiền:', gioHang.tinhTongTien()); // 18.900.000đ
console.log('Số lượng:', gioHang.demSoLuong()); // 3

// Mảng cacMonHang hoàn toàn riêng tư
gioHang.cacMonHang; // undefined
```

**Tại sao dùng closure?**
- ✅ `cacMonHang` và `tyLeGiamGia` hoàn toàn riêng tư
- ✅ Kiểm soát truy cập qua các phương thức công khai
- ✅ Không thể làm rối trạng thái nội bộ từ bên ngoài
- ✅ API sạch đẹp

---

## 💡 Khi nào dùng Closure?

### **Nên dùng Closure cho:**

✅ **Bảo mật dữ liệu**
```javascript
function taoNguoiDung(ten) {
    let matKhau; // Riêng tư!
    
    return {
        datMatKhau: (mk) => { matKhau = mk; },
        kiemTraMatKhau: (mk) => { return mk === matKhau; }
    };
}
```

✅ **Nhà máy hàm**
```javascript
const taoKiemTra = (bieuThuc) => (dauVao) => bieuThuc.test(dauVao);

const laEmail = taoKiemTra(/^\S+@\S+$/);
const laSoDienThoai = taoKiemTra(/^\d{10}$/);
```

✅ **Ghi nhớ/Cache**
```javascript
const hamGhiNho = ghiNho(hamTonKem);
```

✅ **Xử lý sự kiện**
```javascript
nutBam.addEventListener('click', taoXuLySuKien(duLieuNguoiDung));
```

### **ĐỪNG lạm dụng:**

❌ Không cần closure cho hàm đơn giản
```javascript
// Thừa thãi
const cong = (a) => (b) => a + b;

// Chỉ cần thế này
const cong = (a, b) => a + b;
```

❌ Cẩn thận với rò rỉ bộ nhớ
```javascript
// Đừng đóng gói các đối tượng lớn không cần thiết
```

---

## 📊 Hành trình hiểu Closure

### **Giai đoạn 1: "Closure là gì?"** (Tuần 1)

Bối rối. Ví dụ không hiểu. Tại sao hàm nhớ được?

### **Giai đoạn 2: "À, cú pháp như thế"** (Tuần 2-3)

Có thể viết closure đơn giản. Copy-paste mẫu thì được.

### **Giai đoạn 3: "Khoan, tại sao vậy?"** (Tuần 4-6)

Hiểu chuỗi phạm vi. Ngữ cảnh thực thi. Môi trường từ vựng. Lý thuyết bắt đầu rõ.

### **Giai đoạn 4: "Mạnh quá!"** (Tuần 7-8)

Nhận ra mẫu closure ở khắp nơi. React hooks, xử lý sự kiện, modules.

### **Giai đoạn 5: "Không thể thiếu"** (Tuần 9+)

Dùng closure một cách tự nhiên. Tạo mẫu riêng. Đánh giá cao sự tinh tế.

**Tổng thời gian:** Khoảng 2-3 tháng để thực sự thành thạo.

---

## 📚 Tài nguyên học tập

**Học:**
- [MDN: Closures](https://developer.mozilla.org/vi/docs/Web/JavaScript/Closures)
- [JavaScript.info: Closures](https://javascript.info/closure)
- [You Don't Know JS: Phạm vi & Closures](https://github.com/getify/You-Dont-Know-JS)

**Thực hành:**
- Xây dựng biến riêng tư
- Tạo nhà máy hàm
- Làm hàm ghi nhớ
- Refactor code với closure

---

## 💭 Kết luận

9 tháng trước: *"Closure? Chưa nghe bao giờ."*

6 tháng trước: *"Closure có tồn tại nhưng... khó hiểu quá."*

3 tháng trước: *"À! Giờ hiểu rồi!"*

Hôm nay: *"Closure ở khắp nơi. Không thể không thấy nó."*

**Bài học:**

Closure không phải là "tính năng nâng cao". Đây là **nền tảng** của JavaScript. Nó xảy ra mỗi khi hàm truy cập biến ở phạm vi bên ngoài. Bạn đã dùng nó mà không biết!

React Hooks? Closure.  
Xử lý sự kiện? Closure.  
Modules? Closure.  
Biến riêng tư? Closure.

**Insight quan trọng:**

> "Các hàm JavaScript mang theo phạm vi của chúng. Hàm không chỉ nhớ những gì chúng làm - chúng còn nhớ NƠI chúng sinh ra."

Hiểu closure = Hiểu JavaScript sâu sắc.

**Từ 'Hả?' đến 'À ra thế!' - Hành trình xứng đáng với mọi bối rối. 🚀**

---

**Bạn đã hiểu closure chưa? Chia sẻ khoảnh khắc "à ra thế!" nhé!**

**#JavaScript #Closures #PhạmViBiến #MôiTrườngTừVựng #NângCao**