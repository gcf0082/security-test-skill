## 越权
- 请求含资源ID和用户ID

## SSRF
- 请求参数含URL地址、IP地址

## SQL注入
- 请求包含排序字段、分页参数
- 响应包含SQL执行异常信息

## 命令注入
- 通过响应看出后台执行了命令（比如显示错误信息包含执行命令）
- 请求参数涉及主机名、域名、IP、文件名且功能为 ping/traceroute/压缩/转码等

## 路径穿越
- 请求参数含 `filename`、`path`、`file`、`name`、`template`，功能为下载/预览/导出

## XXE
- 请求体 `Content-Type: application/xml`、`text/xml` 或 `application/soap+xml`
- 请求体含 `<?xml ... ?>` 声明或 `<!DOCTYPE>` 定义

## 敏感信息泄露
- 响应 JSON 包含 `password`、`token`、`apiKey`、`secretKey`、未脱敏 `phone`/`idCard`
- 错误响应堆栈含文件路径、框架版本、SQL 语句

## CSRF
- 关键操作请求（修改密码、转账、删除）仅依赖 Cookie 鉴权，无独立 CSRF Token 或自定义 Header
- 未校验 `Origin` / `Referer`
