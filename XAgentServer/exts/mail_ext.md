# FunctionDef email_content
**email_content函数**: 该函数的功能是生成一封HTML格式的电子邮件内容。

该函数接受一个用户信息的字典作为参数，该字典包含用户的姓名、电子邮件地址、令牌和用户ID。函数内部定义了一个HTML模板，其中使用了Python的格式化字符串功能（f-string）来动态插入用户的信息。这些信息包括用户的姓名、电子邮件地址、令牌，以及一个激活链接，用户需要点击这个链接来激活他们的账户。

在HTML模板中，使用了内联CSS样式来设置邮件内容的外观，如字体、背景颜色、边距、填充等。邮件的主体内容包括对用户的欢迎词、账户信息的说明、激活账户的操作指示，以及联系方式。

函数最后返回构造好的HTML邮件内容字符串。

在项目中，`email_content`函数被调用于`XAgentServer/application/routers/user.py`文件中的`register`异步函数里。在用户注册流程中，首先检查用户是否已存在，如果不存在，则创建一个包含用户信息的字典，并生成一个唯一的令牌。然后，使用`email_content`函数生成邮件内容，并根据配置决定是发送邮件还是直接激活用户账户。如果邮件发送成功，用户信息将被添加到数据库中。

**注意**：
- 在使用`email_content`函数时，需要确保传入的用户信息字典包含所有必要的键值对，如`name`、`email`、`token`和`user_id`。
- 函数返回的HTML内容需要通过支持HTML格式的邮件发送服务来发送，以确保邮件内容正确显示。
- 在实际部署时，需要替换邮件内容中的联系邮箱地址和激活链接为实际使用的值。

**输出示例**：
```html
<body style="font-family: Arial, sans-serif;background-color: #f5f5f5;margin: 0; padding: 0;">
    <div style="background-color: #ffffff;margin: 0 auto;padding: 20px;border-radius: 10px;box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);">
        <h1 style="font-size: 28px;margin-bottom: 20px;">Hello Alice,</h1>
        <p style="font-size: 16px;line-height: 1.5;color: #333333;text-indent:2em;">Welcome to XAgent, your personal assistant! Thanks for signing up for XAgent. There are some information about your account:</p>
        <p style="font-size: 16px;line-height: 1.5;color: #333333;text-indent:2em;">Your XAgent Account: <b>alice@example.com</b></p>
        <p style="font-size: 16px;line-height: 1.5;color: #333333;text-indent:2em;">Token: <b>123456abcdef</b></p>
        <p style="font-size: 16px;line-height: 1.5;color: #333333;text-indent:2em;">Next is an activation link. You need to click on this link to activate your account. After that, you will be able to use XAgent happily:<a href="http://example.com/auth?user_id=abc123&token=123456abcdef">http://example.com/auth?user_id=abc123&token=123456abcdef</a>! This Verification link will expire in 7 days.</p>
        <p>If you have any questions, please contact us at yourxagent@gmail.com .</p>
        <p style="margin-top: 20px;font-size: 14px;color: #999999;text-indent:2em;">Best wishes!</p>
        <p style="margin-top: 20px;font-size: 14px;color: #999999;">XAgent Team</p>
    </div>
</body>
```
这是一个示例输出，显示了一封格式化的欢迎邮件内容，其中包含了用户的姓名、账户信息、激活链接等。
***
