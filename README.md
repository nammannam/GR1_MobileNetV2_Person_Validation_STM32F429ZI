# MobileNetV2 Person Image Classification

### Giảng viên hướng dẫn
- **Họ và tên**: TS.Đỗ Công Thuần

### Sinh viên thực hiện
- **Họ và tên**: Nguyễn Khánh Nam
- **Mã số sinh viên**: 20225749
- **Lớp**: Việt Nhật 03 - K67

---

Dự án này triển khai mô hình MobileNetV2 để phát hiện người trên vi điều khiển STM32F429ZI sử dụng X-CUBE-AI framework.

## 📋 Mô tả

Dự án sử dụng mô hình MobileNetV2 đã được quantized (INT8) với kích thước đầu vào 128x128x3 để thực hiện inference trên STM32F429ZI Discovery board. Mô hình được tối ưu hóa cho RAM với dung lượng:
- **Model weights**: 401.85 KiB
- **Activations**: 260.05 KiB  
- **Total RAM**: 308.05 KiB

Mô tả chi tiết hơn được trình bày trong báo cáo.

## 🛠️ Yêu cầu hệ thống

### Phần mềm
- **STM32CubeIDE 1.18.0** hoặc mới hơn
- **X-CUBE-AI 10.1.0** (ST Edge AI Core v2.1.0)
- **STM32CubeMX** (tích hợp trong STM32CubeIDE)

### Phần cứng
- **STM32F429I-DISC1** Discovery board
- Cable USB Type-A to Micro-B
- Máy tính Windows/Linux/macOS

## 🚀 Hướng dẫn cài đặt và chạy

### Bước 1: Cài đặt phần mềm

1. **Tải và cài đặt STM32CubeIDE 1.18.0**:
   - Truy cập [st.com](https://www.st.com/en/development-tools/stm32cubeide.html)
   - Tải version 1.18.0 cho hệ điều hành của bạn
   - Cài đặt theo hướng dẫn

2. **Cài đặt X-CUBE-AI 10.1.0**:
   - Mở STM32CubeIDE
   - Vào `Help` → `Manage Embedded Software Packages`
   - Trong tab `STMicroelectronics`, tìm và cài đặt `X-CUBE-AI` version 10.1.0

### Bước 2: Import dự án

1. **Clone hoặc tải dự án**:
   ```bash
   git clone <repository-url>
   ```

2. **Import vào STM32CubeIDE**:
   - Mở STM32CubeIDE
   - `File` → `Import` → `General` → `Existing Projects into Workspace`
   - Chọn folder `STM32F429I-DISC1`
   - Click `Finish`

### Bước 3: Cấu hình dự án

1. **Kiểm tra cấu hình X-CUBE-AI**:
   - Mở file `STM32F429I-DISC1.ioc`
   - Trong tab `Pinout & Configuration`, kiểm tra `Software Packs` → `X-CUBE-AI`
   - Đảm bảo model `mobilenet_v2_0.35_128_fft_int8.tflite` đã được load

2. **Generate code** (nếu cần):
   - Click `GENERATE CODE` trong STM32CubeMX
   - Chọn `Yes` để generate code

### Bước 4: Build dự án

1. **Clean và Build**:
   - Right-click vào project → `Clean Project`
   - Right-click vào project → `Build Project`
   - Hoặc sử dụng `Ctrl+B`

2. **Kiểm tra build thành công**:
   - Console sẽ hiển thị `Build Finished`
   - File `.elf` sẽ được tạo trong folder `Debug/` hoặc `Release/`

### Bước 5: Flash và Debug

1. **Kết nối board**:
   - Kết nối STM32F429I-DISC1 với máy tính qua USB
   - Đảm bảo driver ST-Link đã được cài đặt

2. **Flash firmware**:
   - Right-click vào project → `Run As` → `STM32 C/C++ Application`
   - Hoặc click nút `Run` (▶️) trên toolbar

3. **Debug** (tùy chọn):
   - Right-click vào project → `Debug As` → `STM32 C/C++ Application`
   - Sử dụng breakpoints để debug code

## 📁 Cấu trúc dự án

```
STM32F429I-DISC1/
├── Core/                           # Core application files
│   ├── Inc/                        # Header files
│   │   ├── main.h                  # Main application header
│   │   ├── stm32f4xx_hal_conf.h    # HAL configuration
│   │   └── stm32f4xx_it.h          # Interrupt handlers header
│   ├── Src/                        # Source files
│   │   ├── main.c                  # Main application
│   │   ├── stm32f4xx_hal_msp.c     # HAL MSP functions
│   │   ├── stm32f4xx_it.c          # Interrupt handlers
│   │   └── system_stm32f4xx.c      # System initialization
│   └── Startup/                    # Startup assembly file
├── X-CUBE-AI/                      # AI-generated files
│   ├── App/                        # AI application layer
│   │   ├── network.c               # Generated AI network
│   │   ├── network_data.c          # Network weights/biases
│   │   ├── app_x-cube-ai.c         # AI application main
│   │   └── ai*.c/h                 # Various AI helper files
│   └── Target/                     # Platform-specific files
├── Drivers/                        # STM32 HAL drivers
├── Debug/                          # Debug build output
├── Release/                        # Release build output
├── mobilenet_v2_0.35_128_fft_int8.tflite  # Original TensorFlow Lite model
└── STM32F429I-DISC1.ioc           # STM32CubeMX configuration
```

## 🔧 Thông số kỹ thuật

### Model thông tin
- **Model**: MobileNetV2 (0.35 width multiplier)
- **Input size**: 128×128×3 (RGB image)
- **Output**: 1 float value (person detection score)
- **Quantization**: INT8
- **Framework**: TensorFlow Lite

### Memory usage
- **Flash (Model weights)**: 401.85 KiB
- **RAM (Activations)**: 260.05 KiB
- **Total RAM**: 308.05 KiB
- **MACC operations**: 19,094,660

### MCU specifications
- **Chip**: STM32F429ZIT6
- **Core**: ARM Cortex-M4F @ 180MHz
- **Flash**: 2MB
- **RAM**: 256KB + 64KB CCM
- **Package**: LQFP144

## 🐛 Troubleshooting

### Lỗi thường gặp

1. **Build error: "X-CUBE-AI not found"**
   - Đảm bảo X-CUBE-AI 10.1.0 đã được cài đặt
   - Regenerate code từ STM32CubeMX

2. **Memory overflow error**
   - Kiểm tra linker script (*.ld files)
   - Tối ưu hóa model hoặc giảm buffer size

3. **ST-Link connection error**
   - Cập nhật ST-Link driver
   - Kiểm tra cable USB và connection

4. **Model inference error**
   - Kiểm tra input data format (uint8, range 0-255)
   - Verify model đã được load đúng

### Debug tips

1. **Sử dụng UART để debug**:
   - USART1 đã được cấu hình
   - Sử dụng `printf()` để debug thông tin

2. **Monitor memory usage**:
   - Kiểm tra stack và heap usage
   - Sử dụng Memory Protection Unit nếu cần

3. **Performance profiling**:
   - Sử dụng SysTick timer để đo thời gian inference
   - Monitor CPU load

## 📚 Tài liệu tham khảo

- [STM32F429 Reference Manual](https://www.st.com/resource/en/reference_manual/rm0090-stm32f405415-stm32f407417-stm32f427437-and-stm32f429439-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
- [X-CUBE-AI User Manual](https://www.st.com/resource/en/user_manual/um2526-getting-started-with-xcubeai-expansion-package-for-artificial-intelligence-ai-stmicroelectronics.pdf)
- [STM32CubeIDE User Guide](https://www.st.com/resource/en/user_manual/um2609-stm32cubeide-user-guide-stmicroelectronics.pdf)
- [MobileNetV2 Paper](https://arxiv.org/abs/1801.04381)

## 📄 License

Dự án này tuân theo các license:
- STMicroelectronics Software License (xem `LICENSE.pdf`)
- X-CUBE-AI License (xem `LICENSE_X-CUBE-AI.txt`)

