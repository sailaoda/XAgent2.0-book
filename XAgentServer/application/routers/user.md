# AsyncFunctionDef register
**register函数**: 该函数的功能是注册新用户。

该`register`函数是一个异步函数，用于处理用户注册的逻辑。它接收几个表单参数：`email`（电子邮件地址），`name`（用户姓名），`corporation`（公司名称），`position`（职位），`industry`（行业），以及一个数据库会话`db`。函数返回一个`ResponseBody`对象，该对象包含注册操作的结果。

函数首先检查数据库中是否已存在相同电子邮件地址的用户。如果存在，则返回一个`ResponseBody`对象，其`success`属性为`False`，并带有消息"user is already exist"（用户已存在）。

如果用户不存在，函数将生成一个新的UUID作为用户的唯一标识符`user_id`，以及一个用于验证的`token`。然后，它创建一个包含用户信息的字典`user`，包括用户ID、电子邮件、姓名、验证令牌、公司名称、职位、行业以及创建和更新时间。

接下来，函数尝试通过电子邮件发送验证信息。如果`XAgentServerEnv.Email.send_email`配置为`True`，则使用`yag`（一个全局邮件发送对象）发送验证邮件。如果邮件发送失败，会捕获`smtplib.SMTPAuthenticationError`异常，并返回一个`ResponseBody`对象，其`success`属性为`False`，消息为"email send failed!"（邮件发送失败！）。

如果在发送邮件或添加用户到数据库的过程中发生任何其他异常，函数将捕获通用的`Exception`异常，并返回一个`ResponseBody`对象，其`success`属性为`False`，消息为"register failed"（注册失败）。

如果一切顺利，用户信息将被添加到数据库中，并返回一个`ResponseBody`对象，其`success`属性为`True`，消息为"Register success, we will send a email to you!"（注册成功，我们将向您发送一封电子邮件！），并包含用户信息。

**注意**：
- 函数使用了`uuid.uuid4().hex`来生成唯一的用户ID和验证令牌。
- 函数使用了`datetime.now().strftime("%Y-%m-%d %H:%M:%S")`来记录用户的创建和更新时间。
- 邮件发送功能依赖于`XAgentServerEnv.Email.send_email`的配置，以及`yag`邮件发送对象。
- 函数中的异常处理确保了即使在出现错误的情况下，也能返回一个明确的响应给调用者。

**输出示例**：
如果用户注册成功，并且邮件发送功能被启用，返回的`ResponseBody`可能如下所示：
```json
{
  "data": {
    "user_id": "生成的UUID",
    "email": "用户的电子邮件",
    "name": "用户姓名",
    "token": "生成的验证令牌",
    "available": false,
    "corporation": "公司名称",
    "position": "职位",
    "industry": "行业",
    "create_time": "创建时间",
    "update_time": "更新时间",
    "is_beta": false
  },
  "success": true,
  "message": "Register success, we will send a email to you!"
}
```
如果用户已存在，返回的`ResponseBody`可能如下所示：
```json
{
  "success": false,
  "message": "user is already exist"
}
```
***
# AsyncFunctionDef auth
**auth函数**: 该函数的功能是进行用户认证。

该函数`auth`是一个异步函数，用于验证用户的身份。它接收三个参数：`user_id`、`token`和`db`。其中`user_id`和`token`通过查询参数（Query）传递，`db`是一个数据库会话对象，通过依赖注入（Depends）获得。

函数首先使用`UserCRUD.get_user`方法尝试从数据库中获取与`user_id`匹配的用户。如果没有找到用户，函数将返回一个`ResponseBody`对象，其中`success`字段为`False`，并带有消息"user is not exist"。

如果用户存在，函数将继续检查提供的`token`是否与用户的`token`相匹配。如果不匹配，同样返回一个`ResponseBody`对象，`success`为`False`，消息为"token is not correct"。

接下来，函数会检查`token`是否过期。它通过计算当前时间与用户最后更新时间的差值来判断。如果超过了7天（60秒 * 60分钟 * 24小时 * 7天），则认为`token`已过期，并返回相应的`ResponseBody`对象，消息为"token is expired"。

如果`token`验证通过，函数还会检查用户的`available`字段。如果用户当前不可用（`available`为`False`），则将其设置为`True`，更新用户的`update_time`为当前时间，并调用`UserCRUD.update_user`方法更新数据库中的用户信息。如果用户已经是可用状态，则返回一个`ResponseBody`对象，`success`为`False`，消息为"user is already available!"。

最后，如果所有验证都通过，函数将返回一个`ResponseBody`对象，其中`data`字段包含用户信息的字典表示，`success`为`True`，消息为"auth success"。

**注意**：
- 该函数需要与数据库交互，因此在使用时需要确保数据库连接是有效的。
- `user_id`和`token`是通过查询参数传递的，因此在调用此函数时需要在URL中提供这些参数。
- 函数返回的`ResponseBody`对象中的`success`字段可用于判断认证是否成功，`message`字段提供了认证失败的原因。

**输出示例**：
如果用户认证成功，可能的返回值如下：
```json
{
  "data": {
    "user_id": "123456",
    "username": "example_user",
    "email": "user@example.com",
    // 其他用户信息字段
  },
  "success": true,
  "message": "auth success"
}
```
如果用户认证失败，可能的返回值如下：
```json
{
  "data": null,
  "success": false,
  "message": "user is not exist" // 或其他失败原因
}
```
***
# AsyncFunctionDef login
**login函数**: 该函数的功能是实现用户登录验证。

该`login`函数是一个异步函数，用于处理用户登录请求。它接收三个参数：`email`、`token`和`db`。其中，`email`和`token`通过表单提交，而`db`是通过依赖注入得到的数据库会话。

函数首先使用`UserCRUD.get_user`方法尝试从数据库中获取与提供的`email`相匹配的用户信息。如果没有找到对应的用户，函数将返回一个`ResponseBody`对象，其中`success`字段为`False`，并附带消息"user is not exist"。

如果用户存在，函数将继续检查提交的`token`是否与数据库中存储的`token`相匹配。如果不匹配，同样返回一个`ResponseBody`对象，`success`字段为`False`，消息为"token is not correct"。

接下来，函数检查用户的`available`字段是否为`True`，即用户是否可用。如果用户不可用，它将返回一个`success`为`False`，消息为"user is not available"的`ResponseBody`对象。

如果用户存在，`token`正确，且用户状态为可用，函数将返回一个`success`为`True`，消息为"login success"的`ResponseBody`对象，并将用户信息以字典形式放在`data`字段中。

**注意**：
- 该函数是异步的，因此在调用时需要使用`await`关键字。
- 函数返回的是一个`ResponseBody`对象，而不是直接的数据结构，这有助于标准化响应格式。
- 函数中的错误消息应根据实际情况进行国际化处理，以便支持多语言环境。

**输出示例**：
如果用户不存在，可能的返回值为：
```json
{
  "success": false,
  "message": "user is not exist",
  "data": null
}
```
如果`token`不正确，可能的返回值为：
```json
{
  "success": false,
  "message": "token is not correct",
  "data": null
}
```
如果用户不可用，可能的返回值为：
```json
{
  "success": false,
  "message": "user is not available",
  "data": null
}
```
如果登录成功，可能的返回值为：
```json
{
  "success": true,
  "message": "login success",
  "data": {
    // 用户信息的字典形式
  }
}
```
***
# AsyncFunctionDef check
**check函数**: 此函数的功能是检查令牌是否有效。

该`check`函数是一个异步函数，用于验证传入的令牌（token）是否有效。它接受两个参数：`token`和`db`。`token`是一个字符串，通过表单（Form）提交，而`db`是一个数据库会话实例，通过依赖注入（Depends）获得数据库连接。

函数首先检查`token`是否为`None`。如果是，函数将返回一个`ResponseBody`对象，其中`success`属性设置为`False`，并附带消息"token is none"，表示没有提供令牌。

如果`token`不为`None`，函数将调用`UserCRUD.token_is_exist`方法来检查数据库中是否存在该令牌。`UserCRUD.token_is_exist`方法接受数据库会话`db`、令牌`token`和用户ID`user_id`（在此处设置为`None`）作为参数，并返回一个布尔值，指示令牌是否存在。

如果`result`为`True`，即令牌存在，函数将返回一个`ResponseBody`对象，其中`data`属性包含`result`的值，`success`属性设置为`True`，并附带消息"token is effective"，表示令牌有效。

如果`result`为`False`，即令牌不存在，函数将返回一个`ResponseBody`对象，其中`data`属性包含`result`的值，`success`属性设置为`False`，并附带消息"token is invalid"，表示令牌无效。

**注意**：使用此函数时，需要确保`Form`和`Depends`等依赖项已正确配置，并且`UserCRUD.token_is_exist`方法能够正确访问数据库以验证令牌。

**输出示例**:
如果令牌不存在，可能的返回值示例为：
```json
{
  "data": false,
  "success": false,
  "message": "token is invalid"
}
```
如果令牌存在，可能的返回值示例为：
```json
{
  "data": true,
  "success": true,
  "message": "token is effective"
}
```
如果未提供令牌，可能的返回值示例为：
```json
{
  "success": false,
  "message": "token is none"
}
```
***
