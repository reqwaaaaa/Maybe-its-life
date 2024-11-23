# Trivia

## 常见的正则表达式用法

### 常用正则表达式示例
- **匹配电子邮件**：
  ```regex
  ^[\w.-]+@[\w.-]+\.[a-zA-Z]{2,}$
  ```
  用于验证电子邮件地址的格式。

- **匹配 IP 地址**：
  ```regex
  ^(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])(\.(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])){3}$
  ```
  用于匹配 IPv4 地址。

- **匹配电话号码**：
  ```regex
  ^\+?[0-9]{1,3}?[-.\s]?(\([0-9]{1,4}\)|[0-9]{1,4})[-.\s]?[0-9]{1,4}[-.\s]?[0-9]{1,9}$
  ```
  适用于多种格式的电话号码验证。

  - **匹配中国大陆+86电话**：
  ```regex
  ^\+?86[-.\s]?[1][3-9][0-9]{9}$
  ```
  专门用于验证中国大陆地区的电话号码，支持可选的国际区号 `+86`。

- **匹配 URL**：
  ```regex
  ^(https?|ftp)://[^\s/$.?#].[^\s]*$
  ```
  用于验证 URL 是否有效。

- **匹配日期（格式 YYYY-MM-DD）**：
  ```regex
  ^(\d{4})-(0[1-9]|1[0-2])-(0[1-9]|[12][0-9]|3[01])$
  ```
  检查日期格式是否符合标准 ISO 8601。
---
## 常见的编码方式及应用

### 常见编码方式
- **UTF-8**：
  - **用途**：最广泛使用的编码，支持世界上几乎所有语言的字符。
  - **特点**：可变长度编码（1-4 字节），节省空间且向下兼容 ASCII。

- **ASCII**：
  - **用途**：用于早期的计算机系统，只支持基本的英文字符。
  - **特点**：固定长度编码（1 字节），只能表示 128 个字符。

- **UTF-16**：
  - **用途**：用于支持更多语言字符，如汉字、日文等。
  - **特点**：固定或变长编码（2 或 4 字节），适合需要处理大量多语言字符的系统。

- **Base64**：
  - **用途**：在 URL、电子邮件、HTTP 请求中编码二进制数据为文本。
  - **特点**：将二进制数据编码为 64 个可打印字符，常用于图像、文件传输等。

- **Hexadecimal (十六进制)**：
  - **用途**：用于表示数据的字节流、颜色值和内存地址。
  - **特点**：每个字节用两个十六进制字符表示，简洁易读。

---
## 常用端口号与服务对应表

### 传输层协议相关
- **20 (FTP Data Transfer)** - File Transfer Protocol 数据传输，主要用于文件的主动传输。
- **21 (FTP Control)** - File Transfer Protocol 控制端口，用于控制命令的传输。
- **22 (SSH)** - Secure Shell，用于加密远程登录和其他安全服务。
- **23 (Telnet)** - Telnet 协议，支持不加密的远程登录。
- **25 (SMTP)** - Simple Mail Transfer Protocol，用于发送电子邮件。
- **53 (DNS)** - Domain Name System，用于域名解析。
- **67, 68 (DHCP)** - Dynamic Host Configuration Protocol，67 是服务器端端口，68 是客户端端口，用于自动分配 IP 地址。
- **69 (TFTP)** - Trivial File Transfer Protocol，简化版的 FTP，用于简单的文件传输。
- **80 (HTTP)** - Hypertext Transfer Protocol，超文本传输协议，用于网页浏览。
- **110 (POP3)** - Post Office Protocol v3，用于电子邮件的接收。
- **119 (NNTP)** - Network News Transfer Protocol，用于网络新闻传输。
- **123 (NTP)** - Network Time Protocol，用于网络时间同步。
- **143 (IMAP)** - Internet Message Access Protocol，用于电子邮件的收取和管理。
- **161, 162 (SNMP)** - Simple Network Management Protocol，161 用于管理设备的请求，162 用于接收设备的通知（陷阱）。
- **194 (IRC)** - Internet Relay Chat，用于互联网聊天服务。
- **443 (HTTPS)** - Hypertext Transfer Protocol Secure，安全超文本传输协议，通过 SSL/TLS 加密。

### 文件传输和远程管理
- **445 (SMB/CIFS)** - Server Message Block / Common Internet File System，用于文件共享。
- **514 (Syslog)** - 系统日志协议，用于传输系统日志消息。
- **520 (RIP)** - Routing Information Protocol，用于路由信息的更新。
- **873 (rsync)** - rsync 协议，支持快速增量文件传输。
- **990 (FTPS)** - FTP over SSL，用于加密的 FTP 服务。

### 数据库与消息队列服务
- **1433 (MSSQL)** - Microsoft SQL Server，用于 SQL Server 数据库服务。
- **1521 (Oracle)** - Oracle 数据库服务。
- **3306 (MySQL)** - MySQL 数据库服务。
- **5432 (PostgreSQL)** - PostgreSQL 数据库服务。
- **6379 (Redis)** - Redis 内存数据库服务。
- **27017 (MongoDB)** - MongoDB 数据库服务。

### 常用应用和网络服务
- **389 (LDAP)** - Lightweight Directory Access Protocol，轻量级目录访问协议，用于目录服务访问。
- **636 (LDAPS)** - LDAP over SSL，用于加密的 LDAP 服务。
- **993 (IMAPS)** - IMAP over SSL，用于加密的 IMAP 邮件服务。
- **995 (POP3S)** - POP3 over SSL，用于加密的 POP3 邮件服务。
- **1883 (MQTT)** - Message Queuing Telemetry Transport，用于物联网设备消息传输。
- **5672 (AMQP)** - Advanced Message Queuing Protocol，用于消息队列服务（如 RabbitMQ）。

### HTTP 和应用层服务
- **8080 (HTTP Alternate)** - 常用的 HTTP 替代端口，一般用于测试或代理服务。
- **8443 (HTTPS Alternate)** - HTTPS 的替代端口，通常用于安全 Web 服务。

### VPN 和远程访问服务
- **1194 (OpenVPN)** - OpenVPN 服务，支持安全的虚拟私人网络连接。
- **1701 (L2TP)** - Layer 2 Tunneling Protocol，第二层隧道协议，用于 VPN 连接。
- **1723 (PPTP)** - Point-to-Point Tunneling Protocol，用于 VPN 连接。
- **4500 (IPsec NAT-T)** - Internet Protocol Security，用于 IPsec 穿越 NAT 的通信。
---
## 没用但是可爱
### 笑脸与表情
- `:smile:` 😊
- `:grinning:` 😀
- `:laughing:` 😆
- `:wink:` 😉
- `:blush:` 😊
- `:heart_eyes:` 😍
- `:sweat_smile:` 😅
- `:joy:` 😂
- `:cry:` 😢
- `:sob:` 😭
- `:angry:` 😠
- `:neutral_face:` 😐
- `:sunglasses:` 😎
- `:thumbsup:` 👍
- `:thumbsdown:` 👎

### 手势
- `:wave:` 👋
- `:ok_hand:` 👌
- `:v:` ✌️
- `:raised_hand:` ✋
- `:point_up:` ☝️
- `:point_right:` 👉
- `:point_left:` 👈
- `:clap:` 👏
- `:pray:` 🙏

### 动物与自然
- `:dog:` 🐶
- `:cat:` 🐱
- `:mouse:` 🐭
- `:rabbit:` 🐰
- `:bear:` 🐻
- `:sunny:` ☀️
- `:cloud:` ☁️
- `:snowflake:` ❄️
- `:fire:` 🔥
- `:flower:` 🌸

### 食物与饮料
- `:apple:` 🍎
- `:banana:` 🍌
- `:watermelon:` 🍉
- `:pizza:` 🍕
- `:hamburger:` 🍔
- `:cake:` 🍰
- `:coffee:` ☕
- `:beer:` 🍺

### 旅行与地点
- `:airplane:` ✈️
- `:car:` 🚗
- `:train:` 🚋
- `:bus:` 🚌
- `:ship:` 🚢
- `:tent:` ⛺
- `:earth_africa:` 🌍

### 对象与符号
- `:gift:` 🎁
- `:trophy:` 🏆
- `:bell:` 🔔
- `:lock:` 🔒
- `:key:` 🔑
- `:heart:` ❤️
- `:star:` ⭐
- `:zap:` ⚡
- `:bulb:` 💡

### 人物与活动
- `:runner:` 🏃
- `:dancer:` 💃
- `:muscle:` 💪
- `:family:` 👪
- `:couple:` 👫
- `:bow:` 🙇
- `:skier:` ⛷️

---



