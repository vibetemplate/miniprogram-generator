# uni-app小程序配置规范文档

本文档定义了 `uniapp-config.json` 的完整规范，用于自动生成uni-app跨平台小程序项目。

## 📋 规范概述

uni-app小程序生成器使用声明式配置文件 `uniapp-config.json` 来定义跨平台小程序的结构、功能和内容。AI工具基于此配置自动生成完整的uni-app项目代码。

## 🏗️ 配置文件结构

```json
{
  "spec_version": "1.0",
  "project": { ... },
  "business": { ... },
  "platforms": { ... },
  "pages": [ ... ],
  "tabBar": { ... },
  "components": [ ... ],
  "features": { ... },
  "design": { ... },
  "build": { ... },
  "deployment": { ... }
}
```

## 📱 项目基本信息 (project)

### 必需字段

```json
{
  "project": {
    "name": "my-uniapp",
    "type": "ecommerce|service|content|tool|community",
    "appid": "wx1234567890abcdef",
    "title": "我的uni-app小程序",
    "description": "跨平台小程序应用描述",
    "version": "1.0.0"
  }
}
```

### 可选字段

```json
{
  "project": {
    "author": "开发者名称",
    "keywords": ["跨平台", "uni-app", "小程序"],
    "category": "工具",
    "homepage": "https://example.com",
    "versionName": "1.0.0",
    "versionCode": 100
  }
}
```

### 项目类型说明

| 类型 | 说明 | 典型页面 | 跨平台特色 |
|------|------|----------|----------|
| `ecommerce` | 电商购物 | 首页、商品、购物车、订单 | 多端支付、地址管理 |
| `service` | 服务预约 | 首页、服务、预约、个人中心 | 地图定位、日历选择 |
| `content` | 内容资讯 | 首页、文章、分类、搜索 | 富文本、分享功能 |
| `tool` | 工具应用 | 功能页、历史、设置 | 数据同步、导入导出 |
| `community` | 社区论坛 | 首页、发布、动态、消息 | 实时通讯、媒体上传 |

## 🏢 业务信息 (business)

```json
{
  "business": {
    "name": "公司/品牌名称",
    "logo": {
      "light": "/static/logo-light.png",
      "dark": "/static/logo-dark.png"
    },
    "slogan": "品牌标语",
    "contact": {
      "phone": "400-123-4567",
      "email": "contact@example.com",
      "address": "公司地址",
      "wechat": "微信号",
      "website": "https://example.com",
      "qq": "QQ号码"
    },
    "service_time": {
      "weekdays": "9:00-18:00",
      "weekends": "10:00-17:00",
      "timezone": "Asia/Shanghai"
    }
  }
}
```

## 🌐 平台配置 (platforms)

### 支持的平台

```json
{
  "platforms": {
    "mp-weixin": {
      "enabled": true,
      "appid": "wx1234567890abcdef",
      "setting": {
        "urlCheck": false,
        "es6": true,
        "enhance": true,
        "postcss": true,
        "minified": true
      }
    },
    "mp-alipay": {
      "enabled": true,
      "appid": "alipay_app_id",
      "setting": {
        "minified": true
      }
    },
    "h5": {
      "enabled": true,
      "title": "H5应用标题",
      "template": "default",
      "router": {
        "mode": "history",
        "base": "/"
      }
    },
    "app": {
      "enabled": false,
      "name": "APP应用名称",
      "appid": "__UNI__XXXXXX"
    }
  }
}
```

### 平台特性对比

| 功能 | 微信小程序 | 支付宝小程序 | H5 | APP |
|------|------------|--------------|----|----|
| 登录 | 微信授权 | 支付宝授权 | 账号登录 | 多种方式 |
| 支付 | 微信支付 | 支付宝支付 | 在线支付 | 原生支付 |
| 地图 | 腾讯地图 | 高德地图 | Web地图 | 原生地图 |
| 分享 | 原生分享 | 原生分享 | Web分享 | 原生分享 |

## 📄 页面配置 (pages)

### 页面定义结构

```json
{
  "pages": [
    {
      "path": "pages/index/index",
      "name": "首页",
      "type": "home|list|detail|form|profile",
      "style": {
        "navigationBarTitleText": "首页",
        "navigationBarBackgroundColor": "#ffffff",
        "navigationBarTextStyle": "black",
        "backgroundColor": "#f8f8f8",
        "enablePullDownRefresh": true,
        "onReachBottomDistance": 50,
        "backgroundTextStyle": "dark"
      },
      "components": ["banner", "product-list", "category-nav"],
      "platforms": ["mp-weixin", "mp-alipay", "h5"],
      "data_source": {
        "api": "/api/home",
        "mock": true,
        "cache": true
      }
    }
  ]
}
```

### 页面类型特性

| 类型 | 用途 | 推荐组件 | 数据需求 |
|------|------|----------|----------|
| `home` | 首页 | banner, navigation, grid | 轮播图、导航、推荐 |
| `list` | 列表页 | search-bar, list-item, load-more | 列表数据、分页 |
| `detail` | 详情页 | image-gallery, tabs, action-bar | 详细信息、操作 |
| `form` | 表单页 | form-item, picker, upload | 表单数据、验证 |
| `profile` | 个人中心 | user-info, cell-group | 用户信息、设置 |

## 📱 标签栏配置 (tabBar)

```json
{
  "tabBar": {
    "enabled": true,
    "color": "#7A7E83",
    "selectedColor": "#3cc51f",
    "borderStyle": "black",
    "backgroundColor": "#ffffff",
    "position": "bottom",
    "list": [
      {
        "pagePath": "pages/index/index",
        "iconPath": "static/tab-home.png",
        "selectedIconPath": "static/tab-home-active.png",
        "text": "首页"
      },
      {
        "pagePath": "pages/category/index",
        "iconPath": "static/tab-category.png",
        "selectedIconPath": "static/tab-category-active.png",
        "text": "分类"
      }
    ]
  }
}
```

## 🧩 组件配置 (components)

### Vue组件定义

```json
{
  "components": [
    {
      "name": "ProductCard",
      "type": "display",
      "framework": "vue3",
      "props": {
        "product": {
          "type": "Object",
          "required": true,
          "properties": {
            "id": "number",
            "name": "string",
            "price": "number",
            "image": "string",
            "rating": "number"
          }
        },
        "size": {
          "type": "String",
          "default": "medium",
          "enum": ["small", "medium", "large"]
        }
      },
      "events": ["click", "add-cart", "share"],
      "slots": ["header", "footer"],
      "style": "card",
      "platforms": ["mp-weixin", "mp-alipay", "h5", "app"]
    }
  ]
}
```

### 组件分类

- **基础组件**: Button, Input, Image, Text
- **布局组件**: Row, Col, Grid, Flex
- **导航组件**: NavBar, TabBar, Breadcrumb
- **数据展示**: Card, List, Table, Tag
- **数据录入**: Form, Picker, Upload, Switch
- **反馈组件**: Modal, Toast, Loading
- **业务组件**: ProductCard, OrderItem, UserProfile

## ⚙️ 功能配置 (features)

### 基础功能

```json
{
  "features": {
    "login": {
      "enabled": true,
      "providers": {
        "mp-weixin": ["wechat"],
        "mp-alipay": ["alipay"],
        "h5": ["username", "phone", "wechat"],
        "app": ["username", "phone", "wechat", "qq"]
      },
      "auto_login": true,
      "session_timeout": 7200
    },
    "payment": {
      "enabled": true,
      "providers": {
        "mp-weixin": ["wechat_pay"],
        "mp-alipay": ["alipay_pay"],
        "h5": ["wechat_pay", "alipay_pay"],
        "app": ["wechat_pay", "alipay_pay", "apple_pay"]
      },
      "sandbox": true
    },
    "location": {
      "enabled": true,
      "providers": {
        "mp-weixin": "tencent",
        "mp-alipay": "amap",
        "h5": "web_api",
        "app": "native"
      },
      "precision": "gcj02",
      "cache": true
    }
  }
}
```

### 跨平台功能适配

```json
{
  "features": {
    "share": {
      "enabled": true,
      "platforms": {
        "mp-weixin": {
          "to_friends": true,
          "to_timeline": false
        },
        "mp-alipay": {
          "to_friends": true,
          "to_timeline": true
        },
        "h5": {
          "native": false,
          "custom": true
        },
        "app": {
          "native": true,
          "providers": ["wechat", "qq", "weibo"]
        }
      }
    },
    "camera": {
      "enabled": true,
      "scan_code": true,
      "photo": true,
      "video": false
    },
    "storage": {
      "local": true,
      "cloud": {
        "enabled": false,
        "provider": "leancloud|bmob|firebase"
      }
    }
  }
}
```

## 🎨 设计配置 (design)

### 主题和样式

```json
{
  "design": {
    "theme": "light|dark|auto",
    "color_scheme": {
      "primary": "#007aff",
      "secondary": "#5ac8fa",
      "success": "#34c759",
      "warning": "#ff9500",
      "error": "#ff3b30",
      "info": "#5856d6",
      "text": {
        "primary": "#000000",
        "secondary": "#999999",
        "placeholder": "#c0c4cc"
      },
      "background": {
        "primary": "#ffffff",
        "secondary": "#f7f7f7",
        "tertiary": "#efeff4"
      }
    },
    "typography": {
      "font_family": "PingFangSC-Regular, sans-serif",
      "font_sizes": {
        "xs": "20rpx",
        "sm": "24rpx",
        "base": "28rpx",
        "lg": "32rpx",
        "xl": "36rpx",
        "2xl": "40rpx"
      }
    },
    "spacing": {
      "xs": "8rpx",
      "sm": "16rpx",
      "base": "24rpx",
      "lg": "32rpx",
      "xl": "48rpx",
      "2xl": "64rpx"
    },
    "border_radius": {
      "sm": "8rpx",
      "base": "12rpx",
      "lg": "16rpx",
      "xl": "24rpx",
      "full": "50%"
    }
  }
}
```

### 响应式设计

```json
{
  "design": {
    "responsive": {
      "enabled": true,
      "breakpoints": {
        "xs": "0",
        "sm": "576rpx",
        "md": "768rpx",
        "lg": "992rpx",
        "xl": "1200rpx"
      },
      "container": {
        "padding": "32rpx",
        "max_width": "1200rpx"
      }
    },
    "layout": {
      "grid": {
        "columns": 12,
        "gutter": "24rpx"
      },
      "safe_area": {
        "enabled": true,
        "top": true,
        "bottom": true
      }
    }
  }
}
```

## 🔧 构建配置 (build)

### 编译配置

```json
{
  "build": {
    "typescript": {
      "enabled": true,
      "strict": true,
      "declaration": false
    },
    "scss": {
      "enabled": true,
      "dart_sass": true
    },
    "optimization": {
      "minify": true,
      "tree_shaking": true,
      "code_splitting": true,
      "vendor_splitting": true
    },
    "polyfill": {
      "enabled": true,
      "targets": "> 1%, last 2 versions"
    },
    "assets": {
      "inline_limit": 4096,
      "compress": true,
      "cdn": {
        "enabled": false,
        "domain": "https://cdn.example.com"
      }
    }
  }
}
```

### 条件编译

```json
{
  "build": {
    "conditional_compilation": {
      "enabled": true,
      "variables": {
        "MP_WEIXIN": true,
        "MP_ALIPAY": true,
        "H5": true,
        "APP_PLUS": false
      }
    },
    "plugins": [
      {
        "name": "uniapp-plugin-mock",
        "enabled": true,
        "options": {
          "mockPath": "./mock"
        }
      }
    ]
  }
}
```

## 🚀 部署配置 (deployment)

### 环境配置

```json
{
  "deployment": {
    "environments": {
      "development": {
        "api_base_url": "https://dev-api.example.com",
        "debug": true,
        "mock": true,
        "source_map": true
      },
      "staging": {
        "api_base_url": "https://staging-api.example.com",
        "debug": false,
        "mock": false,
        "source_map": true
      },
      "production": {
        "api_base_url": "https://api.example.com",
        "debug": false,
        "mock": false,
        "source_map": false
      }
    },
    "platforms": {
      "mp-weixin": {
        "uploadCommand": "npm run build:mp-weixin",
        "outputPath": "dist/build/mp-weixin"
      },
      "h5": {
        "buildCommand": "npm run build:h5",
        "outputPath": "dist/build/h5",
        "publicPath": "/"
      },
      "app": {
        "buildCommand": "npm run build:app",
        "outputPath": "dist/build/app"
      }
    }
  }
}
```

## 🔍 配置验证

### 使用JSON Schema验证

```bash
# 安装验证工具
npm install -g ajv-cli

# 验证配置文件
ajv validate -s schemas/uniapp-config.schema.json -d uniapp-config.json
```

### 常见配置错误

1. **平台AppID错误**
   ```json
   // ❌ 错误
   "appid": "123456"
   
   // ✅ 正确
   "appid": "wx1234567890abcdef"
   ```

2. **页面路径格式错误**
   ```json
   // ❌ 错误
   "path": "/pages/index"
   
   // ✅ 正确
   "path": "pages/index/index"
   ```

3. **组件平台支持配置错误**
   ```json
   // ❌ 错误
   "platforms": ["weixin", "alipay"]
   
   // ✅ 正确
   "platforms": ["mp-weixin", "mp-alipay"]
   ```

## 📊 配置示例

### 电商uni-app配置

```json
{
  "spec_version": "1.0",
  "project": {
    "name": "fashion-store-uniapp",
    "type": "ecommerce",
    "appid": "wx1234567890abcdef",
    "title": "时尚商城",
    "description": "跨平台时尚购物应用",
    "version": "1.0.0"
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
      "type": "home",
      "components": ["banner", "category-grid", "product-list"]
    }
  ],
  "tabBar": {
    "enabled": true,
    "list": [
      {
        "pagePath": "pages/index/index",
        "text": "首页",
        "iconPath": "static/tab-home.png",
        "selectedIconPath": "static/tab-home-active.png"
      }
    ]
  },
  "features": {
    "login": { "enabled": true },
    "payment": { "enabled": true },
    "share": { "enabled": true }
  }
}
```

## 🎯 最佳实践

### 跨平台开发建议

1. **组件设计原则**
   - 优先使用uni-app内置组件
   - 自定义组件支持多端适配
   - 使用条件编译处理平台差异

2. **样式规范**
   - 使用rpx单位保证响应式
   - 避免使用平台特有样式属性
   - 统一设计语言和交互规范

3. **API调用策略**
   - 优先使用uni-app统一API
   - 平台特有功能使用条件编译
   - 做好降级处理和错误处理

4. **性能优化**
   - 合理使用分包加载
   - 图片资源优化和懒加载
   - 减少页面初始渲染时间

### 代码组织建议

```
src/
├── pages/          # 页面文件
├── components/     # 组件文件
├── static/         # 静态资源
├── store/          # 状态管理
├── utils/          # 工具函数
├── api/            # 接口封装
├── styles/         # 全局样式
├── uni_modules/    # uni-app插件
├── App.vue         # 应用入口
├── main.ts         # 主入口文件
├── manifest.json   # 应用配置
├── pages.json      # 页面配置
└── uni.scss        # 全局样式变量
```

---

更多详细信息请参考 [uni-app官方文档](https://uniapp.dcloud.net.cn/)。