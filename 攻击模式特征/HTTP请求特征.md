> 小节名 = 攻击名，与 `攻击模式库/<X>/` 目录名 1:1 对应。bullet 描述本层（HTTP 请求 / 响应侧）能直接观察到的、可触发该攻击的特征。

## SQL注入
- 请求含排序字段（`orderBy`、`sort`）、分页参数（`pageNum`、`limit`）等可能直接拼到 SQL 的字段
- 响应回显 SQL 异常（`SQLException`、`ORA-`、`syntax near`）

## 命令注入
- 请求参数涉及主机名、域名、IP、文件名，功能为 ping / traceroute / 压缩 / 转码 等会调用外部命令的能力
- 响应回显命令执行结果或错误（如 `sh: ... not found`）

## SSTI
- 响应回显原样模板表达式（`{{...}}`、`${...}`、`<%= %>` 未渲染），或暴露模板引擎错误（`FreemarkerException`、`Jinja2 traceback`）

## 路径穿越
- 请求参数含 `filename`、`path`、`file`、`name`、`template`，功能为下载 / 预览 / 导出

## 任意文件上传
- multipart 请求 filename 含可执行扩展（`.jsp` / `.php` / `.aspx` / `.jspx` / `.jar`）或多重扩展（`a.jpg.php`）
- filename 与 Content-Type 不一致（如声明 `image/jpeg` 但 filename 为 `.php`）
- 上传响应回显可直接访问的文件 URL

## CSV
- 响应 `Content-Type` 为 `text/csv` / `application/vnd.ms-excel` / `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`
- 响应 `Content-Disposition: attachment; filename=*.csv|.xls|.xlsx`
- 响应体单元格首字符为 `=` / `+` / `-` / `@` / `\t` / `\r`（用户输入未净化原样写入单元格）

## XXE
- 请求体 `Content-Type` 为 `application/xml`、`text/xml`、`application/soap+xml`
- 请求体含 `<?xml ... ?>` 声明或 `<!DOCTYPE>` 定义

## SSRF
- 请求参数含 URL、IP、域名（功能为远程抓取、回调、转发、健康检查）

## URL重定向
- 请求参数为完整 URL（`redirect` / `url` / `target` / `next` / `returnUrl`），响应 `Location` 头使用了该参数

## 越权
- 请求参数同时含资源 ID 与用户 ID（横向越权可替换为他人 ID）
- 路径中暴露资源主键（`/order/<id>`、`/user/<id>`），仅靠 Cookie 鉴权

## JWT缺陷
- 请求 `Authorization: Bearer eyJ...` 三段式 Token，或 Cookie 中存在三段 base64 串
- 服务端下发 JWT 但未观察到刷新机制（永不过期）

## 弱口令暴力破解
- 登录 / 找回密码 接口连续失败无锁定、无频率限制响应（429 / Retry-After）
- 响应根据账号状态返回不同错误码 / 文案（与「用户枚举」交叉）

## 验证码缺陷
- 验证码参数可置空 / 重放（多次请求复用同一验证码值仍通过）
- 短信发送接口无频率限制头（如 `Retry-After`），无图形验证前置

## 用户枚举
- 登录 / 注册 / 找回密码 接口对存在 / 不存在用户返回不同响应（错误码、消息体、HTTP 状态、响应时长可区分）

## 敏感信息
- 响应 JSON 含 `password`、`token`、`apiKey`、`secretKey`、未脱敏 `phone` / `idCard`
- 错误响应堆栈暴露文件路径、框架版本、SQL 语句
