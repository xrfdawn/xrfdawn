# Chat Friends - 现代化好友聊天系统

> 基于 Next.js + Supabase + Socket.IO 的极简风格好友聊天应用，支持注册、登录、资料完善、好友管理、分组、聊天（文本/图片/实时推送）、UI美化、错误处理、交互体验优化，兼容桌面端体验。

---

## ✨ 特性 Features

- **注册/登录/邮箱验证**：支持邮箱注册、邮箱验证、密码登录。
- **资料完善**：支持头像上传、昵称编辑、密码设置。
- **好友管理**：
  - 搜索邮箱/昵称添加好友，备注、分组管理。
  - 删除好友、分组筛选、分组编辑。
  - 禁止添加自己为好友，添加后双方互为好友。
- **聊天功能**：
  - 支持文本、图片消息。
  - 实时消息推送，未读消息红点提醒。
  - 聊天窗口自动滚动、消息已读处理。
- **UI/交互**：
  - 极简现代风格，兼容Windows桌面。
  - 侧边栏切换“聊天”“好友管理”“我的资料”，高亮与红点提示。
  - 退出/切换账号功能，模态弹窗确认。
- **安全性**：Supabase 鉴权、RLS 行级安全，API参数校验。
- **AI辅助开发**：所有主要文件头部均有 `// prompt:` 注释，便于二次开发。

---

## 🗂️ 目录结构

```
chat-friends/
├── src/app/           # Next.js 页面与API路由
├── src/lib/           # 通用库（Supabase、Socket等）
├── src/context/       # React全局Context
├── supabase/          # Supabase配置与SQL
├── public/            # 静态资源
├── package.json       # 依赖与脚本
└── README.md          # 项目说明
```

---

## 🛠️ 技术栈

- [Next.js](https://nextjs.org/) 14.x
- [Supabase](https://supabase.com/) (PostgreSQL)
- [Socket.IO](https://socket.io/)
- React 18.x
- TypeScript 5.x

---

## 🚀 快速开始

1. **安装依赖**
   ```bash
   npm install
   ```
2. **配置 Supabase**
   - 在 `supabase/` 目录下配置你的 Supabase 项目参数（见 `.env` 示例）。
   - 确保数据库表结构、RLS（行级安全）、Storage 权限已按文档设置。
3. **启动项目**
   ```bash
   npm run dev
   ```
4. **访问**
   - 打开浏览器访问 `http://localhost:3001`
   - 注册新账号，按提示完成邮箱验证和资料完善。

---

## ☁️ 云端数据库与邮件系统说明

1. 本项目使用 sysdase 的线上 PostgreSQL 数据库，所有数据均存储在云端，需在配置中填写 sysdase 提供的 key。
2. 项目集成了 sysdase 的邮件收发系统，所有注册用户必须通过真实邮箱进行验证，确保账号安全。
3. sysdase 平台已将重定向地址设置为 `http://localhost:3001`，因此本地开发和测试时请务必使用 3001 端口访问项目。

---

## 📚 功能实现与加分项分析

### 基础功能实现情况

| 功能点                | 实现情况         | 说明/建议补充                      |
|----------------------|----------------|-----------------------------------|
| 好友列表              | ✅ 已实现        | `/api/friends`、好友管理页面已具备 |
| 搜索好友              | ✅ 已实现        | `/api/friends/search`、UI已具备   |
| 添加好友              | ✅ 已实现        | 支持备注、分组、双向添加           |
| 删除好友              | ✅ 已实现        | 双向删除，防止唯一性冲突           |
| 聊天对话列表          | ✅ 已实现        | 聊天主界面左侧为会话/好友列表      |
| 跟好友聊天            | ✅ 已实现        | 支持文本消息，Socket.IO 实时推送   |
| 发送图片              | ✅ 已实现        | 聊天窗口支持图片上传与发送         |

### 规范与建议

| 规范点                | 实现情况         | 说明/建议补充                      |
|----------------------|----------------|-----------------------------------|
| AI辅助编码+prompt注释 | ✅ 已实现        | 主要文件头部均有 `// prompt:` 注释 |
| 目录结构清晰          | ✅ 已实现        | 结构合理，便于维护与扩展           |
| RESTful API           | ✅ 已实现        | API 路由均为 RESTful 风格          |
| 前后端分离            | ✅ 已实现        | API、前端、Socket.IO 分离          |
| 使用Supabase          | ✅ 已实现        | 作为后端、鉴权、存储、数据库       |
| 数据库                | ✅ 已实现        | Supabase 默认 PostgreSQL           |
| 一定的安全性          | ✅ 部分实现      | 已有鉴权、RLS，建议补充更多安全措施 |

### 后续优化

- **更多IM体验**：如群聊、消息撤回、批量管理等。

---

## 📖 详细接口文档

### 注册/登录

#### POST `/register`
- 功能：邮箱+密码+昵称+头像注册，注册后需邮箱验证。
- 请求参数：
  ```json
  {
    "email": "string",
    "password": "string",
    "nickname": "string",
    "avatar_url": "string (可选)"
  }
  ```
- 返回：
  ```json
  {
    "user": { ... },
    "error": null
  }
  ```

#### POST `/login`
- 功能：邮箱+密码登录。
- 请求参数：
  ```json
  {
    "email": "string",
    "password": "string"
  }
  ```
- 返回：
  ```json
  {
    "user": { ... },
    "error": null
  }
  ```

#### POST `/complete-profile`
- 功能：首次登录后完善资料。
- 请求参数：
  ```json
  {
    "nickname": "string",
    "avatar_url": "string (可选)"
  }
  ```
- 返回：
  ```json
  {
    "success": true
  }
  ```

### 好友管理

#### GET `/api/friends`
- 功能：获取当前用户的好友列表。
- Header：`x-user-id: string`
- 返回：
  ```json
  {
    "friends": [
      {
        "id": "string",
        "friend_id": "string",
        "remark": "string",
        "group_name": "string",
        "created_at": "string",
        "profiles": {
          "nickname": "string",
          "avatar_url": "string"
        }
      }
    ]
  }
  ```

#### POST `/api/friends`
- 功能：添加好友（双向插入，禁止自加，已存在先删除再加）。
- Header：`x-user-id: string`
- 请求参数：
  ```json
  {
    "friend_id": "string",
    "remark": "string (可选)",
    "group_name": "string (可选)"
  }
  ```
- 返回：
  ```json
  {
    "success": true
  }
  ```

#### DELETE `/api/friends`
- 功能：删除好友（双向删除）。
- Header：`x-user-id: string`
- 请求参数：
  ```json
  {
    "friend_id": "string"
  }
  ```
- 返回：
  ```json
  {
    "success": true
  }
  ```

#### PATCH `/api/friends`
- 功能：备注/分组编辑。
- Header：`x-user-id: string`
- 请求参数：
  ```json
  {
    "friend_id": "string",
    "remark": "string (可选)",
    "group_name": "string (可选)"
  }
  ```
- 返回：
  ```json
  {
    "success": true
  }
  ```

#### GET `/api/friends/search`
- 功能：通过邮箱或昵称模糊搜索用户。
- Header：`x-user-id: string`
- 查询参数：`q=关键词`
- 返回：
  ```json
  {
    "users": [
      {
        "id": "string",
        "nickname": "string",
        "avatar_url": "string",
        "email": "string"
      }
    ]
  }
  ```

#### GET `/api/friends/groups`
- 功能：获取所有分组。
- Header：`x-user-id: string`
- 返回：
  ```json
  {
    "groups": ["分组1", "分组2", ...]
  }
  ```

### 聊天相关

#### GET `/api/messages`
- 功能：获取与某好友的历史消息。
- Header：`x-user-id: string`
- 查询参数：`friend_id=string`
- 返回：
  ```json
  {
    "messages": [
      {
        "id": "string",
        "sender_id": "string",
        "receiver_id": "string",
        "content": "string",
        "image_url": "string (可选)",
        "created_at": "string",
        "is_read": true
      }
    ]
  }
  ```

#### POST `/api/messages`
- 功能：发送消息（文本/图片）。
- Header：`x-user-id: string`
- 请求参数：
  ```json
  {
    "friend_id": "string",
    "content": "string (可选)",
    "image_url": "string (可选)"
  }
  ```
- 返回：
  ```json
  {
    "success": true
  }
  ```

#### GET `/api/messages/unread-count`
- 功能：获取所有未读消息总数及每个好友的未读数。
- Header：`x-user-id: string`
- 返回：
  ```json
  {
    "total": 3,
    "perFriend": {
      "friend_id1": 2,
      "friend_id2": 1
    }
  }
  ```

#### POST `/api/messages/mark-read`
- 功能：将与某好友的消息标记为已读。
- Header：`x-user-id: string`
- 请求参数：
  ```json
  {
    "friend_id": "string"
  }
  ```
- 返回：
  ```json
  {
    "success": true
  }
  ```

---

## ❓ 常见问题 FAQ

1. **添加/删除好友后列表未刷新？**
   - 已实现添加/删除后自动刷新好友列表，若遇到问题请刷新页面或检查网络。
2. **删除好友后无法重新添加？**
   - 已修复为双向删除+添加前先删除，彻底避免唯一性冲突。
3. **多窗口多账号登录冲突？（方便测试）**
   - 已移除 localStorage 依赖，完全依赖 Supabase session，每个窗口互不影响。
4. **邮箱验证链接打不开？**
   - 邮箱客户端可能用 iframe sandbox 打开，建议复制链接到浏览器新标签页访问。
5. **未来可优化方向**
   - 支持群聊、消息撤回、表情包、消息漫游。
   - 更丰富的资料页、好友备注/分组批量管理。
   - 更细致的错误提示与操作引导。


## 📄 许可证 License

MIT License © 2024 Xurufen & Chat Friends Contributors 