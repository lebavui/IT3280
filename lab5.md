---
layout: page
title: Lab 5 - Các hàm dịch vụ hệ thống
permalink: /lab5/
nav_order: 5
---

# Bài 5 - Các hàm dịch vụ hệ thống

## Mục đích

Sau bài thực hành này, sinh viên sẽ nắm được cách lưu trữ các chuỗi ký tự trong bộ nhớ. Sinh viên cũng có thể lập trình được ứng dụng xử lý chuỗi ký tự và đưa ra màn hình bằng cách sử dụng lời gọi các dịch vụ hệ thống.

---

## Tài liệu

---

## Chuẩn bị

--- 

## About Ecall

Một số các dịch vụ hệ thống, chủ yếu liên quan đến nhập xuất dữ liệu, có thể được sử dụng trong các chương trình hợp ngữ. Khi gọi các dịch vụ hệ thống, nội dung các thanh ghi không ảnh hưởng, ngoại trừ các thanh ghi chưa kết quả trả về. Danh sách các dịch vụ mà RARS cung cấp có thể được tra cứu trong menu Help, thẻ Syscalls. 

### Các bước thực hiện một lời gọi hệ thống 

1. Nạp số hiệu dịch vụ vào thanh ghi a7.
2. Nạp các tham số nếu có vào các thanh ghi được chỉ định, ví dụ a0, a1, a2, …
3. Thực hiện lệnh ecall.
4. Nhận kết quả, nếu có, từ các thanh ghi được chỉ định. 

**Ví dụ: Hiển thị một số nguyên ra cửa sổ Run I/O**
```
.text
   li    a7, 1    # 1 là số hiệu dịch vụ in số nguyên
   li a0, 0x307   # giá trị cần in
   ecall          # thực hiện lời gọi hệ thống
```

**Một số dịch vụ thường dùng**

1. **In số nguyên**

    Hiển thị một số nguyên ra cửa sổ Run I/O. \
    Tham số:
    - a7 = 1
    - a0 = số nguyên cần in

    Giá trị trả về: Không có

    Ví dụ:
    ```
    .text
        li a7, 1
        li a0, 0x307
        ecall
    ```

    Kết quả thực hiện:
    ![pic5_1!](../images/pic5_1.png) 

2. **In số nguyên trong hộp thoại**

    Hiển thị số nguyên trong hộp thoại kèm theo thông điệp. \
    Tham số:
    - a7 = 56
    - a0 = địa chỉ chuỗi ký tự thông điệp
    - a1 = số nguyên cần in

    Giá trị trả về: Không có

    Ví dụ:
    ```
    .data
        message: .asciz "So nguyen la: "
    .text
        li a7, 56      
        la a0, message
        li a1, 0x307   
        ecall
    ```
    
    Kết quả thực hiện: \
    <img src="../images/pic5_2.png" alt="drawing" width="400"/>

3. **In chuỗi ký tự**

    In chuỗi ký tự ra màn hình Run I/O. \
    Tham số:
    - a7 = 4
    - a0 = địa chỉ của chuỗi được in

    Kết quả trả về:	Không có

    Ví dụ:
    ```
    .data
        message: .asciz "Khoa \nKy thuat May tinh"
    .text
        li a7, 4
        la a0, message
        ecall
    ```
    
    Kết quả thực hiện:
    ![pic5_3!](../images/pic5_3.png)

4. **In chuỗi ký tự trong hộp thoại**

    Hiển thị chuỗi ký tự trong hộp thoại kèm theo chuỗi thông điệp. \
    Tham số:
    - a7 = 59
    - a0 = địa chỉ của chuỗi thông điệp
    - a1 = địa chỉ chuỗi cần in 

    Kết quả trả về:	Không có

    Ví dụ:
    ```
    .data
        message: .asciz "Truong: "
        address: .asciz "Cong nghe thong tin va Truyen thong"
    .text
        li  a7, 59
        la  a0, message
        la  a1, address   
        ecall
    ```

    Kết quả thực hiện: \
    <img src="../images/pic5_4.png" alt="drawing" width="400"/>

5. **Nhập số nguyên**

    Nhập một số nguyên từ bàn phím. \
    Tham số:
    - a7 = 5

    Giá trị trả về:
    - a0 = số nguyên được nhập

    Ví dụ:
    ```
    .text
        li a7, 5
        ecall
    ```

    Kết quả thực hiện: \
    <img src="../images/pic5_5.png" alt="drawing" width="400"/>

    **Chú ý: Các dịch vụ 5, 6, 7, 8, 12 có thể nhập dữ liệu trong cửa sổ Run I/O hoặc trong hộp thoại popup bằng cách thay đổi lựa chọn ở menu Settings / Popup dialog for input syscalls (5, 6, 7, 8, 12)**

6. **Nhập chuỗi ký tự**

    Nhập một chuỗi ký tự từ bàn phím. \
    Tham số:
    - a7 = 8
    - a0 = địa chỉ buffer chứa chuỗi ký tự được nhập
    - a1 = độ dài tối đa được nhập

    Giá trị trả về:	Không có

    **Chú ý:** \
    Với độ dài tối đa được nhập là n (a1), chuỗi ký tự có độ dài không vượt quá n-1.
    - Nếu độ dài ngắn hơn, ký tự xuống dòng được chèn vào cuối. 
    - Với trường hợp còn lại, giá trị null được đặt ở cuối chuỗi (vị trí n-1).

    Trường hợp đặc biệt:
    - Nếu n = 1, việc nhập bị bỏ qua, giá trị null được đặt tại địa chỉ đầu của buffer. 
    - Nếu n < 1, việc nhập bị bỏ qua, không giá trị nào được ghi vào buffer.

    Ví dụ:
    ```
    .data
        buffer: .space 100    # Buffer 100 byte chua chuoi ki tu
    .text
        li  a7, 8
        la  a0, buffer
        li  a1, 100
        ecall
    ```

    Kết quả thực hiện: \
    <img src="../images/pic5_6.png" alt="drawing" width="400"/>

    ![pic5_7!](../images/pic5_7.png)
 
7. **Nhập chuỗi ký tự với hộp thoại**

    Hiển thị hộp thoại thông điệp để nhập chuỗi ký tự, cho phép kiểm tra kết quả nhập dữ liệu.

    Tham số:
    - a7	= 54
    - a0 	= địa chỉ chuỗi thông điệp
    - a1	= địa chỉ buffer chứa dữ liệu nhập được
    - a2 	= số ký tự tối đa cần nhập

    Giá trị trả về:
    - a1 chứa trạng thái kết quả:
        - 0: Nhập dữ liệu thành công.
        - -2: Nút Cancel được nhấn. Buffer không thay đổi.
        - -3: Nút OK được nhấn nhưng không có dữ liệu nhập vào. Buffer không thay đổi.
        - -4: Độ dài chuỗi ký tự nhập vào vượt quá độ dài tối đa đã khai báo. Buffer chứa số ký tự tối đa cho phép kèm theo ký tự null kết thúc xâu.

    Ví dụ:
    ```
    .data
        message: .asciz "Ho va ten sinh vien:"
        buffer:  .space   100
    .text
        li   a7, 54
        la   a0, message
        la   a1, buffer
        li   a2, 100
    ecall
    ```

    Kết quả thực hiện: \
    <img src="../images/pic5_8.png" alt="drawing" width="400"/> 

8. **In ký tự**

    In một ký tự ra cửa sổ Run I/O.

    Tham số:
    - a7 = 11
    - a0 = Ký tự cần in (có thể sử dụng số nguyên chứa mã ASCII của ký tự hoặc bản thân ký tự trong cặp dấu nháy đơn)

    Giá trị trả về: Không có

    Ví dụ:
    ```
    .text
    li   a7, 11
    li   a0, 'k'
    ecall 
    ```

    Kết quả thực hiện:
    ![pic5_9!](../images/pic5_9.png)

9. **Nhập ký tự**

    Nhập một ký tự từ bàn phím.

    Tham số:
    - a7 = 12

    Giá trị trả về:
    - a0 = Ký tự được nhập

    Ví dụ:
    ```
    .text
        li   a7, 12
        ecall 
    ```

    Kết quả thực hiện: \
    <img src="../images/pic5_10.png" alt="drawing" width="400"/>

10.	**ConfirmDialog**

    Hiển thị hộp thoại thông điệp với 3 lựa chọn Yes \| No \| Cancel

    Tham số:
    - a7 = 50
    - a0 = địa chỉ chuỗi thông điệp

    Giá trị trả về:
    - a0 = chứa giá trị người dùng đã chọn
        - 0: Yes
        - 1: No
        - 2: Cancel

    Ví dụ:
    ```
    .data
    question: .asciz "Ban co phai la SV Ky thuat May tinh?"
    .text
        li   a7, 50
        la   a0, question
        ecall
    ```

    Kết quả thực hiện: \
    <img src="../images/pic5_11.png" alt="drawing" width="400"/>

11.	**MessageDialog**

    Hiển thị hộp thoại chứa thông điệp kèm theo biểu tượng.

    Tham số:
    - a7 = 55
    - a0 = địa chỉ chuỗi thông điệp
    - a1 = kiểu thông điệp được hiển thị:
        - 0: error message, indicated by Error icon 
        - 1: information message, indicated by Information icon 
        - 2: warning message, indicated by Warning icon 
        - 3: question message, indicated by Question icon 
        - other: plain message (no icon displayed)

    Giá trị trả về: Không có

    Ví dụ:
    ```
    .data
    message: .asciz "Chuong trinh bi loi."
    .text
        li   a7, 55
        la   a0, message
        li a1, 0
        ecall
    ``` 
    Kết quả thực hiện: \
    <img src="../images/pic5_12.png" alt="drawing" width="400"/>

12.	**Exit**

    Kết thúc chương trình. Không có lệnh Exit trong tập lệnh của bất kỳ bộ xử lý nào. Exit là một dịch vụ của hệ điều hành.

    Tham số:
    - a7 = 10

    Giá trị trả về: Không có

    Ví dụ:
    ```
    li   a7, 10
    ecall         
    ```

---

## Bài chuẩn bị ở nhà

### Home Assignment 1

Chương trình hợp ngữ đơn giản sau in ra màn hình một dòng ký tự “Hello world”. Ta dùng hàm printf để làm điều đó. Đọc kỹ đoạn mã này, chú ý đến cách truyền tham số cho hàm printf.

```
# Laboratory Exercise 5, Home Assignment 1
.data
test: .asciz "Hello World"
.text
   li  a7, 4
   la  a0, test
   ecall
```

### Home Assignment 2

Hàm strcpy trong ngôn ngữ C, thực hiện việc sao chép chuỗi y vào chuỗi x sử dụng ký tự null ‘\0’ để đánh dấu kết thúc chuỗi. Chương trình dưới đây minh họa việc thực hiện hàm này. Đọc kỹ và hiểu chương trình này.

![pic5_13!](../images/pic5_13.png)

```
# Laboratory Exercise 5, Home Assignment 2
.data
x: .space 32         # destination string x, empty
y: .asciz "Hello"    # source string y

.text
strcpy:
   add   s0, zero, zero # s0 = i=0
L1:
   add   t1, s0, a1     # t1 = s0 + a1 = i + y[0] = address of y[i]
   lb    t2, 0(t1)      # t2 = value at t1 = y[i]
   add   t3, s0, a0     # t3 = s0 + a0 = i + x[0] = address of x[i]
   sb    t2, 0(t3)      # x[i]= t2 = y[i]
   beq   t2,zero,end_of_strcpy # if y[i]==0, exit
   addi  s0, s0, 1      # s0=s0 + 1 <-> i=i+1
   j     L1             # next character
end_of_strcpy:
```

### Home Assignment 3
Chương trình sau thực hiện đếm số ký tự của một chuỗi ký tự kết thúc bằng ký tự null. Đọc hiểu đoạn mã sau đây.

```
# Laboratory Exercise 5, Home Assignment 3
.data
string: .space 50
message1: .asciz  "Nhap xau: "
message2: .asciz  "Do dai xau la: "

.text
main:
get_string:
   # TODO Input string from keyboard
get_length: 
   la    a0, string           # a0 = address(string[0])
   li    t0, 0                # t0 = i = 0
check_char: 
   add   t1, a0, t0           # t1 = a0 + t0 = address(string[0]+i) 
   lb    t2, 0(t1)            # t2 = string[i]
   beq   t2, zero, end_of_str # Is null char? 
   addi  t0, t0, 1            # t0 = t0 + 1 -> i = i + 1
   j  check_char
end_of_str:  
end_of_get_length:
print_length: 
   # TODO print result to console
```

---

## Bài thực hành

### Assignment 1

Tạo project thực hiện Home Assignment 1. Dịch và nạp chương trình lên trình mô phỏng. Chạy và quan sát kết quả. Chuyển đến Data Segment, kiểm tra cách chuỗi ký tự được lưu trữ trong bộ nhớ.

### Assignment 2

Tạo project thực hiện chương trình in tổng của hai toán hạng nằm trong thanh ghi s0 và s1 theo định dạng sau:
	"The sum of (s0) and (s1) is (result)"

### Assignment 3

Tạo project thực hiện Home Assignment 2. Đọc hiểu mã nguồn, khởi tạo các biến cần thiết cho chương trình, thực hiện hàm strcpy. Dịch và nạp lên mô phỏng, chạy và quan sát kết quả.

### Assignment 4

Tạo project thực hiện Home Assignment 3, sử dụng ecall để nhập chuỗi ký tự cần đếm, và in kết quả ra màn hình.

### Assignment 5

Viết chương trình cho phép người dùng nhập chuỗi ký tự bằng cách nhập từng ký tự. Việc nhập sẽ kết thúc khi người dùng nhấn Enter hoặc khi độ dài chuỗi ký tự vượt quá 20. In chuỗi đã nhập theo chiều ngược lại.


---

## Kết luận