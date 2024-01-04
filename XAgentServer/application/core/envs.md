# ClassDef XAgentServerEnv
**XAgentServerEnv功能**：这个类用于设置XAgentServer的环境变量。

XAgentServerEnv是一个包含了XAgentServer的环境变量的类。通过设置这些环境变量，可以对XAgentServer的行为进行配置。

以下是XAgentServerEnv类的详细分析和描述：

- `app`：表示XAgentServer的应用程序名称。
- `prod`：表示XAgentServer是否处于生产环境。默认为False。
- `base_dir`：表示XAgentServer的基本目录。
- `use_redis`：表示是否使用Redis作为XAgentServer的缓存数据库。默认为False。
- `recorder_root_dir`：表示XAgentServer的运行记录的根目录。
- `default_login`：表示是否使用默认用户进行登录。默认为True。
- `check_running`：表示是否检查交互是否正在运行。默认为False。
- `host`：表示XAgentServer的主机地址。默认为"0.0.0.0"。
- `port`：表示XAgentServer的端口号。默认为8090。
- `debug`：表示是否启用调试模式。默认为True。
- `reload`：表示是否在代码更改时重新加载XAgentServer。默认为True。
- `workers`：表示XAgentServer的工作进程数。默认为1。
- `share_url`：表示共享URL的地址。

`DB`类：
- `use_db`：表示是否使用数据库。默认为True。
- `db_url`：表示数据库的连接URL。

`Redis`类：
- `use_redis`：表示是否使用Redis作为缓存数据库。默认为False。
- `redis_url`：表示Redis的连接URL。
- `redis_host`：表示Redis的主机地址。默认为"localhost"。
- `redis_port`：表示Redis的端口号。默认为6379。
- `redis_db`：表示Redis的数据库编号。默认为0。
- `redis_password`：表示Redis的密码。

`Email`类：
- `send_email`：表示是否使用邮件发送消息。默认为False。
- `email_host`：表示邮件服务器的主机地址。
- `email_port`：表示邮件服务器的端口号。默认为465。
- `email_user`：表示邮件服务器的用户名。
- `email_password`：表示邮件服务器的密码。
- `auth_server`：表示邮件服务器的认证服务器。

`Upload`类：
- `upload_dir`：表示上传文件的目录路径。
- `upload_allowed_types`：表示允许上传的文件类型。

**注意**：如果更改了环境变量的值，需要重新启动XAgentServer以使更改生效。可以通过运行`python start_server.py`命令或手动启动一个unicorn服务器来重新启动XAgentServer。

请注意：
- `XAgentServerEnv`类用于配置XAgentServer的环境变量。
- 可以根据实际需求修改环境变量的值。
- 需要注意更改环境变量后需要重新启动XAgentServer。
- 可以根据需要启用或禁用数据库、Redis、邮件发送和文件上传功能。
## ClassDef DB
**DB类的功能**: 此类用于配置数据库连接信息。

DB类定义了与数据库连接相关的配置信息。它包含了两个类属性：

1. `use_db`: 一个布尔值，用于指示是否使用数据库。如果设置为True，则项目将尝试连接数据库；如果设置为False，则项目在运行时不会尝试建立数据库连接。这可以用于在不同的环境中快速切换数据库的使用状态，例如在测试环境中可能不需要连接真实的数据库。

2. `db_url`: 一个字符串，包含了数据库的连接信息。这个字符串遵循SQLAlchemy的连接字符串格式，通常包括数据库类型+数据库驱动名称://用户名:密码@主机名:端口/数据库名。在这个类中，`db_url`被设置为连接到本地MySQL数据库的URL，其中数据库类型为mysql，使用pymysql作为驱动，用户名为root，密码为xagent，主机名为localhost，端口为3306，数据库名为xagent。

**注意**:
- 在实际部署时，应该确保`db_url`中的用户名、密码和数据库名与实际环境相匹配，并且数据库服务已经启动并可以连接。
- 出于安全考虑，不应该在代码中硬编码数据库密码，而是应该使用环境变量或其他安全的方式来管理敏感信息。
- 如果项目需要连接到不同类型的数据库，或者使用不同的数据库驱动，需要相应地修改`db_url`中的内容。
- `use_db`属性可以用于控制某些环境下是否启用数据库功能，例如在单元测试中可能不希望连接到真实的数据库，可以将其设置为False。
## ClassDef Redis
**Redis类功能**: 该类用于配置Redis数据库的连接参数。

Redis类定义了一组用于连接Redis数据库的配置参数。这些参数包括Redis服务器的URL、主机名、端口号、数据库编号以及密码。这个类本身不包含方法，它的作用主要是作为一个配置容器，存储连接Redis所需的各种参数。

在代码中，Redis类包含以下属性：
- `use_redis`：一个布尔值，用于指示是否使用Redis。
- `redis_url`：一个字符串，表示Redis服务器的URL，默认为"redis://localhost"。
- `redis_host`：一个字符串，表示Redis服务器的主机名，默认为"localhost"。
- `redis_port`：一个整数，表示Redis服务器的端口号，默认为6379。
- `redis_db`：一个整数，表示要连接的Redis数据库的编号，默认为0。
- `redis_password`：一个字符串，表示连接Redis数据库所需的密码，默认为"xagent"。

在项目中，Redis类被调用于`XAgentServer/exts/redis_ext.py`文件中。在这个文件的`__init__`方法中，创建了一个Redis客户端实例，使用了Redis类中定义的配置参数。如果环境变量中设置了`REDIS_HOST`，则会优先使用环境变量的值，否则使用`Redis.redis_host`作为主机名。端口、数据库编号和密码则直接使用Redis类中的默认值。

**注意**：
- 在实际部署和使用时，应根据实际的Redis服务器配置来设置这些参数。如果有必要，可以通过环境变量来覆盖默认的主机名。
- `use_redis`参数可以用来控制是否启用Redis功能，这在某些情况下可能用于开关缓存或其他依赖Redis的功能。
- 默认的参数值仅适用于本地开发环境，生产环境中应确保安全地配置`redis_password`，避免使用默认值。
- 在使用Redis客户端时，应确保Redis服务器已经启动并且可以被正确连接。
## ClassDef Email
**Email功能**: 这个类的功能是配置邮件发送的相关参数。

Email类是用于配置邮件发送的相关参数。它包含以下属性：

- send_email: 一个布尔值，表示是否发送邮件。默认为False。
- email_host: 字符串，表示邮件服务器的主机地址。
- email_port: 整数，表示邮件服务器的端口号。默认为465。
- email_user: 字符串，表示发送邮件的用户名。
- email_password: 字符串，表示发送邮件的密码。
- auth_server: 字符串，表示认证服务器的地址。

在项目中，Email类被以下文件调用：

文件路径：XAgentServer/application/global_val.py
调用代码如下：
```python
def init_yag(logger):
    """初始化yagmail服务

    Args:
        logger (_type_): _description_
    """
    global yag
    if XAgentServerEnv.Email.send_email:
        yag = yagmail.SMTP(user=XAgentServerEnv.Email.email_user,
                           password=XAgentServerEnv.Email.email_password,
                           host=XAgentServerEnv.Email.email_host)
        logger.info("初始化yagmail")
```

文件路径：XAgentServer/application/routers/user.py
调用代码如下：
```python
async def register(email: str = Form(...),
                   name: str = Form(...),
                   corporation: str = Form(...),
                   position: str = Form(...),
                   industry: str = Form(...),
                   db: Session = Depends(get_db)) -> ResponseBody:
    """
    注册用户
    """
    if UserCRUD.is_exist(db=db, email=email):
        return ResponseBody(success=False, message="用户已存在")

    token = uuid.uuid4().hex
    user = {"user_id": uuid.uuid4().hex, "email": email, "name": name,
            "token": token, "available": False, "corporation": corporation,
            "position": position, "industry": industry,
            "create_time": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
            "update_time": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
            "is_beta": False}
    try:

        contents = email_content(user)

        if XAgentServerEnv.Email.send_email:
            from XAgentServer.application.global_val import yag
            yag.send(user["email"], 'XAgent Token Verification', contents)
        else:
            user["available"] = True
        UserCRUD.add_user(db=db, user_dict=user)
    except smtplib.SMTPAuthenticationError:
        return ResponseBody(success=False, message="邮件发送失败！", data=None)

    except Exception:
        return ResponseBody(success=False, message="注册失败", data=None)

    return ResponseBody(data=user, success=True, message="注册成功，我们将向您发送一封邮件！")
```

文件路径：XAgentServer/exts/mail_ext.py
调用代码如下：
```python
def email_content(user):
    html_body = f"""
<body style="font-family: Arial, sans-serif;background-color: #f5f5f5;margin: 0; padding: 0;">
    <div style="background-color: #ffffff;margin: 0 auto;padding: 20px;border-radius: 10px;box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);">
        <h1 style="font-size: 28px;margin-bottom: 20px;">Hello {user['name']},</h1>
        <p style="font-size: 16px;line-height: 1.5;color: #333333;text-indent:2em;">欢迎使用XAgent，您的个人助手！感谢您注册XAgent。以下是关于您的账户的一些信息：</p>
        <p style="font-size: 16px;line-height: 1.5;color: #333333;text-indent:2em;">您的XAgent账户：<b>{user["email"]}</b></p>
        <p style="font-size: 16px;line-height: 1.5;color: #333333;text-indent:2em;">您需要在后续登录时使用此令牌进行身份验证：</p>
        <p style="font-size: 16px;line-height: 1.5;color: #333333;text-indent:2em;">Token: <b>{user["token"]}</b></p>
        
        <p style="font-size: 16px;line-height: 1.5;color: #333333;text-indent:2em;">下面是一个激活链接，您需要点击此链接来激活您的账户。之后，您将能够愉快地使用XAgent：<a href="{XAgentServerEnv.Email.auth_server}/auth?user_id={user["user_id"]}&token={user["token"]}">{XAgentServerEnv.Email.auth_server}/auth?user_id={user["user_id"]}&token={user["token"]}</a>！此验证链接将在7天后过期。</p>
        <p>如果您有任何问题，请联系我们：yourxagent@gmail.com。</p>
        <p style="margin-top: 20px;font-size: 14px;color: #999999;text-indent:2em;">祝您一切顺利！</p>
        <p style="margin-top: 20px;font-size: 14px;color: #999999;">XAgent团队</p>
    </div>
</body>"""
    return html_body
```

**注意**: 在使用Email类时，需要根据实际情况配置相关参数，如邮件服务器的主机地址、端口号、用户名和密码等。在调用Email类的代码中，需要先判断是否需要发送邮件，然后根据配置的参数进行邮件发送操作。
## ClassDef Upload
**Upload类的功能**：Upload类用于处理文件上传的相关配置和操作。

Upload类具有以下属性：
- upload_dir：文件上传的目录路径。
- upload_allowed_types：允许上传的文件类型列表。

在Upload类中，首先会检查upload_dir目录是否存在，如果不存在则会创建该目录。然后定义了允许上传的文件类型列表。

**注意**：使用该类时需要注意以下几点：
- 需要确保upload_dir目录存在或能够被创建。
- 上传的文件类型需要在upload_allowed_types列表中进行配置。

在项目中的调用情况如下：

文件路径：XAgentServer/application/routers/workspace.py
调用代码如下：
```python
async def create_upload_files(files: List[UploadFile] = File(...),
                              user_id: str = Depends(user_is_available)) -> ResponseBody:
    """上传文件"""

    if len(files) == 0:
        return ResponseBody(success=False, message="文件为空！")
    if len(files) > 5:
        files = files[:5]

    if not os.path.exists(os.path.join(XAgentServerEnv.Upload.upload_dir, user_id)):
        os.makedirs(os.path.join(XAgentServerEnv.Upload.upload_dir, user_id))

    for f in files:
        if f.size > 1024 * 1024 * 1:
            return ResponseBody(success=False,
                                message="文件大小超过限制，每个文件限制为1MB！")

    file_list = []
    for file in files:
        file_name = uuid.uuid4().hex + os.path.splitext(file.filename)[-1]
        with open(os.path.join(XAgentServerEnv.Upload.upload_dir, user_id, file_name), "wb") as f:
            f.write(await file.read())
            file_list.append({"uuid": file_name, "name": file.filename})
    return ResponseBody(data={"user_id": user_id,
                              "file_list": file_list},
                        success=True, message="上传成功")
```

在该文件中，create_upload_files函数使用了Upload类的upload_dir属性，用于指定文件上传的目录路径。在文件上传前，会检查文件是否为空，文件数量是否超过限制，并根据用户ID创建对应的上传目录。然后遍历文件列表，检查文件大小是否超过限制，并将文件保存到指定的上传目录中。最后返回上传成功的响应。

文件路径：XAgentServer/server.py
调用代码如下：
```python
def interact(self, interaction: XAgentInteraction):
    # query = message
    """
    XAgent服务器启动函数
    """
    from XAgent.config import CONFIG as config
    xagent_core = None
    try:
        config.reload()
        args = {}
        # args
        args = interaction.parameter.args

        self.logger.info(
            f"服务器正在运行，开始查询为 {args.get('goal', '')}")
        xagent_param = XAgentParam()

        # 构建查询
        xagent_param.build_query({
            "role_name": "Assistant",
            "task": args.get("goal", ""),
            "plan": args.get("plan", ["注意初始目标中的语言，始终用与初始目标相同的语言回答。"]),
        })
        xagent_param.build_config(config)
        xagent_core = XAgentCoreComponents()
        # 构建XAgent核心组件
        xagent_core.build(xagent_param, interaction=interaction)
        json_str = json.dumps(
            xagent_param.config.to_dict(), indent=2)
        json_str=re.sub(r'"api_key": "(.+?)"', r'"api_key": "**"', json_str)
        self.logger.info(json_str)
        self.logger.typewriter_log(
            "Human-In-The-Loop",
            Fore.RED,
            str(xagent_param.config.enable_ask_human_for_help),
        )

        file_list = interaction.base.file_list
        for file in file_list:
            file_uuid = file.get("uuid", "")
            file_name = file.get("name", "")
            if file_uuid.startswith("/"):
                file_path = file_uuid
            else:
                file_path = os.path.join(XAgentServerEnv.Upload.upload_dir,
                                         interaction.base.user_id, file_uuid)

            upload_dir = os.path.join(
                xagent_core.base_dir, "upload")
            if not os.path.exists(upload_dir):
                os.makedirs(upload_dir)
            # 拷贝到workspace
            if interaction.call_method == "web":
                shutil.copy(file_path, os.path.join(upload_dir, file_name))
            else:
                if os.path.exists(file_path):
                    if os.path.samefile(file_path, os.path.join(upload_dir, file_name)):
                        # 文件路径相同,跳过复制
                        pass
                    else:
                        shutil.copy(file_path, os.path.join(upload_dir, file_name))
                    # shutil.copy(file_path, os.path.join(upload_dir, file_name))

            new_file = os.path.join(upload_dir, file_name)
            try:
                xagent_core.toolserver_interface.upload_file(new_file)
            except Exception as e:
                self.logger.typewriter_log(
                    "上传文件时发生错误",
                    Fore.RED,
                    f"{new_file}\n{e}",
                )
                raise XAgentUploadFileError(str(e)) from e

        match args["workflow"]:
            case "DoubleLoop":
                from XAgent import DoubleLoopFlow
                workflow = DoubleLoopFlow()
                workflow.start(goal=args.task, milestones=args.milestones)

            case "Interaction":
                from XAgent import InteractionFlow
                workflow = InteractionFlow()
                workflow.start()

            case _:
                raise NotImplementedError(f"未知的工作流程 {args['workflow']}")
    except Exception as e:
        raise XAgentRunningError(str(e)) from e
    finally:
        if xagent_core is not None:
            xagent_core.close()
```

在该文件中，interact函数使用了Upload类的upload_dir属性，用于指定文件上传的目录路径。在文件上传前，会检查文件是否为空，文件数量是否超过限制，并根据用户ID创建对应的上传目录。然后遍历文件列表，将文件拷贝到指定的上传目录中，并调用toolserver_interface.upload_file方法上传文件。最后根据不同的工作流程类型执行相应的操作。

**注意**：在使用Upload类时，需要确保upload_dir目录存在或能够被创建，并且需要根据实际需求配置upload_allowed_types属性。
***
