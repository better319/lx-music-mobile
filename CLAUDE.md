# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

LX Music 移动版（洛雪音乐助手）是一个基于 React Native 0.73.11 开发的跨平台音乐播放器应用，目前仅支持 Android 5.0+ 平台。

## 常用开发命令

### 开发环境
```bash
npm run dev          # 运行Android开发模式
npm run start        # 启动Metro服务
npm run sc           # 启动Metro服务（清除缓存）
```

### 代码质量检查
```bash
npm run lint         # ESLint代码检查
npm run lint:fix     # ESLint自动修复
```

### 构建打包
```bash
npm run pack:android:debug    # 构建Android调试版
npm run pack:android         # 构建Android发布版（Windows）
cd android && ./gradlew assembleRelease  # 构建Android发布版（Linux/Mac）
npm run clear                # 清理构建缓存
```

### 调试工具
```bash
npm run rd           # 启动React DevTools
npm run menu         # 打开Android开发菜单
```

## 项目架构

### 核心目录结构
- `src/core/` - 核心业务逻辑（播放、音乐资源、搜索等）
- `src/store/` - 状态管理（自定义轻量级状态系统）
- `src/plugins/` - 插件系统（播放器、存储、歌词等）
- `src/screens/` - 页面组件
- `src/components/` - 通用UI组件
- `src/navigation/` - 导航配置
- `src/theme/` - 主题系统
- `src/utils/` - 工具函数

### 状态管理架构
项目使用自定义轻量级状态管理系统，而非Redux：
- 每个状态模块包含 `state.ts` + `action.ts`
- 通过事件总线进行状态变更通知
- 全局状态存储在内存中，非持久化
- 状态分层：应用级、模块级、组件级

### 音乐播放架构
三层播放架构：
1. **插件层** (`src/plugins/player/`) - 基于react-native-track-player封装
2. **核心逻辑层** (`src/core/player/`) - 播放状态管理和列表控制
3. **资源层** (`src/core/music/`) - 在线/本地/下载音乐获取

### 插件系统设计
- 播放器插件：封装原生播放功能
- 存储插件：数据持久化
- 歌词插件：歌词获取和解析
- 同步插件：多端数据同步

## 开发规范

### 代码组织原则
1. **关注点分离**：业务逻辑、UI组件、状态管理分离
2. **模块化**：功能模块化，便于维护和测试
3. **类型安全**：全面的TypeScript类型定义
4. **事件驱动**：使用事件系统进行模块间通信

### 文件命名规范
- `camelCase.ts` - 工具函数和逻辑文件
- `PascalCase.tsx` - React组件文件
- `kebab-case.json` - 配置文件

### 状态管理最佳实践
- 状态集中管理，避免组件间直接传递
- 使用事件系统进行状态变更通知
- 异步操作通过专门的action处理

## 重要注意事项

### 平台限制
- 仅支持 Android 5.0+，无 iOS 支持计划
- React Native 版本锁定在 0.73.11
- Node.js 版本要求 >= 18，npm >= 8.5.2

### 版权合规
- 项目完全免费且开源
- 禁止商业用途
- 需要在24小时内清除使用过程中产生的版权数据
- 数据来源为各音乐平台公开API

### 构建配置
- Android编译SDK版本：35
- 最低SDK版本：21（Android 5.0）
- 目标SDK版本：29
- 构建工具版本：35.0.0

### GitHub Actions
- master分支推送自动构建发布版
- beta分支推送自动构建测试版
- 自动签名和多架构APK生成（arm64-v8a, armeabi-v7a, x86_64, x86, universal）

## 常见问题解决

### 开发环境
1. Metro服务启动失败：使用 `npm run sc` 清除缓存重启
2. Android设备连接问题：检查USB调试和ADB驱动
3. 构建失败：运行 `npm run clear` 清理构建缓存

### 代码问题
1. 类型错误：检查 `src/types/` 中的类型定义
2. 状态管理：参考现有状态模块的 `state.ts` + `action.ts` 模式
3. 事件通信：使用 `global.state_event` 进行跨模块通信

### 构建发布
1. 发布构建需要配置签名密钥
2. GitHub Actions自动处理签名和发布流程
3. 构建产物包含MD5校验值用于完整性验证