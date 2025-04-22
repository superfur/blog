---
title: Whistle 跨平台 Web 调试代理工具使用教程
date: 2025-04-22 22:41:00
tags:
  - 开发工具
  - 调试
  - 代理
categories: 开发工具
---

# Whistle 跨平台 Web 调试代理工具使用教程

## 什么是 Whistle？

Whistle（读音[ˈwɪsəl]，拼音[wēisǒu]）是一个基于 Node.js 实现的跨平台 Web 调试代理工具。它类似于 Windows 平台上的 Fiddler，主要用于查看、修改 HTTP、HTTPS、WebSocket 的请求和响应，也可以作为 HTTP 代理服务器使用。

## 安装与配置

### 1. 安装 Whistle

```bash
# 使用 yarn 安装
yarn global add whistle

# 或者使用 npm 安装
npm install -g whistle

# 使用淘宝镜像安装
npm install whistle -g --registry=https://registry.npm.taobao.org
```

### 2. 启动 Whistle

```bash
# 启动 whistle
w2 start

# 查看帮助信息
w2 help

# 停止 whistle
w2 stop

# 重启 whistle
w2 restart
```

### 3. 配置浏览器代理

1. 安装 Chrome 代理插件（推荐使用 SwitchyOmega）
2. 配置代理地址为：`127.0.0.1:8899`
3. 访问 Whistle 控制台：`http://127.0.0.1:8899/`

### 4. 安装 HTTPS 证书

为了能够捕获 HTTPS 请求，需要安装 Whistle 的根证书：

1. 访问 `http://127.0.0.1:8899/` 下载根证书
2. Windows：双击证书，安装到"受信任的根证书颁发机构"
3. iOS：访问 `rootca.pro` 安装证书，并在设置中信任证书
4. Android：访问 `rootca.pro` 安装证书，并将 WiFi 设为可信任网络

## 常用功能

### 1. 请求过滤

```bash
# 隐藏某些域名的请求
/xxx.cn|abc/ filter://hide
```

### 2. 修改请求头

```bash
# 修改 User-Agent
http://m.aliexpress.com ua://{{wp_ua}}

# 修改请求头
http://m.aliexpress.com reqHeaders://{{req-headers}}

# 修改响应头
/https?:\/\/(cdn)\.cn/ resHeaders://{resHeaders.json}
```

### 3. 修改请求/响应内容

```bash
# 修改请求体
http://localhost:8000/api/posts reqBody://{posts.json}

# 修改响应体
http://localhost:8000/api/posts resBody://{posts.json}
```

### 4. 移动端调试

```bash
# 使用 Weinre 调试移动端页面
http://www.qq.com/ weinre://test
```

### 5. 代理到本地

```bash
# 线上代理到本地调试
//xxx.cn//0002/ localhost:3334//0002/

# 代理到本地文件
https://xx.cn/lowcode/editer.js file:///path/to/local/file.js
```

### 6. 延迟响应

```bash
# 设置延迟响应时间（毫秒）
https://xxx.cn/api/xxxcms/selectById resDelay://3000
```

### 7. 模拟错误

```bash
# 修改返回码
https://xxx.cn/api/xxxcms/selectById statusCode://500
```

### 8. 注入脚本和样式

```bash
# 注入 JavaScript
https://wq.jd.com/ jsAppend://{test.js}

# 注入 CSS
https://wq.jd.com/ cssAppend://{test.css}
```

## 移动端配置

### iOS 设备

1. 连接 PC 机的热点
2. 配置代理：设置 > WiFi > 当前网络 > 配置代理 > 手动
3. 输入 PC 机 IP 和端口 8899
4. 访问 `rootca.pro` 安装证书
5. 在设置中信任证书：设置 > 通用 > 关于本机 > 证书信任设置

### Android 设备

1. 连接 PC 机的热点
2. 配置代理：设置 > WiFi > 当前网络 > 修改网络 > 高级选项 > 代理
3. 输入 PC 机 IP 和端口 8899
4. 访问 `rootca.pro` 安装证书
5. 将 WiFi 设为可信任网络

## 常见问题

1. **证书信任问题**
   - iOS 设备需要手动开启证书信任
   - 部分 iOS 版本可能无法设置证书信任
   - 建议使用较新的 iOS 版本

2. **代理连接问题**
   - 检查防火墙设置
   - 确保 PC 机和移动设备在同一网络
   - 验证 IP 地址和端口配置

3. **HTTPS 请求无法捕获**
   - 确认证书安装正确
   - 检查证书是否被信任
   - 验证 HTTPS 拦截功能是否开启

## 实用技巧

1. **微信公众号调试**
   - 配置微信开发者工具代理
   - 设置 > 代理设置 > 设置本机 IP 和端口 8899

2. **线上调试**
   - 使用 source-map 映射源码
   - 配置域名和端口
   - 代理错误 js 文件并添加 sourceMappingURL

3. **性能优化**
   - 使用 filter 规则减少不必要的请求显示
   - 合理使用延迟响应模拟网络状况
   - 利用日志分组功能进行调试

## 特殊用法

### 1. 请求重定向

```bash
# 将请求重定向到其他域名
http://example.com http://test.com

# 将请求重定向到本地文件
http://example.com file:///path/to/local/file.html

# 将请求重定向到本地目录
http://example.com dir:///path/to/local/dir
```

### 2. 请求合并

```bash
# 合并多个请求到一个请求
http://example.com/req1 http://example.com/req2 merge://{merge.json}

# merge.json 内容示例
{
  "req1": {
    "url": "http://example.com/req1",
    "method": "GET"
  },
  "req2": {
    "url": "http://example.com/req2",
    "method": "GET"
  }
}
```

### 3. 请求替换

```bash
# 替换请求内容
http://example.com replace://{replace.json}

# replace.json 内容示例
{
  "search": "old content",
  "replace": "new content",
  "type": "text/html"
}
```

### 4. 请求缓存

```bash
# 缓存请求响应
http://example.com cache://{cache.json}

# cache.json 内容示例
{
  "maxAge": 3600,  // 缓存时间（秒）
  "statusCode": 200
}
```

### 5. 请求限速

```bash
# 限制请求速度
http://example.com speed://100  // 100KB/s
```

### 6. 请求重试

```bash
# 请求失败时自动重试
http://example.com retry://3  // 重试3次
```

### 7. 请求超时设置

```bash
# 设置请求超时时间
http://example.com timeout://5000  // 5秒超时
```

### 8. 请求日志

```bash
# 记录请求日志
http://example.com log://{log.json}

# log.json 内容示例
{
  "format": "json",
  "path": "/path/to/log/file"
}
```

### 9. 请求统计

```bash
# 统计请求信息
http://example.com stats://{stats.json}

# stats.json 内容示例
{
  "format": "json",
  "path": "/path/to/stats/file"
}
```

### 10. 请求模拟

```bash
# 模拟请求响应
http://example.com mock://{mock.json}

# mock.json 内容示例
{
  "statusCode": 200,
  "headers": {
    "Content-Type": "application/json"
  },
  "body": {
    "data": "mock data"
  }
}
```

### 11. 请求转发

```bash
# 将请求转发到其他代理
http://example.com proxy://127.0.0.1:8080

# 使用多个代理
http://example.com proxy://127.0.0.1:8080,127.0.0.1:8081
```

### 12. 请求过滤

```bash
# 根据请求方法过滤
http://example.com method://GET

# 根据请求头过滤
http://example.com header://{header.json}

# header.json 内容示例
{
  "User-Agent": "Chrome"
}
```

### 13. 请求修改

```bash
# 修改请求方法
http://example.com method://POST

# 修改请求路径
http://example.com path:///new/path

# 修改请求查询参数
http://example.com query://{query.json}

# query.json 内容示例
{
  "param1": "value1",
  "param2": "value2"
}
```

### 14. 响应修改

```bash
# 修改响应状态码
http://example.com statusCode://404

# 修改响应头
http://example.com resHeaders://{headers.json}

# 修改响应内容
http://example.com resBody://{body.json}
```

### 15. 请求拦截

```bash
# 拦截请求并修改
http://example.com reqInterceptor://{interceptor.js}

# interceptor.js 内容示例
module.exports = function(req, res, next) {
  req.headers['X-Custom-Header'] = 'value';
  next();
};
```

### 16. 响应拦截

```bash
# 拦截响应并修改
http://example.com resInterceptor://{interceptor.js}

# interceptor.js 内容示例
module.exports = function(req, res, next) {
  res.headers['X-Custom-Header'] = 'value';
  next();
};
```

### 17. 请求重放

```bash
# 重放请求
http://example.com replay://{replay.json}

# replay.json 内容示例
{
  "times": 3,  // 重放次数
  "interval": 1000  // 间隔时间（毫秒）
}
```

### 18. 请求录制

```bash
# 录制请求
http://example.com record://{record.json}

# record.json 内容示例
{
  "format": "json",
  "path": "/path/to/record/file"
}
```

### 19. 请求回放

```bash
# 回放录制的请求
http://example.com playback://{playback.json}

# playback.json 内容示例
{
  "format": "json",
  "path": "/path/to/record/file"
}
```

### 20. 请求分析

```bash
# 分析请求性能
http://example.com analyze://{analyze.json}

# analyze.json 内容示例
{
  "format": "json",
  "path": "/path/to/analyze/file"
}
```

这些特殊用法可以帮助开发者更灵活地使用 Whistle 进行调试和开发。根据实际需求，可以组合使用这些功能来实现更复杂的调试场景。

## 总结

Whistle 是一个功能强大的 Web 调试代理工具，特别适合移动端开发和调试。通过合理配置和使用，可以大大提高开发效率，解决各种调试难题。建议开发者熟练掌握其基本功能和常用技巧，以便更好地服务于开发工作。 