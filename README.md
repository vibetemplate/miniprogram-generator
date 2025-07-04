# 智能uni-app小程序生成器 🚀

> 借鉴DXT设计理念，为AI工具提供标准化的uni-app小程序生成模板和规范

一个专为Cursor、Claude Code等AI开发工具设计的uni-app小程序生成器库。用户只需复制标准提示词，AI即可自动生成完整的现代化跨平台小程序代码。

## ✨ 核心特性

- 🎯 **标准化提示词** - 一键复制，AI直接理解
- 📋 **结构化配置** - JSON驱动的uni-app小程序定义
- 🎨 **丰富模板** - 电商、服务、内容、工具等类型
- ⚡ **现代技术栈** - uni-app + Vue 3 + TypeScript + Pinia
- 🌐 **跨平台支持** - 微信、支付宝、H5、APP一套代码多端发布
- 🚀 **即用即部署** - 生成即可发布到各大平台

## 🚀 快速开始

### 1. 复制AI提示词

将以下提示词复制到Cursor、Claude Code或其他AI工具中：

```
我想生成一个现代化的uni-app跨平台小程序。请按照以下步骤操作：

1. **仔细阅读规范文档：**
   - https://github.com/vibetemplate/miniprogram-generator/blob/main/README.md - uni-app小程序生成器概述和架构模式
   - https://github.com/vibetemplate/miniprogram-generator/blob/main/UNIAPP-SPEC.md - 完整的uni-app配置规范和字段定义
   - https://github.com/vibetemplate/miniprogram-generator/blob/main/examples - 参考实现，包括电商、服务、内容等示例

2. **创建合适的uni-app项目结构：**
   - 根据UNIAPP-SPEC.md生成有效的uniapp-config.json配置文件
   - 实现现代化的uni-app项目，包含合理的页面和组件结构
   - 自动生成pages.json、manifest.json等uni-app配置文件
   - 包含跨平台兼容性处理和性能优化

3. **遵循最佳开发实践：**
   - 使用现代技术栈 (uni-app + Vue 3 + TypeScript + Pinia)
   - 组件化开发，页面和组件结构清晰
   - 包含适当的错误处理、加载状态和用户交互反馈
   - 支持多端适配(微信小程序、支付宝小程序、H5、APP)
   - 添加完整的项目配置和发布说明

4. **生成生产级代码：**
   - 验证所有页面和功能在多端正确实现
   - 确保符合各平台开发规范和审核要求
   - 包含完整的manifest.json、pages.json等配置文件
   - 支持HBuilderX和CLI两种开发方式

请生成完整的、可立即发布的uni-app跨平台小程序代码。专注于现代设计、干净的代码和遵循uni-app最佳实践。
```

### 2. 与AI交互

AI会自动：
- 📖 读取uni-app开发规范和配置说明
- 🎨 根据您的需求选择合适的模板
- ⚙️ 生成配置文件和项目结构
- 🔧 创建完整的uni-app项目代码

### 3. 本地开发

```bash
# 通过HBuilderX导入项目
# 或使用CLI开发
npm install -g @dcloudio/uvm
npm install -g @dcloudio/cli

# 运行到微信小程序
npm run dev:mp-weixin

# 运行到H5
npm run dev:h5

# 运行到APP
npm run dev:app
```

### 4. 发布部署

```bash
# 构建微信小程序
npm run build:mp-weixin

# 构建H5
npm run build:h5

# 构建APP
npm run build:app
```

## 📦 支持的小程序类型

| 类型 | 描述 | 适用场景 | 核心页面 | 跨平台特色 |
|------|------|----------|----------|----------|
| `ecommerce` | 电商购物 | 购物、零售 | 首页、商品、购物车、订单 | 支付适配、地址选择 |
| `service` | 服务预约 | 美容、餐饮、维修 | 首页、服务、预约、个人中心 | 地图定位、时间选择 |
| `content` | 内容资讯 | 新闻、博客、知识 | 首页、文章、分类、搜索 | 富文本展示、分享 |
| `tool` | 工具应用 | 计算器、查询 | 功能页、历史、设置 | 数据存储、导出 |
| `community` | 社区论坛 | 交流、分享 | 首页、发布、动态、消息 | 实时通讯、图片上传 |

## 🎨 Vue组件模板库

### 基础组件
- `NavBar` - 自定义导航栏（支持状态栏适配）
- `TabBar` - 底部标签栏（多端样式统一）
- `SearchBar` - 搜索组件
- `ProductCard` - 商品卡片
- `UserAvatar` - 用户头像

### 业务组件
- `ShoppingCart` - 购物车组件
- `OrderList` - 订单列表
- `CommentList` - 评论列表
- `AddressPicker` - 地址选择器
- `PaymentMethod` - 支付方式选择

### 跨平台适配组件
- `PlatformView` - 平台差异化视图
- `SafeArea` - 安全区域适配
- `StatusBar` - 状态栏适配

## 📋 配置文件说明

uni-app小程序通过 `uniapp-config.json` 文件进行配置：

```json
{
  "spec_version": "1.0",
  "project": {
    "name": "my-uniapp",
    "type": "ecommerce",
    "appid": "wx1234567890abcdef",
    "title": "我的uni-app小程序",
    "description": "跨平台小程序应用"
  },
  "platforms": {
    "mp-weixin": { "enabled": true },
    "mp-alipay": { "enabled": true },
    "h5": { "enabled": true },
    "app": { "enabled": false }
  },
  "pages": [
    {
      "path": "pages/index/index",
      "name": "首页",
      "style": {
        "navigationBarTitleText": "首页"
      }
    }
  ],
  "features": {
    "login": true,
    "payment": true,
    "location": false
  }
}
```

## 🛠️ 技术栈

- **开发框架**: uni-app (Vue 3)
- **语言**: TypeScript / JavaScript
- **状态管理**: Pinia
- **UI组件**: uni-ui / uView UI
- **样式**: SCSS / 原子化CSS
- **构建工具**: Vite / HBuilderX
- **跨平台**: 微信小程序、支付宝小程序、H5、APP

## 🌐 跨平台特性

### 平台适配
- **微信小程序**: 支持最新API和组件
- **支付宝小程序**: 自动适配支付宝规范
- **H5**: 响应式设计，PWA支持
- **APP**: 原生性能，插件扩展

### 条件编译
```javascript
// #ifdef MP-WEIXIN
// 微信小程序特有代码
// #endif

// #ifdef H5
// H5特有代码
// #endif

// #ifdef APP-PLUS
// APP特有代码
// #endif
```

## 📚 学习资源

- [uni-app官方文档](https://uniapp.dcloud.net.cn/)
- [Vue 3官方文档](https://cn.vuejs.org/)
- [最佳实践指南](./docs/最佳实践.md)
- [配置详细说明](./docs/配置说明.md)
- [跨平台开发指南](./docs/跨平台开发.md)

## 🤝 贡献指南

欢迎贡献新的模板和组件！请查看 [贡献指南](CONTRIBUTING.md)。

## 📄 许可证

MIT License - 查看 [LICENSE](LICENSE) 文件了解详细信息。

---

让AI成为您的uni-app跨平台开发伙伴！🚀