# FreeOS Desktop 技术手册

## 项目概述

**FreeOS Desktop** 是一个基于 FreeRTOS 实时操作系统的嵌入式桌面操作系统，运行在正点原子 ATK Apollo STM32H743 开发板上。项目采用安卓风格的桌面交互设计，支持多应用图标显示、触摸交互、应用生命周期管理，并集成了多种硬件外设。

**目标平台**: ATK Apollo STM32H743 (ARM Cortex-M7, 400MHz)  
**IDE**: Keil MDK-ARM  
**构建方式**: 仅通过 Keil IDE 构建

---

## 手册目录

### [第一部分：项目概述与系统启动](01-项目概述与系统启动.md)

- 项目简介与核心特性
- 目录结构详解
- 系统启动序列（硬件初始化 → FreeRTOS 启动）
- FreeRTOS 配置详解（`FreeRTOSConfig.h`）
- 静态内存分配机制
- 6 个独立内存池管理

**关键文件**: [`User/main.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/main.c), [`User/FreeRTOSConfig.h`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/FreeRTOSConfig.h), [`User/freertos_demo.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/freertos_demo.c)

---

### [第二部分：日志系统与应用框架](02-日志系统与应用框架.md)

- ISR 安全环形缓冲区日志系统（`ring_log.c`）
- LED 心跳指示与错误处理（`sys_panic()`）
- 统一应用接口（`app_interface_t`）
- 应用生命周期管理
- 线程安全的应用管理器（互斥量保护）
- 桌面双任务架构（desktop_task + touch_task）

**关键文件**: [`User/ring_log.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/ring_log.c), [`User/APP/app_interface.h`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/APP/app_interface.h), [`User/APP/app_manager.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/APP/app_manager.c), [`User/APP/desktop_app.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/APP/desktop_app.c)

---

### [第三部分：渲染系统与 UI 组件](03-渲染系统与UI组件.md)

- 渲染命令机制（`rcmd_xxx`）
- 批量绘制与字符串池管理
- 数据项组件（`ui_data_item`）与局部刷新
- 数据行分组组件（`ui_data_row_group`）
- 按钮组件（`ui_button`）
- 内存池管理（6 个独立内存池）

**关键文件**: [`Middlewares/RENDER/render_cmd.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Middlewares/RENDER/render_cmd.c), [`User/APP/Components/ui_data_row_group.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/APP/Components/ui_data_row_group.c), [`User/APP/Components/ui_button.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/APP/Components/ui_button.c), [`Middlewares/MALLOC/malloc.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Middlewares/MALLOC/malloc.c)

---

### [第四部分：现有应用与开发规范](04-现有应用与开发规范.md)

- 传感器应用（`sensor_app`）
- 音乐播放器（`music_app`）
- 相机应用（`camera_app`）
- 文件管理器（`file_manager`）
- 应用开发规范
- 最小可运行示例
- 调试指南

**关键文件**: [`User/APP/sensor_app/sensor_app.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/APP/sensor_app/sensor_app.c), [`User/APP/music_player/music_app.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/APP/music_player/music_app.c), [`User/APP/camera_app/camera_app.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/APP/camera_app/camera_app.c), [`User/APP/file_manager/file_manager_app.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/APP/file_manager/file_manager_app.c), [`User/APP/APP开发规范.md`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/APP/APP开发规范.md)

---

### [第五部分：桌面系统与通用组件](05-桌面系统与通用组件.md)

- 桌面系统（渐变壁纸、状态栏、导航栏、图标网格）
- 自动网格布局算法
- 时钟显示优化（仅变化时重绘）
- 标题栏组件（`titlebar`）
- 开机画面（不依赖外部字库）
- 字符串集中管理（`app_strings.h`）

**关键文件**: [`Middlewares/GUI/desktop.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Middlewares/GUI/desktop.c), [`User/APP/MODELS/titlebar.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/APP/MODELS/titlebar.c), [`User/startup_screen.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/startup_screen.c), [`User/app_strings.h`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/User/app_strings.h)

---

### [第六部分：硬件驱动层详解](06-硬件驱动层详解.md)

- LCD 显示驱动（MCU 接口 + LTDC RGB 接口）
- 触摸屏驱动（电阻屏 + FT5206/GT9XXX 电容屏）
- IMU 传感器驱动 SH3001（加速度计 + 陀螺仪）
- 磁力计驱动 ST480MC
- 摄像头驱动 OV5640 + DCMI 接口
- 音频编解码器驱动 ES8388
- SDRAM 驱动与帧缓冲管理
- NORFlash 驱动（QSPI 模式）
- MPU 内存保护配置

**关键文件**: [`Drivers/BSP/LCD/lcd.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Drivers/BSP/LCD/lcd.c), [`Drivers/BSP/LCD/ltdc.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Drivers/BSP/LCD/ltdc.c), [`Drivers/BSP/TOUCH/ft5206.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Drivers/BSP/TOUCH/ft5206.c), [`Drivers/BSP/SH3001/sh3001.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Drivers/BSP/SH3001/sh3001.c), [`Drivers/BSP/ST480MC/st480mc.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Drivers/BSP/ST480MC/st480mc.c), [`Drivers/BSP/OV5640/ov5640.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Drivers/BSP/OV5640/ov5640.c), [`Drivers/BSP/DCMI/dcmi.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Drivers/BSP/DCMI/dcmi.c), [`Drivers/BSP/ES8388/es8388.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Drivers/BSP/ES8388/es8388.c), [`Drivers/BSP/SDRAM/sdram.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Drivers/BSP/SDRAM/sdram.c), [`Drivers/BSP/NORFLASH/norflash.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Drivers/BSP/NORFLASH/norflash.c), [`Drivers/BSP/MPU/mpu.c`](file:///g:/codeversion/h732worktask/基于FreeRTOS的作业实现/Drivers/BSP/MPU/mpu.c)

---

## 快速参考

### 核心架构图

```
┌─────────────────────────────────────────────────────────────┐
│                        应用层                                │
│  sensor_app  music_app  camera_app  file_manager  ...       │
├─────────────────────────────────────────────────────────────┤
│                      应用框架层                              │
│  app_interface_t  app_manager  app_registry                 │
├─────────────────────────────────────────────────────────────┤
│                      桌面系统层                              │
│  desktop_app (desktop_task + touch_task)  GUI/desktop       │
├─────────────────────────────────────────────────────────────┤
│                      中间件层                                │
│  RENDER  TEXT  PICTURE  MALLOC  FATFS  USB                  │
├─────────────────────────────────────────────────────────────┤
│                      RTOS 层                                 │
│  FreeRTOS (静态内存分配、互斥量、软件定时器)                  │
├─────────────────────────────────────────────────────────────┤
│                      硬件抽象层                              │
│  STM32H7xx HAL Driver                                       │
├─────────────────────────────────────────────────────────────┤
│                      板级驱动层                              │
│  LCD  Touch  SDRAM  NORFlash  SH3001  OV5640  ES8388  ...  │
└─────────────────────────────────────────────────────────────┘
```

### 任务优先级

| 任务 | 优先级 | 周期 | 职责 |
|------|--------|------|------|
| 定时器任务 | 31 | - | 软件定时器服务 |
| 触摸任务 | 3 | 10ms | 触摸扫描、事件分发 |
| 桌面任务 | 2 | 100ms | UI 更新 |
| 日志任务 | 1 | 10ms | 日志刷新、LED 心跳 |
| 空闲任务 | 0 | - | 系统空闲处理 |

### 内存池

| 标识 | 区域 | 容量 |
|------|------|------|
| SRAMIN | AXI SRAM | 448 KB |
| SRAMEX | SDRAM | ~28 MB |
| SRAM12 | SRAM1+SRAM2 | 240 KB |
| SRAM4 | SRAM4 | 60 KB |
| SRAMDTCM | DTCM | 120 KB |
| SRAMITCM | ITCM | 60 KB |

### 关键约束

1. **调度器启动前初始化**：含 `delay_ms()` 的硬件初始化必须放在 `freertos_demo()` 之前
2. **中断优先级**：使用 FreeRTOS API 的中断优先级必须 <= 5
3. **update() 执行时间**：应 < 50ms
4. **rcmd 成对使用**：`rcmd_begin()` 和 `rcmd_flush()` 必须成对出现
5. **触摸返回值**：`handle_touch()` 返回 `0` 触发返回桌面，`0xFF` 表示未处理