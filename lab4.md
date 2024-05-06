---
layout: page
title: Lab 4 - Các lệnh số học và logic
permalink: /lab4/
nav_order: 4
---

## Mục đích
Sau bài thực hành này, sinh viên hiểu được cách sử dụng các lệnh số học (arithmetic instruction), lệnh logic (logical instruction) và lệnh dịch bit (shift instruction). Từ đó, sinh viên cũng hiểu về hiện tượng tràn số học (overflow, carry out) và cách xác định khi tràn số xảy ra.

---

## Tài liệu

---

## Chuẩn bị

---

## Bài chuẩn bị ở nhà

### Home Assignment 1
Lệnh số học đã được giới thiệu ở Bài thực hành 2, bài này sẽ đề cập đến trường hợp đặc biệt khi thực hiện lệnh cộng hai số 32-bit, đó là hiện tượng tràn số.

**Support:** Tổng của hai số nguyên 32-bit có thể không biểu diễn được trong thanh ghi 32-bit (do bị tràn khi cộng module 2 có nhớ). Trong trường hợp này, ta nói overflow (carry out) xảy ra. Overflow chỉ có thể xảy xa khi hai toán hạng đó cùng dấu (áp dụng cho số có dấu, còn carry-out là tràn số không dấu).
Khi cộng hai toán hạng không âm (âm), nếu tổng của chúng nhỏ hơn (lớn hơn) các toán hạng, overflow sẽ xảy ra.

**Chú ý:** RISC-V không có cơ chế tự bảo vệ như các kiến trúc khác (MIPS), vậy để phòng tránh overflow (carray out) xảy ra, phải cài đặt thuật toán phần mềm để bảo vệ dựa trên quy tắc trên. 

Chương trình dưới đây xác định tràn số dựa vào quy tắc trên. Hai toán hạng nguồn được lưu vào thanh ghi **s1** và **s2**, toán hạng đích được lưu vào thanh ghi **s3**. Thanh ghi **t0** chứa kết quả Nếu tràn số xảy ra **t0** = 1, nếu không xảy ra **t0** = 0.

```
# Laboratory Exercise 4, Home Assignment 1
.text
   # TODO: Initialize s1 and s2 in different cases
   li    t0, 0             # No overflow is default status
   add   s3, s1, s2        # s3 = s1 + s2
   xor   t1, s1, s2        # Test if s1 and s2 have the same sign 
   blt   t1, zero, EXIT    # If not, exit
   slt   t2, s3, s1  
   blt   s1, zero, NEGATIVE   # Test if s1 and s2 is negative?
   beq   t2, zero, EXIT       # s1 and s2 are positive
   # if s3 > s1 then the result is not overflow
   j     OVERFLOW             
NEGATIVE:                     
   bne   t2, zero, EXIT    # s1 and s2 are negative
   # if s3 < s1 then the result is not overflow 
OVERFLOW:
   li    t0, 1          # The result is overflow
EXIT:
```

### Home Assignment 2
Các phép logic cơ bản bao gồm **and**, **or**, **xor** và **not**. 
Các lệnh này thao tác trên từng bit của hai thanh ghi nguồn và ghi kết quả vào thanh ghi đích.
![pic4_1!](../images/pic4_1.png)

Các lệnh **and**, **or** và **xor** cũng có phiên bản immediate, lệnh sử dụng một thanh ghi nguồn và một số nguyên có dấu 12-bit.
Một số ứng dụng của các lệnh logic:
1. Lệnh **and** thường được sử dụng để trích xuất một hoặc nhiều bit trong thanh ghi.
2. Lệnh **and** thường được sử dụng để xóa một hoặc nhiều bit.
3. Lệnh **or** thường được sử dụng thiết lập một hoặc nhiều bit.

Chương trình dưới đây minh họa cách trích xuất nội dung thanh ghi sử dụng các lệnh logic. Như đã nói, ta có thể trích xuất 1 bit hoặc nhiều bit có trong thanh ghi. Hãy đọc kỹ ví dụ sau đây để hiểu rõ từng dòng lệnh:
```
# Laboratory Exercise 4, Home Assignment 2
.text
   li    s0, 0x12345678 # Test value
   andi  t0, s0, 0xff   # Extract LSB of s0
   andi  t1, s0, 0x0400 # Extract bit 10 of s0
```

### Home Assignment 3 

Loại lệnh tiếp theo được xét trong bài này là lệnh dịch bit (shift instruction). Lệnh này dịch toàn bộ 32-bit của thanh ghi sang trái hoặc sang phải. Các lệnh dịch bit bao gồm **sll** (shift left logical), **srl** (shift right logical), and **sra** (shift right arithmetic). Đối với lệnh shift right sẽ có hai phiên bản đó là logical (gán tất cả bit mới bằng 0) và arithmetic (gán tất cả các bit mới bằng bit dấu).
![pic4_2!](../images/pic4_2.png)

RISC-V có hỗ trợ phiên bản immediate (**slli**, **srli**, and **srai**), với 5-bit hằng số.
Chú ý rằng, khi dịch trái (phải) N-bit sẽ tương đương với nhân (chia) với chính nó 2^N nếu kết quả không vượt quá giới hạn 32-bit. 
```
# Laboratory Exercise 3, Home Assignment 3
.text
   li    s0, 1       # s0 = 1
   sll   s1, s0, 2   # s1 = s0 * 4
```

---

## Bài thực hành
 
## Assignment 1

Tạo project để thực hiện chương trình ở Home Assignment. Dịch và chạy mô phỏng với RARS. Khởi tạo các toán hạng cần thiết, chạy từng lệnh của chương trình, quan sát bộ nhớ và giá trị thanh ghi.

## Assignment 2

Viết một chương trình thực hiện các công việc sau:
1. Trích xuất MSB của thanh ghi **s0**
2. Xóa LSB của thanh ghi **s0**
3. Thiết lập LSB của thanh ghi **s0** (bit 7 đến bit 0 được thiết lập bằng 1)
4. Xóa thanh ghi **s0** bằng cách dùng các lệnh logic (**s0** = 0)
MSB: Most Significant Byte (Byte có trọng số cao)
LSB: Least Significant Byte (Byte có trọng số thấp)
![pic4_3!](../images/pic4_3.png)

### Assignment 3

Như đã đề cập, giả lệnh không phải lệnh chính thống của RISC-V, khi biên dịch assembler sẽ chuyển chúng thành các lệnh chính thống. Viết chương trình thực thi các giả lệnh dưới đây sử dụng các lệnh chính thống mà RISC-V định nghĩa:
1. abs   s0, s1
=>	s0 = abs($s1)
2. move  s0, s1
=>	s0 = s1
3. not   s0
=>	s0 = bit_invert(s0)
4. ble   s1, s2, label
=>	if (s1 <= s2) 
		j label

### Assignment 4

Để xác định tràn số xảy ra khi thực hiện phép cộng, có một cách đơn giản hơn so với cách được mô tả trong Home Assignment 1. Giải thuật được mô tả như sau: Khi cộng hai toán hạng cùng dấu, tràn số xảy ra nếu tổng của chúng không cùng dấu với hai toán hạng nguồn. Hãy viết chương trình xác định tràn số theo giải thuật trên.

### Assignment 5

Viết chương trình thực hiện nhân với một lũy thừa của 2 (2, 4, 8, 16, …) mà không sử dụng lệnh nhân.
Ứng dụng phép dịch bit để thực hiện phép nhân có lợi gì so với sử dụng các lệnh nhân trong extension M (RV32M: RISC-V 32-bit Multiplier/Divider).

--- 

## Kết luận
