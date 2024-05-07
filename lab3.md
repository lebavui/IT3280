---
layout: page
title: Lab 3 - Các lệnh nhảy và lệnh rẽ nhánh
permalink: /lab3/
nav_order: 3
---

# Bài 3 - Các lệnh nhảy và lệnh rẽ nhánh

## Mục đích
Sau bài thực hành này, sinh viên sẽ hiểu và biết cách sử dụng các lệnh nhảy và lệnh rẽ nhánh. Từ đó, sinh viên có thể thực hiện các câu lệnh điều khiển, vòng lặp trong ngôn ngữ lập trình bậc cao bằng hợp ngữ, hiểu bản chất của các lệnh đó khi thực hiện trong máy tính.

--- 

## Tài liệu

---

## Chuẩn bị

Trước khi bắt đầu bài này, sinh viên cần xem trước tài liệu tham khảo và bài thực hành chi tiết, cẩn thận. Để làm tốt, sinh viên cần xem lại và hiểu rõ bài thực hành 2, sử dụng công cụ mô phỏng thành thạo.

---

## Bài chuẩn bị ở nhà

- Lệnh nhảy có điều kiện (khi điều kiện thỏa mãn thì nhảy đến nhãn): RISC-V hỗ trợ 6 lệnh nhảy có điều kiện.
![pic3_1!](../images/pic3_1.png)
- Lệnh nhảy không điều kiện: RISC-V hỗ trợ hai lệnh dưới đây.
![pic3_2!](../images/pic3_2.png)
Tuy nhiên ta sử dụng giả lệnh **jump** (**j label**), lệnh này nhảy đến label được đánh dấu trong mã nguồn.
- Lệnh **slt t1, t2, t3**, thiết lập t1 = 1 nếu t2 < t3, ngược lại xóa t1 = 0.
- Các ví dụ dưới đây minh họa cách thực hiện các cấu trúc thường sử dụng trong ngôn ngữ lập trình bậc cao, 
    - Cấu trúc rẽ nhánh **if/else** 
    - Cấu trúc lặp **for/while**
    - Cấu trúc lựa chọn **switch/case**

### Home Assignment 1

Ví dụ minh họa dưới đây thực hiện cấu trúc rẽ nhánh if-then-else sử dụng các lệnh hợp ngữ được đề cập ở trên.
Cấu trúc cần thực hiện như sau:
```
if (i <= j)
    x = x + 1
    z = 1
else
    y = y - 1
    z = 2 * z
```

Trước hết, sinh viên nên vẽ lưu đồ thuật toán cho ví dụ này. Sau đó đọc kỹ đoạn mã hợp ngữ sau đây, hiểu rõ chức năng từng lệnh.
```
# Laboratory Exercise 3, Home Assignment 1
.text
start:
   # TODO:
   # Initialize i to s1
   # Initialize j to s2

   # Cách 1:
   # blt s2, s1, else   # if j < i then jump else

   # Cách 2:
   slt      t0, s2, s1     # set t0 = 1 if j < i else clear t0 = 0
   bne      t0, zero, else # t0 != 0 means t0 = 1, jump else   

then: 
   addi  t1, t1, 1         # then part: x=x+1
   addi  t3, zero, 1       # z=1
   j     endif             # skip “else” part
else: 
   addi  t2, t2, -1        # begin else part: y=y-1
   add   t3, t3, t3        # z=2*z
endif:
```

### Home Assignment 2

Ví dụ dưới đây minh họa cách thực hiện một cấu trúc lặp để tính tổng các phần tử của mảng A.
Mô tả thuật toán với ngôn ngữ lập trình bậc cao (ngôn ngữ lập trình C):
```
sum = 0;
for (int i = 0; i < n; i += step)
    sum += A[i];
```
Mô tả thuật toán với lập trình hợp ngữ:
```
    sum = 0
    i = 0
loop:
    if (i >= n) goto endloop
    sum = sum + A[i]
    i = i + step
    goto loop
endloop:
```

**Cách sử dụng mảng trong lập trình hợp ngữ:** A là biến lưu trữ địa chỉ đầu tiên của mảng – địa chỉ của phần tử A[0], vậy khi muốn truy cập tới phần tử A[i], ta cần truy cập tới ô nhớ có địa chỉ A + 4×i. Sử dụng lệnh lw để đọc phần tử mảng vào thanh ghi, và lệnh sw để ghi giá trị từ thanh ghi vào phần tử mảng.
![pic3_3!](../images/pic3_3.png)

Giả sử ta có chỉ số i, địa chỉ cơ sở của mảng A, số phần tử mảng n, bước lặp và kết quả tính tổng được lưu trữ tương ứng các trong thanh ghi s1, s2, s3, s4, s5. Đọc kỹ đoạn mã nguồn sau để hiểu cách chương trình hoạt động.
```
# Laboratory 3, Home Assigment 2
.data
   A: .word 1, 3, 2, 5, 4, 7, 8, 9, 6
.text
   # TODO: Initialize s2, s3, s4 registers
   li s1, 0    # i = 0
   li s5, 0    # sum = 0
loop: 
   slt t2, s1, s3    # check loop condition i < n
   beq   t2, zero, endloop # if i >= n then end loop
   add t1, s1, s1    # t1 = 2 * s1
   add t1, t1, t1    # t1 = 4 * s1 => t1 = 4*i
   add t1, t1, s2    # t1 store the address of A[i]
   lw    t0, 0(t1)   # load value of A[i] in t0
   add s5, s5, t0    # sum = sum + A[i]
   add s1, s1, s4 # i = i + step
   j  loop           # go to loop
endloop:
```

### Home Assignment 3

Cấu trúc **switch/case** cho phép rẽ nhánh theo nhiều hướng dựa vào giá trị của một biến số nguyên. Trong ví dụ dưới đây, biến test có thể mang một trong các giá trị 0, 1, 2 và tùy thuộc vào giá trị cụ thể của biến test mà các câu lệnh khác nhau sẽ được thực hiện.
```
switch(test) {
    case 0:
        a=a+1; break;
    case 1:
        a=a-1; break;
    case 2:
        b=2*b; break;
    }
```
Giả sử a và b được lưu trữ trong thanh ghi s2 và s3, giá trị của biến test được khai báo trong bộ nhớ. Đọc kỹ đoạn mã sau và hiểu các thực hiện biểu thức lựa chọn switch/case bằng hợp ngữ.
```
# Laboratory Exercise 3, Home Assignment 3
.data
   test: .word 0
.text
   la s0, test    # Nạp địa chỉ của biến test vào s0
   lw s1, 0(s0)   # Nạp giá trị của biến test vào s1
   li t0, 0       # Nạp giá trị cần kiểm tra
   li t1, 1       # Nạp giá trị cần kiểm tra
   li t2, 2       # Nạp giá trị cần kiểm tra
   beq s1, t0, case_0
   beq s1, t1, case_1
   beq s1, t2, case_2
   j default
case_0: 
   addi s2, s2, 1 # a = a + 1
   j continue
case_1: 
   sub s2, s2, t1 # a = a - 1
   j continue
case_2: 
   add s3, s3, s3 # b = 2 * b
   j continue
default: 
continue: 
```

---

## Bài thực hành

### Assignment 1

Tạo project để thực hiện đoạn mã trong Home Assignment 1. Khởi tạo các biến cần thiết. Dịch và mô phỏng với RARS. Chạy chương trình ở chế độ từng dòng lệnh, kiểm tra sự thay đổi của bộ nhớ và nội dung các thanh ghi ở mỗi bước chạy.

### Assignment 2

Tạo project để thực hiện đoạn mã trong Home Assignment 2. Khởi tạo các biến cần thiết và mảng A. Dịch và mô phỏng với RARS. Chạy chương trình ở chế độ từng dòng lệnh, quan sát sự thay đổi của bộ nhớ và nội dung các thanh ghi ở mỗi bước chạy. Thay bộ giá trị khác để kiểm tra sự đúng đắn của chương trình.

### Assignment 3

Tạo project để thực hiện đoạn mã trong Home Assignment 3. Dịch và mô phỏng với RARS. Chạy chương trình ở chế độ từng dòng lệnh, quan sát sự thay đổi của bộ nhớ và nội dung các thanh ghi ở từng bước chạy. Thay đổi bộ giá trị và chạy lại chương trình một vài lần để kiểm tra tất cả các trường hợp.

### Assignment 4

Thay thế điều kiện rẽ nhánh trong Home Assignment 1 bằng các điều kiện mới sau đây:
1. i < j
2. i >= j
3. i + j <= 0
4. i + j > m + n (với m và n được lưu trong các thanh ghi khác)

### Assignment 5

Thay thế điều kiện nhảy trong Home Assignment 2 bằng các điều kiện nhảy mới sau đây:
1. i <= n
2. sum >= 0
3. A[i] != 0

### Assignment 6

Tạo project để thực hiện chương trình sau: Tìm phần tử có giá trị tuyệt đối lớn nhất từ một danh sách các số nguyên 32-bit. Giả sử danh sách số nguyên được lưu trong một mảng biết trước số phần tử.

---

## Kết luận

Trước khi hoàn thành bài thực hành này, suy nghĩ cách giải quyết vấn đề sau: Các lệnh **branch** khác gì so với các lệnh **jump**, tại sao chúng được chia thành 2 khuôn dạng khác nhau?
