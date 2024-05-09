---
layout: page
title: Sử dụng Visual Studio Code và PlatformIO extension để lập trình hợp ngữ và gỡ lỗi trên ESP32-C3
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
