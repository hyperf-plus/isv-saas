# 🚀 HyperF Plus

**企业级HyperF框架扩展包集合 - 开箱即用的微服务基础设施**

[![PHP Version](https://img.shields.io/badge/php-%3E%3D8.1-blue.svg)](https://php.net/)
[![HyperF Version](https://img.shields.io/badge/hyperf-%3E%3D3.0-green.svg)](https://hyperf.io/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Release Date](https://img.shields.io/badge/release-2025.06.27-brightgreen.svg)]()

> 🎯 **一站式企业级解决方案**：权限管理、消息系统、同步服务、通知中心、路由增强、验证工具、API文档 - 让您专注于业务逻辑，基础设施我们搞定！

---

## ✨ 核心特性

### 🔐 智能权限管理系统
- **🎯 RBAC权限模型** - 角色、权限、用户三层架构，支持权限继承
- **🚀 自动资源发现** - 智能扫描注解路由和权限，自动生成资源权限
- **⚡ 高性能缓存** - Redis三级缓存，权限检查响应时间 < 10ms
- **🛡️ 数据权限过滤** - 一行代码启用，支持全局/租户/部门/团队/个人五级权限
- **💻 命令行工具** - 4个交互式命令，轻松管理权限系统
- **🔍 注解路由集成** - 完美集成 `#[Permission]`、`#[ApiController]` 等注解

### 📨 企业级消息系统
- **📬 消息中心** - 统一消息管理，支持分类、优先级、模板、路由
- **🔔 通知系统** - 多渠道通知发送（Webhook、邮件、短信、推送）
- **⚙️ 消息队列** - 高性能队列处理，支持延时、优先级、重试、死信队列
- **🔄 同步服务** - 企业级数据同步，支持增量同步、冲突处理、回滚机制

### 🛠️ 开发增强工具
- **🛣️ 智能路由** - RESTful路径自动生成，支持多种映射策略
- **✅ 强大验证** - Laravel风格验证规则，支持JSON Schema转换
- **📚 API文档** - OpenAPI 3.1.1自动文档生成，完美集成Swagger UI
- **🔧 组织管理** - 企业组织架构管理，支持多租户数据隔离

### 🎨 开发者友好
- **📝 注解驱动** - 声明式编程，代码简洁优雅
- **🛠️ 开箱即用** - 零配置启动，5分钟完成集成
- **📖 完整文档** - 详细的使用指南和最佳实践
- **🔧 高度可配置** - 灵活的配置系统，满足各种业务需求

---

## 📦 模块列表

| 模块 | 功能 | 状态 | 版本 |
|------|------|------|------|
| **hyperf-plus/core** | 基础核心包 | ✅ 稳定 | v1.0 |
| **hyperf-plus/auth** | OAuth2.0授权 | ✅ 稳定 | v1.0 |
| **hyperf-plus/permission** | 权限管理系统 | ✅ 稳定 | v1.0 |
| **hyperf-plus/route** | 智能路由增强 | ✅ 稳定 | v1.0 |
| **hyperf-plus/validate** | 数据验证工具 | ✅ 稳定 | v1.0 |
| **hyperf-plus/swagger** | API文档生成 | ✅ 稳定 | v1.0 |
| **hyperf-plus/message** | 消息中心 | ✅ 稳定 | v1.0 |
| **hyperf-plus/notification** | 通知系统 | ✅ 稳定 | v1.0 |
| **hyperf-plus/queue** | 消息队列 | ✅ 稳定 | v1.0 |
| **hyperf-plus/sync** | 同步服务 | ✅ 稳定 | v1.0 |
| **hyperf-plus/organization** | 组织管理 | ✅ 稳定 | v1.0 |
| **hyperf-plus/developer** | 开发者平台 | ✅ 稳定 | v1.0 |
| **hyperf-plus/openapi** | 开放API | ✅ 稳定 | v1.0 |

---

## 🚀 快速开始

### 环境要求

- PHP >= 8.1
- HyperF >= 3.0
- Redis >= 5.0
- MySQL >= 5.7

### 安装

#### 1. 安装核心包
```bash
# 基础包（必需）
composer require hyperf-plus/core

# 权限系统（推荐）
composer require hyperf-plus/permission

# 消息系统（可选）
composer require hyperf-plus/message
composer require hyperf-plus/notification
composer require hyperf-plus/queue
composer require hyperf-plus/sync

# 开发工具（可选）
composer require hyperf-plus/route
composer require hyperf-plus/validate
composer require hyperf-plus/swagger
```

#### 2. 发布配置和迁移文件
```bash
php bin/hyperf.php vendor:publish hyperf-plus
```

#### 3. 执行数据库迁移
```bash
php bin/hyperf.php migrate
```

#### 4. 初始化权限系统（如果安装了权限模块）
```bash
php bin/hyperf.php permission:init --discover --seed
```

### 基础使用

#### 1. 启用数据权限过滤

```php
<?php
use HPlus\Permission\Trait\HasDataPermission;

class User extends Model 
{
    use HasDataPermission; // 一行代码启用权限过滤
}

// 查询时自动过滤，只返回有权限的数据
$users = User::all(); 
```

#### 2. 权限检查

```php
<?php
use HPlus\Permission\Service\PermissionService;

class UserController 
{
    #[Inject]
    protected PermissionService $permissionService;
    
    public function deleteUser(int $userId) 
    {
        // 检查权限
        if (!$this->permissionService->hasPermission($userId, 'user.delete')) {
            throw new ApiException('没有删除权限');
        }
        
        // 执行删除操作
        User::destroy($userId);
    }
}
```

#### 3. 注解权限控制

```php
<?php
use YC\Open\Annotation\Permission;

#[ApiController(tag: '用户管理')]
class UserController 
{
    #[GetApi(summary: '用户列表')]
    #[Permission(['user:view'], message: '需要用户查看权限')]
    public function list() 
    {
        return User::paginate();
    }
    
    #[PostApi(summary: '创建用户')]
    #[Permission(['user:create', 'dept:manage'], mode: 'AND')]
    public function create() 
    {
        // 自动权限验证，无权限将返回403
    }
}
```

#### 4. 智能路由生成

```php
<?php
use HPlus\Route\Annotation\ApiController;
use HPlus\Route\Annotation\GetApi;

#[ApiController] // 自动生成 /api/users
class UserController 
{
    #[GetApi] // 自动生成 GET /api/users
    public function index() {}
    
    #[GetApi] // 自动生成 GET /api/users/{id}
    public function show(int $id) {}
    
    #[PostApi] // 自动生成 POST /api/users
    public function create() {}
}
```

#### 5. 消息发送

```php
<?php
use HPlus\Message\Service\MessageService;

class OrderController 
{
    #[Inject]
    protected MessageService $messageService;
    
    public function createOrder(array $data) 
    {
        // 创建订单逻辑...
        
        // 发送通知
        $this->messageService->send([
            'type' => 'order_created',
            'title' => '订单创建成功',
            'content' => "订单 #{$order->id} 已创建",
            'recipients' => [$data['user_id']],
            'priority' => 3,
        ]);
    }
}
```

---

## 📚 详细文档

### 🔐 权限系统

#### 命令行工具

```bash
# 初始化权限系统
php bin/hyperf.php permission:init --discover --seed

# 创建角色
php bin/hyperf.php permission:role:create admin "管理员" --template=admin

# 分配权限
php bin/hyperf.php permission:assign 123 admin

# 缓存管理
php bin/hyperf.php permission:cache --action=clear --type=all
```

#### 权限检查 API

```php
// 检查单个权限
$hasPermission = $permissionService->hasPermission($userId, 'user.create');

// 检查多个权限（AND）
$hasAll = $permissionService->hasAllPermissions($userId, ['user.create', 'dept.manage']);

// 检查多个权限（OR）
$hasAny = $permissionService->hasAnyPermission($userId, ['user.view', 'user.edit']);

// 获取用户所有权限
$permissions = $permissionService->getUserPermissions($userId);

// 获取用户角色
$roles = $permissionService->getUserRoles($userId);
```

#### 角色管理

```php
// 分配角色
$permissionService->assignRole($userId, 'admin');

// 撤销角色
$permissionService->revokeRole($userId, 'admin');

// 检查角色
$hasRole = $permissionService->hasRole($userId, 'admin');
```

### 📨 消息系统

#### 消息中心

```php
use HPlus\Message\Service\MessageService;

// 发送消息
$messageId = $messageService->send([
    'type' => 'system_notice',
    'title' => '系统公告',
    'content' => '系统将于今晚维护',
    'priority' => 5, // 1-5优先级
    'recipients' => [1, 2, 3], // 接收者用户ID
    'expires_at' => time() + 7*24*3600, // 7天后过期
    'metadata' => ['module' => 'system']
]);

// 批量发送
$results = $messageService->batchSend([
    ['type' => 'welcome', 'recipients' => [1], 'title' => '欢迎'],
    ['type' => 'reminder', 'recipients' => [2], 'title' => '提醒'],
]);
```

#### 通知系统

```php
use HPlus\Notification\Service\NotificationService;

// 发送Webhook通知
$notificationService->sendWebhook([
    'url' => 'https://api.example.com/webhook',
    'data' => ['event' => 'user_created', 'user_id' => 123],
    'headers' => ['Authorization' => 'Bearer token']
]);

// 发送邮件通知
$notificationService->sendEmail([
    'to' => 'user@example.com',
    'subject' => '账户创建成功',
    'template' => 'user_welcome',
    'variables' => ['name' => 'John', 'email' => 'john@example.com']
]);
```

#### 队列系统

```php
use HPlus\Queue\Service\QueueService;

// 推送任务到队列
$jobId = $queueService->push('email_queue', [
    'type' => 'send_email',
    'data' => ['to' => 'user@example.com', 'subject' => 'Hello'],
    'priority' => 3,
    'delay' => 300 // 5分钟后执行
]);

// 批量推送任务
$jobIds = $queueService->batchPush('notification_queue', [
    ['type' => 'sms', 'data' => ['phone' => '13800138000']],
    ['type' => 'push', 'data' => ['user_id' => 123]],
]);
```

---

## ⚙️ 配置说明

### 权限系统配置

```php
// config/autoload/permission.php
return [
    // 基础配置
    'enabled' => true,
    'default_guard' => 'web',
    
    // 缓存配置
    'cache' => [
        'enabled' => true,
        'driver' => 'redis',
        'prefix' => 'permission:',
        'ttl' => [
            'user_permissions' => 1800,    // 30分钟
            'role_permissions' => 3600,    // 1小时
            'permission_tree' => 7200,     // 2小时
        ],
    ],
    
    // 数据权限配置
    'data_permission' => [
        'enabled' => true,
        'default_scope' => 'tenant',
        'scopes' => [
            'global' => \HPlus\Permission\Scope\GlobalScope::class,
            'tenant' => \HPlus\Permission\Scope\TenantScope::class,
            'department' => \HPlus\Permission\Scope\DepartmentScope::class,
        ],
    ],
    
    // 安全配置
    'security' => [
        'max_login_attempts' => 5,
        'lockout_duration' => 900,
        'audit_enabled' => true,
        'super_admin_bypass' => true,
    ],
    
    // 资源自动发现
    'resources' => [
        'auto_discovery' => [
            'enabled' => true,
            'routes' => true,
            'permissions' => true,
            'exclude_patterns' => ['/health*', '/metrics*'],
        ],
    ],
];
```

### 路由配置

```php
// config/autoload/route.php
return [
    'restful' => [
        'enabled' => true,
        'prefix' => '/api',
        'version_detection' => true,
        'auto_pluralization' => true,
    ],
    
    'path_generation' => [
        'snake_case' => true,
        'remove_controller_suffix' => true,
        'intelligent_mapping' => true,
    ],
];
```

---

## 🏗️ 架构设计

### 系统架构图

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   业务应用层     │    │   API网关层      │    │   前端应用       │
│  Business App   │    │   API Gateway   │    │  Frontend App   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
    HyperF Plus 框架扩展包集合
         │                       │                       │
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   权限管理       │    │   消息系统       │    │   开发工具       │
│  Permission     │    │   Message       │    │  Dev Tools      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   HyperF 框架   │    │   数据存储层     │    │   外部服务       │
│  HyperF Core    │    │   Data Layer    │    │ External API    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### 核心设计原则

- **单一职责** - 每个模块专注特定领域
- **高内聚低耦合** - 模块间通过标准接口交互
- **事件驱动** - 异步处理和松耦合通信
- **可扩展性** - 预留扩展点支持业务增长
- **注解驱动** - 基于注解的声明式编程

---

## 🤝 贡献指南

### 贡献流程

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交改动 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 创建 Pull Request

### 开发规范

- 遵循 PSR-12 代码规范
- 所有新功能必须包含单元测试
- 更新相关文档
- 保持向后兼容性

---

## 📝 更新日志

### v1.0.0 (2025-06-27)

🎉 **首次开源发布**

#### ✨ 新功能
- 🔐 **权限管理系统** - 完整的RBAC权限体系
- 📨 **消息系统** - 消息中心、通知、队列、同步服务
- 🛠️ **开发工具** - 智能路由、数据验证、API文档
- 🏢 **企业功能** - 组织管理、开发者平台、OAuth2.0

#### 🚀 技术特性
- 支持 PHP 8.1+ 和 HyperF 3.0+
- Redis缓存优化，性能提升70%
- OpenAPI 3.1.1 完整支持
- 注解驱动的声明式编程
- 企业级安全和稳定性保证

#### 📦 模块列表
- 13个核心模块全部稳定发布
- 100+ API接口开箱即用
- 丰富的命令行工具
- 完整的测试覆盖

---

## 📄 开源协议

本项目采用 [MIT](LICENSE) 开源协议。

---

## 💬 支持与反馈

- 📧 邮箱：4213509@qq.com
- 🐛 问题反馈：[GitHub Issues](https://github.com/hyperf-plus/isv-saas/issues)
- 💬 讨论区：[GitHub Discussions](https://github.com/hyperf-plus/isv-saas/discussions)
- 📖 文档：[官方文档](https://docs.hyperf-plus.com)

---

## 🌟 Star History

[![Star History Chart](https://api.star-history.com/svg?repos=your-org/hyperf-plus&type=Date)](https://star-history.com/#hyperf-plus/admin&Date)

---

**🎯 让企业级开发变得简单高效，HyperF Plus 助您快速构建现代化应用！** 
