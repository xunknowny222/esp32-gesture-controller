# ESP32-S3 手势识别控制器 - Day 2 测试报告

**测试日期**: 2026-05-28  
**测试人**: 组员 A  
**开发板型号**: ESP32-S3-EYE v2.2  
**测试地点**: 实验室

---

## 📋 测试目标

验证 ESP32-S3-EYE 开发板基本功能，确保硬件正常工作。

---

## ✅ 测试项目

### 1. 开箱检查

| 检查项 | 预期结果 | 实际结果 | 状态 |
|--------|---------|---------|------|
| 开发板外观 | 无损坏 | 完好无损 | ✅ |
| 摄像头排线 | 插紧 | 已插紧 | ✅ |
| LCD 屏幕 | 无裂痕 | 完好 | ✅ |
| USB 接口 | 无松动 | 正常 | ✅ |
| 配件清单 | 开发板×1 | 符合 | ✅ |

**照片证据**：
- 开发板正面照片 ✅
- 开发板背面照片 ✅

---

### 2. LED 闪烁测试

**测试代码**：
```cpp
#define LED_PIN 48

void setup() {
  Serial.begin(115200);
  pinMode(LED_PIN, OUTPUT);
  Serial.println("LED 测试开始...");
}

void loop() {
  digitalWrite(LED_PIN, HIGH);
  Serial.println("LED ON");
  delay(500);
  digitalWrite(LED_PIN, LOW);
  Serial.println("LED OFF");
  delay(500);
}
```

**测试结果**：

| 测试项 | 预期 | 实际 | 状态 |
|--------|------|------|------|
| LED 引脚 | GPIO 48 | GPIO 48 | ✅ |
| LED 闪烁 | 1 秒间隔 | 正常闪烁 | ✅ |
| 串口输出 | 有输出 | 正常输出 | ✅ |

**串口输出示例**：
```
LED 测试开始...
LED ON
LED OFF
LED ON
LED OFF
...
```

**结论**: ✅ LED 测试通过

---

### 3. 串口通信测试

**测试配置**：
- 波特率：115200
- 数据位：8
- 停止位：1
- 校验位：无

**测试结果**：

| 测试项 | 预期 | 实际 | 状态 |
|--------|------|------|------|
| 串口连接 | 成功 | 成功 | ✅ |
| 波特率 | 115200 | 正常 | ✅ |
| 数据接收 | 无乱码 | 正常 | ✅ |

**结论**: ✅ 串口测试通过

---

### 4. 摄像头初始化测试

**测试代码**：
```cpp
#include <esp_camera.h>

// 摄像头引脚定义（ESP32-S3-EYE）
#define PWDN_GPIO_NUM     -1
#define RESET_GPIO_NUM    -1
#define XCLK_GPIO_NUM     15
#define SIOD_GPIO_NUM     4
#define SIOC_GPIO_NUM     5
#define Y9_GPIO_NUM       16
#define Y8_GPIO_NUM       17
#define Y7_GPIO_NUM       18
#define Y6_GPIO_NUM       12
#define Y5_GPIO_NUM       10
#define Y4_GPIO_NUM       8
#define Y3_GPIO_NUM       9
#define Y2_GPIO_NUM       11
#define VSYNC_GPIO_NUM    6
#define HREF_GPIO_NUM     7
#define PCLK_GPIO_NUM     13

void setup() {
  Serial.begin(115200);
  
  camera_config_t config;
  config.ledc_channel = LEDC_CHANNEL_0;
  config.ledc_timer = LEDC_TIMER_0;
  config.pin_d0 = Y2_GPIO_NUM;
  config.pin_d1 = Y3_GPIO_NUM;
  config.pin_d2 = Y4_GPIO_NUM;
  config.pin_d3 = Y5_GPIO_NUM;
  config.pin_d4 = Y6_GPIO_NUM;
  config.pin_d5 = Y7_GPIO_NUM;
  config.pin_d6 = Y8_GPIO_NUM;
  config.pin_d7 = Y9_GPIO_NUM;
  config.pin_xclk = XCLK_GPIO_NUM;
  config.pin_pclk = PCLK_GPIO_NUM;
  config.pin_vsync = VSYNC_GPIO_NUM;
  config.pin_href = HREF_GPIO_NUM;
  config.pin_sscb_sda = SIOD_GPIO_NUM;
  config.pin_sscb_scl = SIOC_GPIO_NUM;
  config.pin_pwdn = PWDN_GPIO_NUM;
  config.pin_reset = RESET_GPIO_NUM;
  config.xclk_freq_hz = 10000000;
  config.frame_size = FRAMESIZE_QQVGA;
  config.pixel_format = PIXFORMAT_GRAYSCALE;
  config.grab_mode = CAMERA_GRAB_WHEN_EMPTY;
  config.fb_location = CAMERA_FB_IN_PSRAM;
  config.fb_count = 1;
  
  esp_err_t err = esp_camera_init(&config);
  if (err != ESP_OK) {
    Serial.printf("摄像头初始化失败：0x%x\n", err);
    return;
  }
  
  Serial.println("✓ 摄像头初始化成功！");
}
```

**测试结果**：

| 测试项 | 预期 | 实际 | 状态 |
|--------|------|------|------|
| 摄像头初始化 | 成功 | 成功 | ✅ |
| PSRAM 配置 | OPI PSRAM | 已启用 | ✅ |
| 图像采集 | 可获取 | 成功 | ✅ |
| 图像尺寸 | 160×120 | 正确 | ✅ |
| 图像格式 | 灰度图 | 正确 | ✅ |

**串口输出示例**：
```
================================
ESP32-S3-EYE 摄像头测试
================================
✓ 摄像头初始化成功！
正在获取图像 #1...
✓ 获取图像成功
  - 尺寸：160 x 120
  - 大小：19200 字节

正在获取图像 #2...
✓ 获取图像成功
  - 尺寸：160 x 120
  - 大小：19200 字节
```

**结论**: ✅ 摄像头测试通过

---

### 5. LCD 屏幕测试

**观察结果**：

| 测试项 | 预期 | 实际 | 状态 |
|--------|------|------|------|
| 屏幕点亮 | 亮起 | 正常亮起 | ✅ |
| 显示内容 | 清晰 | 清晰 | ✅ |
| 色彩 | 正常 | 正常 | ✅ |

**结论**: ✅ LCD 屏幕测试通过

---

## 📊 测试结果汇总

| 测试项目 | 状态 | 备注 |
|---------|------|------|
| 开箱检查 | ✅ 通过 | 硬件完好 |
| LED 闪烁 | ✅ 通过 | GPIO 48 正常 |
| 串口通信 | ✅ 通过 | 115200 波特率 |
| 摄像头初始化 | ✅ 通过 | PSRAM 已启用 |
| 图像采集 | ✅ 通过 | 160×120 灰度图 |
| LCD 屏幕 | ✅ 通过 | 显示正常 |

**总体结论**: ✅ **所有测试通过，开发板功能正常**

---

## ⚠️ 遇到的问题

### 问题 1: 摄像头 FB-OVF 错误

**现象**：
```
frame buffer malloc failed
FB-OVF
```

**原因**：
- 初始分辨率设置过高（QVGA 320×240）
- PSRAM 配置不正确

**解决方案**：
1. 降低分辨率到 QQVGA (160×120)
2. 使用灰度图格式（PIXFORMAT_GRAYSCALE）
3. PSRAM 选择 OPI PSRAM

**结果**: ✅ 已解决

### 问题 2: 串口无输出

**现象**：上传成功后串口监视器无显示

**原因**：
- 波特率不匹配
- 串口监视器未正确打开

**解决方案**：
1. 确认波特率为 115200
2. 上传后按 RESET 键
3. 重新打开串口监视器

**结果**: ✅ 已解决

---

## 📸 测试照片

| 照片名称 | 说明 |
|---------|------|
| 开发板正面.jpg | 开发板正面全貌 |
| 开发板背面.jpg | 开发板背面元件 |
| LED 闪烁.jpg | LED 闪烁测试 |
| 串口输出.png | 串口监视器截图 |
| 摄像头测试.png | 摄像头测试输出 |

---

## 🎯 下一步建议

1. ✅ 开发板验证完成，可以开始后续开发
2. 继续 Day 3 任务：ESP-IDF 环境搭建
3. 准备摄像头驱动验证代码

---

## 📝 测试人员签字

**测试人**: ___________  
**审核人**: ___________  
**日期**: 2026-05-28

---

**文档版本**: v1.0  
**创建时间**: 2026-05-28  
**归档位置**: `docs/Day2_测试报告.md`
