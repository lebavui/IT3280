---
layout: page
title: Sử dụng VS Code và PlatformIO để lập trình hợp ngữ và gỡ lỗi
permalink: /huong-dan-vscode/
nav_order: 20
---

# Hướng dẫn sử dụng Visual Studio Code và PlatformIO extension để lập trình hợp ngữ và gỡ lỗi trên ESP32-C3

- Cài đặt VS Code và PlatformIO extension
- Tạo project lập trình hợp ngữ
- Dịch và nạp chương trình vào kit
- Cách thực hiện chế độ gỡ lỗi với kit tích hợp USB JTAG
    - Cài đặt driver cho USB JTAG
        - Cài đặt phần mềm Zadig (Windows) https://zadig.akeo.ie/#
        - Cài driver libusbK cho USB JTAG
    - Trong VS Code, mở file platformio.ini của project, thêm dòng sau
        ```
        debug_tool = esp-builtin
        ```
    - Trong VS Code nhấn vào **Run and Debug**, sau đó chọn **Start Debugging**
    - Các thao tác có thể thực hiện khi chạy ở chế độ gỡ lỗi
        - Chạy từng lệnh hợp ngữ. **Chú ý: Các lệnh hợp ngữ có thể được dịch thành lệnh 32-bit hoặc lệnh nén 16-bit**
        - Theo dõi giá trị các thanh ghi
        - Quan sát một vùng nhớ trong bộ nhớ

**Chú ý:**
Cấu hình mặc định sử dụng kiến trúc tập lệnh **rv32imc** nên các lệnh có thể được dịch thành mã máy 16-bit để tối ưu bộ nhớ.
Để cấu hình chỉ dịch thành mã máy 32-bit, mở file **platformio-build-esp32c3.py** trong thư mục "C:\Users\YOUR_USERNAME\\.platformio\packages\framework-arduinoespressif32\tools", rồi chuyển các tham số "-march=rv32imc" thành "-march=rv32im".