# register
使用C#写出的HTML注册界面
下载压缩包并解压后，可点击job.slnx，使用Visual Studio进行开发
1. 开发环境与工具
开发工具：Visual Studio 2026（社区版）

框架：.NET 8.0 (ASP.NET Core MVC)

数据库：MySQL 8.0（本地服务器 192.168.31.31）

NuGet 包：

Pomelo.EntityFrameworkCore.MySql（MySQL EF Core 驱动）

Microsoft.EntityFrameworkCore.Tools（迁移命令）

BCrypt.Net-Next（密码哈希）

2. 主要功能实现
用户注册页面（/Account/Register）

表单验证（客户端 + 服务器端）

密码加密存储（BCrypt）

用户名和邮箱唯一性检查（数据库唯一索引）

数据保存到 MySQL 数据库

3. 开发步骤回顾
创建项目：使用 MVC 模板，项目名称 job。

添加 NuGet 包：安装 MySQL 驱动、EF Core 工具、BCrypt。

配置连接字符串：在 appsettings.json 中设置 MySQL 服务器地址、数据库名、用户名、密码。

编写模型：

User：对应数据库表，包含 Id、用户名、邮箱、密码哈希、创建时间。

RegisterViewModel：用于注册表单，包含验证规则。

创建数据库上下文 AppDbContext：管理 User 实体，配置唯一索引。

配置 Program.cs：注册 DbContext 和 MVC 服务。

创建控制器 AccountController：处理注册 GET/POST 请求，包含业务逻辑。

创建视图 Register.cshtml：用户界面，使用 Tag Helpers 绑定模型。

添加导航链接：在布局页添加“注册”链接。

处理数据库远程连接：

创建远程用户 'root'@'%' 或专用用户 'jobuser'@'%'。

修改 MySQL 配置允许远程连接（bind-address、防火墙）。

执行迁移：

解决编译错误（目标框架、包版本、using 缺失）。

解决 dotnet-ef 工具版本冲突，最终使用包管理器控制台成功创建迁移并更新数据库。

运行测试：启动项目，访问注册页面，填写信息提交，验证数据库记录。

4. 关键点与注意事项
目标框架：务必使用稳定的 .NET 8.0，避免预览版带来的包兼容问题。

NuGet 包版本：确保所有 EF Core 相关包版本一致（8.0.x），与 dotnet-ef 工具匹配。

数据库连接：

远程 MySQL 需要创建相应用户并授权。

服务器需开放 3306 端口或使用 SSH 隧道。

连接字符串中的 Database 可以自动创建，但需用户有创建权限。

密码安全：使用 BCrypt 哈希，切勿明文存储。

唯一约束：除了代码检查，数据库层面也设置了唯一索引，防止并发重复。

5. 项目结构
text
job/
├── Controllers/
│   └── AccountController.cs
├── Data/
│   └── AppDbContext.cs
├── Models/
│   ├── User.cs
│   └── RegisterViewModel.cs
├── Views/
│   └── Account/
│       └── Register.cshtml
├── appsettings.json
├── Program.cs
└── job.csproj
