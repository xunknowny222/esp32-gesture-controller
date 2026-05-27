# ESP32-S3 手势识别控制器

<div align="center">

![ESP32-S3](https://img.shields.io/badge/ESP32-S3-WROOM-1-blue)
![Platform](https://img.shields.io/badge/Platform-Arduino%20%7C%20ESP--IDF-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

**基于 ESP32-S3-EYE 的便携式手势识别控制器**

*全国大学生嵌入式芯片与系统设计竞赛参赛作品*

</div>

---

## 📖 项目简介

本项目是一款基于 **ESP32-S3-EYE** 开发板的便携式手势识别控制器，通过摄像头采集手势图像，利用深度学习模型进行实时识别，并通过 Wi-Fi/MQTT 协议控制智能家居设备。

### ✨ 核心特性

- 🖐 **6 种手势识别**：握拳、张开、左滑、右滑、上滑、下滑
- 📷 **实时图像采集**：OV2640 摄像头，支持 QVGA 分辨率
- 🧠 **边缘 AI 推理**：ESP-DL 深度学习框架，单次推理 <300ms
- 📡 **Wi-Fi/MQTT 通信**：可接入 HomeAssistant 等智能家居平台
- 🔋 **便携设计**：锂电池供电，续航 >4 小时
- 📱 **OLED 状态显示**：实时显示识别结果和系统状态

---

## 🎯 应用场景

| 场景 | 功能描述 |
|------|---------|
| 🏠 **智能家居控制** | 手势控制灯光、空调、电视等设备 |
| 🎮 **游戏交互** | 体感游戏手势输入 |
| ♿ **无障碍辅助** | 帮助行动不便人士控制设备 |
| 🎓 **教育演示** | AIoT 教学、嵌入式 AI 实践 |

---

## 🛠️ 硬件清单

### 核心器件

| 器件 | 型号 | 数量 | 备注 |
|------|------|------|------|
| 主控板 | ESP32-S3-EYE | 1 | 集成 OV2640 摄像头 |
| 显示屏 | 0.96" OLED I2C | 1 | 可选，状态显示 |
| 电池 | 3.7V 500mAh 锂电 | 1 | 便携供电 |
| 充电模块 | TP4056 | 1 | 锂电池充电管理 |

### 可选外设

| 器件 | 用途 |
|------|------|
| 红外发射管 | 控制传统家电（电视、空调） |
| 轻触开关 | 电源开关、复位按钮 |
| LED 指示灯 | 系统状态指示 |

---

## 📦 快速开始

### 1. 环境准备

#### Arduino IDE（推荐新手）

1. **下载 Arduino IDE**：https://www.arduino.cc/en/software

2. **安装 ESP32 开发板支持**：
   - 打开 `文件` → `首选项`
   - 在「其他开发板管理器地址」添加：
     ```
     https://espressif.github.io/arduino-esp32/package_esp32_index.json
     ```
   - 打开 `工具` → `开发板` → `开发板管理器`
   - 搜索 `esp32`，安装 **esp32 by Espressif Systems**

3. **安装依赖库**：
   - `项目` → `加载库` → `管理库`
   - 搜索并安装：
     - `esp32-camera` (by Espressif)
     - `PubSubClient` (by Nick O'Leary)
     - `TFT_eSPI` (可选，用于 OLED)

#### ESP-IDF（推荐进阶）

```bash
# 安装 ESP-IDF v5.0+
git clone -b v5.0 --recursive https://github.com/espressif/esp-idf.git
cd esp-idf
./install.sh
. ./export.sh

# 编译项目
cd firmware
idf.py set-target esp32s3
idf.py build
idf.py flash
```

---

### 2. 硬件连接

#### ESP32-S3-EYE 引脚定义

ESP32-S3-EYE 已集成摄像头，无需额外接线。如需连接 OLED：

| OLED | ESP32-S3-EYE |
|------|-------------|
| VCC | 3.3V |
| GND | GND |
| SCL | GPIO 15 |
| SDA | GPIO 4 |

#### 红外发射模块（可选）

| 红外 LED | ESP32-S3-EYE |
|---------|-------------|
| 正极（长脚） | GPIO 18（通过三极管驱动） |
| 负极（短脚） | GND |

---

### 3. 编译与上传

1. **选择开发板**：
   - `工具` → `开发板` → `ESP32S3 Dev Module`

2. **配置参数**：
   - `USB CDC On Boot`: **Enabled**
   - `CPU Frequency`: **240MHz**
   - `Flash Size`: **8MB**
   - `PSRAM`: **Enabled**

3. **选择端口**：
   - `工具` → `Port` → 选择对应的 COM 口

4. **上传代码**：
   - 点击 `→` 按钮编译并上传
   - 首次上传可能需要按住 **BOOT** 键再按 **RESET** 键

5. **打开串口监视器**：
   - 波特率：**115200**
   - 查看系统日志和识别结果

---

## 📁 项目结构

```
esp32-gesture-controller/
├── README.md                    # 项目说明（本文件）
├── firmware/                    # 固件代码
│   ├── main.ino                # Arduino 主程序
│   ├── camera_driver.c         # 摄像头驱动
│   ├── gesture_model.cpp       # 手势识别模型
│   ├── wifi_mqtt.c             # Wi-Fi/MQTT 通信
│   └── oled_display.cpp        # OLED 显示驱动
├── hardware/                    # 硬件资料
│   ├── schematic/              # 原理图
│   ├── connections.md          # 接线说明
│   └── bom.xlsx                # 器件清单
├── docs/                        # 技术文档
│   ├── 技术方案.md              # 完整技术方案
│   ├── 系统架构图.pdf           # 系统架构
│   ├── 测试报告.md              # 功能测试报告
│   └── 用户手册.md              # 使用说明
├── media/                       # 媒体文件
│   ├── 演示视频.mp4            # 比赛演示视频
│   └── 照片/                    # 项目照片
├── .gitignore                   # Git 忽略文件
└── platformio.ini               # PlatformIO 配置（可选）
```

---

## 🧠 手势识别模型

### 支持的手势

| 手势 ID | 手势名称 | 控制功能 |
|--------|---------|---------|
| 0 | 👊 握拳 | 关闭所有设备 |
| 1 | ✋ 张开 | 打开所有设备 |
| 2 | ⬅️ 左滑 | 亮度 - / 音量 - |
| 3 | ➡️ 右滑 | 亮度 + / 音量 + |
| 4 | ⬆️ 上滑 | 上一个设备 |
| 5 | ⬇️ 下滑 | 下一个设备 |

### 模型性能

| 指标 | 数值 |
|------|------|
| 识别准确率 | >85% |
| 单次推理时间 | <300ms |
| 模型大小 | ~200KB |
| 内存占用 | ~500KB |

### 模型部署

使用 **ESP-DL** 框架部署预训练的 MobileNetV1 模型：

```cpp
#include <esp_dl.h>
#include <dl_model.h>

// 加载模型
model = dl_model_load("/spiffs/gesture_model.tflite");

// 推理
dl_input_set(image_buffer);
dl_output_t *output = dl_model_invoke(model);

// 解析结果
int gesture_id = parse_result(output);
```

---

## 📡 MQTT 通信

### 连接配置

在 `wifi_mqtt.c` 中修改以下配置：

```cpp
#define WIFI_SSID "你的 WiFi 名称"
#define WIFI_PASSWORD "你的 WiFi 密码"
#define MQTT_BROKER "broker.emqx.io"  // 或使用自建 MQTT 服务器
#define MQTT_PORT 1883
```

### 主题定义

| 主题 | 方向 | 说明 |
|------|------|------|
| `gesture/command` | 发布 | 手势识别结果 |
| `gesture/control/#` | 订阅 | 控制命令 |

### 示例消息

**发布**（设备 → 服务器）：
```json
{
  "gesture": "fist",
  "timestamp": 1234567890,
  "confidence": 0.92
}
```

**订阅**（服务器 → 设备）：
```json
{
  "device": "light_01",
  "action": "toggle",
  "value": 1
}
```

---

## 🔧 常见问题

### Q1: 编译失败，提示找不到 `esp_camera.h`
**解决**：确保已安装 `esp32-camera` 库，并在 `项目` → `加载库` 中启用。

### Q2: 上传失败，提示 `Failed to connect to ESP32-S3`
**解决**：
1. 检查 USB 线是否支持数据传输
2. 按住 BOOT 键，再按 RESET 键，然后上传
3. 检查 COM 端口是否正确

### Q3: 摄像头无法初始化
**解决**：
1. 检查摄像头排线是否插紧
2. 确认引脚定义与代码一致
3. 尝试降低 XCLK 频率（20MHz → 10MHz）

### Q4: 手势识别准确率低
**解决**：
1. 确保光线充足
2. 手势在摄像头中心区域
3. 距离摄像头 20-40cm
4. 重新采集数据集并微调模型

### Q5: Wi-Fi 连接不稳定
**解决**：
1. 检查 WiFi 信号强度
2. 添加断线重连机制
3. 使用 2.4GHz 频段（ESP32 不支持 5GHz）

---

## 👥 团队成员

| 角色 | 姓名 | 负责内容 |
|------|------|---------|
| 组长 | [你的名字] | 项目管理、系统集成、文档/视频 |
| 组员 A | [姓名] | 硬件设计、结构、采购 |
| 组员 B | [姓名] | 软件开发、算法、通信 |

---

## 📅 开发日志

| 日期 | 进度 | 备注 |
|------|------|------|
| 2026-05-27 | Day 1 | 项目启动，采购清单确认，GitHub 仓库创建 |
| 2026-05-28 | Day 2 | 开发板开箱测试，ESP-IDF 环境搭建 |
| 2026-05-29 | Day 3 | 摄像头驱动验证 |
| ... | ... | ... |

---

## 📄 许可证

本项目采用 **MIT License** 开源协议。

```
Copyright (c) 2026 ESP32 Gesture Controller Team

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

---

## 🙏 致谢

- **乐鑫 (Espressif)**：提供 ESP32-S3-EYE 开发板和技术支持
- **ESP-DL 团队**：提供深度学习框架
- **HomeAssistant 社区**：智能家居平台支持

---

## 📬 联系方式

- 📧 Email: [你的邮箱]
- 💬 问题反馈：请在 GitHub Issues 中提出
- 📺 演示视频：[B 站链接]（待更新）

---

<div align="center">

**如果这个项目对你有帮助，请给一个 ⭐ Star！**

*28 天，全力以赴，使命必达！* 🚀

</div>
