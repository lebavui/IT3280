---
layout: page
title: Lab 2 - Tập lệnh, các lệnh cơ bản, các chỉ thị biên dịch
permalink: /lab2/
nav_order: 2
---

# Bài 2 - Tập lệnh, các lệnh cơ bản, các chỉ thị biên dịch

## Mục đích

Sau bài thực hành này, sinh viên sẽ nắm được nguyên lý cơ bản về cách CPU thực hiện lệnh, tập lệnh của bộ xử lý kiến trúc RISC-V; sử dụng các lệnh hợp ngữ cơ bản và sử dụng công cụ gỡ rối để kiểm nghiệm lại các kiến thức về tập lệnh và hợp ngữ. Sinh viên cũng thành thạo với các chỉ thị biên dịch (Directive) để công cụ RARS có thể dịch thành mã máy một cách đúng đắn.

---

## Tài liệu
- Tài liệu về RISC-V, slide bài giảng học phần.
- The RISC-V Instruction Set Manual

---

## Bài chuẩn bị ở nhà

### Home Assignment 1
Tìm hiểu các thành phần cơ bản của máy tính, mô hình lập trình của máy tính - luồng thực thi của chương trình (Cách chương trình thực hiện trong máy tính) và kiến trúc tập lệnh (Instruction set architecture).

![pic2_1!](../images/pic2_1.png)

### Home Assignment 2
Từ mô hình lập trình của máy tính, đọc tài liệu về kiến trúc RISC-V và ghi nhớ các kiến thức cơ bản sau:
- Luồng hoạt động chương trình trong máy tính, để hiểu rõ:
	- Tên và ý nghĩa của 32 thanh ghi.
	- Các chỉ thị biên dịch (directives)
	- Các thanh ghi đặc biệt: thanh ghi PC, thanh ghi zero, (thanh ghi IR)
- Kiến trúc RV32I, extension mở rộng hỗ trợ phép nhân (Multiplication Extension)
- Khuôn dạng lệnh của các loại lệnh: R, I, S, B, U, J.

![pic2_2!](../images/pic2_2.png)

### Home Assignment 3
Mở menu Help trong RARS để tìm hiểu về các lệnh cơ bản và các chỉ thị biên dịch.

![pic2_3!](../images/pic2_3.png)

---

## Bài thực hành

### Assignment 1: Lệnh gán số nguyên nhỏ 12-bit
**Support:** Trong kiến trúc RV32I không có lệnh gán giá trị trực tiếp vào thanh ghi, mà có thể sử dụng lệnh cộng để gán, ví dụ muốn gán giá trị 0x123 vào thanh ghi **s0**, sử dụng lệnh cộng **addi s0, zero, 0x123**. Tuy nhiên lệnh **addi** thuộc khuôn dạng lệnh I, sử dụng 12 bits để biểu diễn số nguyên có dấu (imm[11:0]). Do đó lệnh **addi** chỉ có thể sử dụng để gán số nguyên có dấu trong phạm vi 12 bits (từ -2048 đến 2047).

Nhập chương trình sau vào công cụ giả lập RARS:

```
# Laboratory Exercise 2, Assignment 1
.text
   addi s0, zero, 0x512    # s0 = 0 + 0x512; I-type: chỉ có thể lưu   
                           # được hằng số 12 bits
   add  s0, x0, zero       # s0 = 0 + 0 ; R-type: có thể sự dụng số
                           # hiệu thanh ghi thay cho tên thanh ghi
```

**Yêu cầu:**
- Sử dụng công cụ gỡ lỗi, chạy từng lệnh và dừng lại.
- Ở mỗi lệnh, quan sát cửa sổ Registers và chú ý:
	- Sự thay đổi giá trị thanh ghi **s0**.
	- Sự thay đổi giá trị thanh ghi **pc** (thanh ghi **pc** nằm ở vị trí dưới cùng trong cửa sổ Registers).
- Trong cửa sổ Text Segment, hãy so sánh mã máy của các lệnh trên với khuôn dạng lệnh để chứng tỏ các lệnh đó đúng như tập lệnh đã quy định.
- Sửa lại lệnh **addi** như bên dưới. Chuyện gì xảy ra sau đó. Hãy giải thích.

`addi s0, zero, 0x20232024`

### Assignment 2: Lệnh gán số 32-bit
**Support:** Để gán một số 32-bit vào thanh ghi 32-bit, sử dụng lệnh load upper immediate instruction (lui) kết hợp với lệnh **addi**. Lệnh **lui**, thuộc khuôn dạng lệnh U-type chứa hằng số 20-bit, nạp một hằng số 20 bits vào 20 bits có trọng số cao (most significant bit) của thanh ghi đích. Lệnh **lui** được sử dụng để nạp 20 bits trọng số cao, và lệnh **addi** được sử dụng để nạp 12 bits trọng số thấp (least significant bit). 

Nhập chương trình sau vào công cụ giả lập RARS:

```
# Laboratory Exercise 2, Assignment 2
# Load 0x20232024 to s0 register
.text
   lui s0, 0x20232     # s0 = 0xABCDE000           
   addi s0, s0, 0x024  # s0 = s0 + 0x123
```

**Yêu cầu:**
- Sử dụng công cụ gỗ lỗi, chạy từng lệnh và dừng lại.
- Ở mỗi lệnh, quan sát cửa sổ Registers và chú ý:
   - Sự thay đổi giá trị thanh ghi **s0**.
   - Sự thay đổi giá trị thanh ghi **pc**.
- Trong cửa sổ Data Segment, nhấn vào Combo Box để chọn và hiển thị dữ liệu trong vùng nhớ chứa lệnh (.text).
   - So sánh dữ liệu trong vùng Data Segment và mã máy trong Text Segment.

**Chú ý:**
- Hằng số (immediate) trong RISC-V luôn là số bù 2 12-bit (12-bit two’s complement numbers), nên khi thực hiện chúng phải mở rộng dấu thành 32-bit với kiểu sign-extended. Một số bộ xử lý khác có cũng có kiểu mở rộng dấu zero-extend. Cần phân biệt hai kiểu mở rộng dấu này.
- Khi muốn nạp một hằng số lớn vào thanh ghi, nếu số 12-bit trong lệnh **addi** là số âm (bit thứ 11 bằng 1), thì cần mở rộng dấu thành 32 bit, và số 20-bit trong lệnh **lui** cần phải tăng lên 1. Giải thích tại sao? 

```
# Laboratory Exercise 2, Assignment 2
# IN HIGH LEVEL LANGUAGE
# int a = 0xFEEDB987;
# IN ASSEMBLY LANGUAGE
.text
   lui s0, 0xFEEDC         # s0 = 0xFEEDC000           
   addi s0, s0, 0xFFFFF987 # s0 = 0xFEEDB987
```

### Assignment 3: Lệnh gán (giả lệnh)

**Support:** Chú ý rằng RISC-V được thiết kế dựa trên kiến trúc RISC, nên kích thước lệnh và độ phức tạp của phần cứng được thiết kế sao cho tối ưu nhất. Tuy nhiên, các công cụ giả lập cung cấp một số lệnh giả (Extended/Pseudo Instructions), các lệnh giả này không nằm trong tập lệnh của RISC-V, nó phổ biến đối với compiler/assemnler và lập trình viên. Khi chuyển sang mã máy, các lệnh giả được dịch sang một hoặc nhiều lệnh chính thống.

Nhập chương trình sau vào công cụ giả lập RARS:

```
# Laboratory Exercise 2, Assignment 3
.text
   li s0, 0x20232024           
   li s0, 0x20
```

**Yêu cầu:**
- Biên dịch, quan sát và so sánh các lệnh ở cột Source và cột Basic trong cửa sổ Text Segment. Giải thích kết quả.

### Assignment 4: Tính biểu thức 2x + y = ?

Nhập chương trình sau vào công cụ RARS. 

```
# Laboratory Exercise 2, Assignment 4
.text
   # Assign X,Y into t1,t2 register
   addi t1, zero, 5     # X = t1 = ?
   addi t2, zero, -1    # Y = t2 = ?
      
   # Expression Z = 2X + Y
   add s0, t1, t1    # s0 = t1 + t1 = X + X = 2X
   add s0, s0, t2    # s0 = s0 + t2 = 2X + Y
```

**Yêu cầu:**
- Sử dụng công cụ gỡ lỗi, chạy từng lệnh và dừng lại.
- Ở mỗi lệnh, quan sát cửa sổ Registers và chú ý:
   - Sự thay đổi giá trị các thanh ghi.
   - Sau khi kết thúc chương trình, kiểm tra xết quả có đúng không?
- Trong cửa số Text Segment, quan sát mã máy của các các lệnh. Từ đó kiểm nghiệm mã máy với khuôn mẫu của lệnh **addi** (I-type) và lệnh **add** (R-type).
- Để hiểu hơn bản chất của việc này, hãy chuyển đổi mã basic của hai lệnh **addi** và **add** dựa vào khuôn dạng lệnh tương ứng, và so sánh với mã máy của từng lệnh (ở cột Code trong cửa sổ Text Segment).
 
### Assignment 5: Phép nhân

**Support:** Phép nhân khác so với các lệnh toán học còn lại, bởi vì khi nhân hai số 32-bit, kết quả có thể là số 64-bit. Kiến trúc RISC-V cung cấp các lệnh khác nhau để thực hiện phép nhân, có thể ghi kết quả là 32-bit hoặc 64-bit, tùy lệnh. Các lệnh này không nằm trong kiến trúc RV32I, mà được nằm trong extension RV32M (RISC-V multiply/divided extension).
- Lệnh **multiply mul rd, rs1, rs2**: Nhân hai toán hạng nằm trong thanh ghi rs1, rs2 và ghi 32 bits có trọng số thấp của kết quả vào thanh ghi rd.
- Để ghi 32-bit trọng số cao của kết quả phép nhân, RISC-V cung cấp lệnh **mulh**, **mulhsu**, **mulhu**.
   - **mulh**: multiply high signed signed, xử lý trên cả hai toán hạng có dấu.
   - **mulhsu**: multiply high signed unsigned, xử lý trên một toán hạng có dấu, một toán hạng không dấu.
   - **mulhu**: multiply high unsigned unsigned, xử lý trên cả hai toán hạng không dấu.

Nhập chương trình sau vào công cụ giả lập RARS:

```
# Laboratory Exercise 2, Assignment 5
.text
   # Assign X, Y into t1, t2 register
   addi t1, zero, 4        # X = t1 =?
   addi t2, zero, 5        # Y = t2 =?

   # Expression Z = X * Y
   mul s1, t1, t2         # s1 chứa 32 bit thấp
```

**Yêu cầu:**
- Biên dịch và quan sát các lệnh mã máy.
- Sử dụng công cụ gỡ lỗi, chạy từng lệnh và quan sát sự thay đổi của các thanh ghi. Kiểm tra kết quả xem có đúng không?
- Giải thích lại chi tiết đoạn lệnh trên.
- Tìm hiểu lệnh chia, lấy code minh họa, giải thích luồng hoạt động cũng như kết quả của các thanh ghi.

### Assignment 6: Tạo biến và truy cập biến

**Support:** Chỉ định truy cập (assembler derectives) hướng dẫn assembler trong việc cấp phát và khởi tạo biến toàn cục, định nghĩa hằng số, và đoạn lệnh được nạp vào phần text segment, biến được nạp vào data segment, … (Hiểu rõ luồng chương trình hoạt động để hiểu rõ hơn về assembler directives).

Nhập chương trình sau vào công cụ giả lập RARS:

```
# Laboratory Exercise 2, Assignment 6
.data             # Khởi tạo biến (declare memory)
   X: .word 5     # Biến X, kiểu word (4 bytes), giá trị khởi tạo = 5
   Y: .word -1    # Biến Y, kiểu word (4 bytes), giá trị khởi tạo = -1
   Z: .word 0     # Biến Z, kiểu word (4 bytes), giá trị khởi tạo = 0

.text          # Khởi tạo lệnh (declare instruction)
   # Nạp giá trị X và Y vào các thanh ghi
   la t5, X       # Lấy địa chỉ của X trong vùng nhớ chứa dữ liệu 
   la t6, Y       # Lấy địa chỉ của Y
   lw t1, 0(t5)   # t1 = X
   lw t2, 0(t6)   # t2 = Y
   
   # Tính biểu thức Z = 2X + Y với các thanh ghi
   add s0, t1, t1
   add s0, s0, t2
     
   # Lưu kết quả từ thanh ghi vào bộ nhớ
   la t4, Z       # Lấy địa chỉ của Z
   sw s0, 0(t4)   # Lưu giá trị của Z từ thanh ghi vào bộ nhớ
```

**Yêu cầu:**
- Biên dịch và quan sát các lệnh trong cửa sổ Text Segment.
   - Lệnh **la** (load address) được biên dịch như thế nào? Giải thích cơ chế hoạt động của lệnh **la**. (Gợi ý, để ý tới giá trị thanh ghi **pc**, địa chỉ của nhãn, đọc tài liệu để hiểu cách hoạt động của lệnh **auipc** và **la**).
- Trong cửa sổ Labels, xem địa chỉ của biến X, Y, Z được lưu trong bộ nhớ. 
   - Double click vào nhãn X, Y, Z trong cửa sổ Label để di chuyển đến vị trí của biến tương ứng trong bộ nhớ trong Data Segment. Xác nhận giá trị của biến trong bộ nhớ và giá trị khởi tạo trong mã nguồn.
- Sử dụng công cụ gỡ lỗi, chạy từng lệnh và dừng lại.
- Ở mỗi lệnh, quan sát cửa sổ Registers và chú ý:
   - Sự thay đổi các thanh ghi.
   - Xác định vai trò của lệnh **lw** và **sw**.
- Tìm hiểu thêm các lệnh **lb**, **sb**.
- Ghi nhớ qui tắc xử lý:
   - Nạp giá trị của biến từ vùng nhớ vào thanh ghi bằng cặp lệnh **la**, **lw**.
   - Xử lý dữ liệu trên thanh ghi.
   - Lưu kết quả từ thanh ghi trở lại vùng nhớ của biến bằng cặp lệnh **la**, **sw**.

---

## Kết luận

