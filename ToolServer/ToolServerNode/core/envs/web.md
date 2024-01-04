# ClassDef WebEnv
**WebEnv函数**: 这个类的功能是提供网页浏览工具。

WebEnv类是BaseEnv类的子类，它提供了与网页浏览相关的功能。它包含了一些方法来操作浏览器、渲染页面、点击元素、滚动页面、输入文本等。

- **__init__(self, config: dict = {})**: 构造函数，用于初始化WebEnv对象。它接受一个配置字典作为参数，并调用父类的构造函数进行初始化。它还初始化了一些属性，如超时时间、页面元素列表等。

- **_check_browser(self)**: 检查浏览器是否准备就绪。如果浏览器还没有设置完成，则等待设置完成。如果页面为空，则重新设置浏览器。

- **_render_page(self) -> Image.Image**: 将当前页面渲染为图像。返回当前页面的图像。

- **_render_page_and_encode(self) -> str**: 将当前页面渲染为图像并编码为base64格式。返回当前页面的base64编码图像。

- **_render_page_with_elems_bbox(self) -> str**: 将当前页面渲染为图像，并在元素周围绘制边界框，然后将其编码为base64格式。返回当前页面的base64编码图像。

- **_setup_browser(self) -> playwright.async_api.Browser**: 设置或重新启动浏览器实例。如果已经存在浏览器实例，则关闭它并停止playwright。然后启动一个新的浏览器实例，并创建一个新的页面。

- **_extract_elements(self) -> list[dict]**: 从当前页面提取元素。使用JavaScript代码从页面中提取元素，并返回元素列表。

- **_look_page(self)**: 查看当前页面。等待页面加载完成，并返回包含页面截图、元素列表和页面内容的字典。

- **goto(self, url: str)**: 转到指定的URL。加载指定的URL，并返回包含页面截图、元素列表和页面内容的字典。

- **_click_by_xpath(self, xpath: str)**: 根据给定的xpath点击元素。如果在给定的元素列表中找不到元素，则尝试在页面中查找元素。使用JavaScript代码获取元素的边界框，并进行点击操作。

- **click(self, elem_xpath: str = None, position: dict = None)**: 点击网页上的目标元素或位置。如果未提供elem_xpath和position，则点击鼠标光标下的元素。如果提供了elem_xpath，则点击指定的元素。如果提供了position，则点击指定的位置。返回包含页面截图、元素列表和页面内容的字典。

- **scroll(self, x: float = 0.0, y: float = 0.6)**: 滚动网页。根据提供的x和y参数滚动页面。返回包含页面截图、元素列表和页面内容的字典。

- **typing(self, elem_xpath: str = None, text: str = None, press: str = None)**: 输入文本并触发键盘事件。如果提供了elem_xpath，则点击指定的元素后输入文本。如果提供了text，则输入指定的文本。如果提供了press，则触发指定的键盘事件。返回包含页面截图、元素列表和页面内容的字典。

**注意**: 使用该类时需要注意以下几点：
- 在使用click、scroll和typing方法时，可以通过提供elem_xpath参数来指定要操作的元素。elem_xpath应该在给定的元素列表中。
- 在使用click方法时，可以通过提供position参数来指定要点击的位置。position应该是一个包含x和y坐标的字典，表示相对于页面宽度和高度的百分比。
- 在使用typing方法时，可以通过提供press参数来触发键盘事件。press应该是一个表示要按下的键的字符串，可以是单个键或多个键的组合，用加号分隔。

**输出示例**:
以下是可能的返回值的示例：
```
{
    "type": "composite",
    "data": [
        {
            "type": "binary",
            "media_type": "image/png",
            "data": "base64_encoded_image_data"
        },
        {
            "type": "simple",
            "data": [
                {"index": 0, "tagName": "div", "text": "Hello", "xpath": "//div[1]"},
                {"index": 1, "tagName": "a", "text": "World", "xpath": "//a[1]"}
            ]
        },
        {
            "type": "simple",
            "data": "<html><body><div>Hello</div><a href='https://example.com'>World</a></body></html>"
        }
    ]
}
```
## FunctionDef __init__
**__init__ 函数**: 该函数的作用是初始化一个Web环境对象。

该函数是一个构造函数，用于初始化Web环境相关的配置和属性。它接受一个可选的字典参数 `config`，默认为空字典。函数的主要任务包括：

1. 调用父类的构造函数来进行基础的初始化操作。
2. 从传入的配置字典 `config` 中提取 `web` 相关的配置，并将其存储在 `self.web_cfg` 属性中。
3. 从 `self.web_cfg` 中获取 `timeout` 设置，并将其存储在 `self.timeout` 属性中。
4. 创建一个异步任务 `self.setup_task`，该任务负责设置浏览器环境，任务名称为 "setup_browser"。
5. 初始化 `self.playwright`、`self.browser` 和 `self.page` 为 `None`，这些属性将在后续步骤中被赋予实际的 Playwright 对象。
6. 初始化 `self.page_elements` 为空列表，该列表用于存储页面元素的信息，例如元素索引、CSS选择器、XPath、边界框、标签名和内部文本等。
7. 读取并存储 `self.elements_extract_js_code` 属性，该属性包含从同级目录下的 `js` 文件夹中读取的 `web_elements.js` 文件内容。这段 JavaScript 代码用于在页面中提取元素信息。

**注意**：
- 在使用该构造函数时，需要确保传入的 `config` 字典包含正确的 `web` 配置项，否则可能会引发 KeyError。
- `self.setup_task` 是一个异步任务，需要在合适的时机等待其完成，以确保浏览器环境正确设置。
- `self.elements_extract_js_code` 的内容是从文件中读取的，因此需要确保 `web_elements.js` 文件存在于正确的路径下。
- 由于涉及到异步编程，使用该对象时需要注意异步上下文和事件循环的管理。
## AsyncFunctionDef _check_browser
**_check_browser 函数**: 该函数的功能是检查浏览器是否准备就绪。

这个异步函数`_check_browser`是`ToolServerNode`项目中`web.py`模块的一部分，它的主要作用是在执行网页操作之前确保浏览器环境已经正确设置并准备好。该函数执行以下步骤：

1. 首先，函数检查`setup_task`（一个异步任务，通常用于设置浏览器环境）是否已经完成。如果该任务尚未完成，函数将等待直到任务完成。
2. 然后，函数检查`self.page`（代表当前浏览器页面的对象）是否为`None`。如果是，说明浏览器尚未设置，函数将调用`_setup_browser`方法来进行浏览器的初始化设置。

在项目中，`_check_browser`函数被多个其他异步方法调用，以确保在执行如导航到新URL、点击页面元素、滚动页面或输入文本等操作前，浏览器已经处于就绪状态。以下是一些调用该函数的例子：

- `goto`方法在导航到指定的URL之前调用`_check_browser`。
- `click`方法在点击页面上的元素或指定位置之前调用`_check_browser`。
- `scoll`方法在滚动页面到指定位置之前调用`_check_browser`。
- `typing`方法在在页面上进行键盘输入之前调用`_check_browser`。

通过这种方式，`_check_browser`函数作为一个前置检查步骤，确保了浏览器环境的稳定性和操作的可靠性。

**注意**：
- `_check_browser`函数是一个内部函数，通常不应该直接从模块外部调用。
- 在使用`_check_browser`函数时，需要注意它是一个异步函数，因此在调用时需要使用`await`关键字。
- 由于`_check_browser`函数依赖于类实例中的`setup_task`和`page`属性，因此在调用该函数之前，确保这些属性已经被正确初始化。
- `_check_browser`函数的调用者需要处理可能发生的异步操作异常。
## AsyncFunctionDef _render_page
**_render_page 函数**: 该函数的功能是渲染当前网页为图像。

_render_page 函数是一个异步函数，它的主要作用是将当前浏览器中的网页截图并将其转换为图像对象。该函数没有接收任何参数，并返回一个 Image.Image 对象，这个对象代表了当前网页的图像。

在该函数的实现中，首先使用 `self.page.screenshot` 方法异步地获取当前网页的截图，截图的类型设置为 "jpeg"。然后，使用 `Image.open` 方法将截图的字节数据转换为一个 PIL（Python Imaging Library）图像对象。这个图像对象随后被返回给调用者。

在项目中，_render_page 函数被 _render_page_with_elems_bbox 函数调用。_render_page_with_elems_bbox 函数的目的是在网页截图上绘制元素的边界框（bbox），并将带有边界框的截图编码为 base64 字符串。在调用 _render_page 函数获取网页截图后，_render_page_with_elems_bbox 函数会使用 ImageDraw.Draw 在截图上绘制边界框和元素索引。这些绘制的边界框和索引用于可视化网页上的元素位置。

**注意**：
- _render_page 函数是一个异步函数，因此在调用时需要使用 `await` 关键字。
- 该函数依赖于 `self.page` 对象，这意味着在调用该函数之前，应该已经有一个有效的页面对象。
- 返回的图像对象是 PIL 图像对象，可以进一步用于图像处理或保存到文件中。
- 在使用该函数时，需要确保已经安装了相关的库，如 `io` 和 `PIL`。

**输出示例**：
由于函数返回的是一个 PIL 图像对象，输出示例将取决于当前网页的内容。如果网页是一个包含文本和图像的简单页面，返回的图像对象将是该网页的视觉呈现。如果要查看实际的图像，可以将图像对象保存到文件中，例如使用以下代码：
```python
image = await _render_page()
image.save('screenshot.jpeg')
```
保存后，可以在文件系统中找到名为 "screenshot.jpeg" 的文件，它包含了网页的截图。
## AsyncFunctionDef _render_page_and_encode
**_render_page_and_encode 函数**: 此函数的功能是将当前页面渲染成图像，并将其编码为 base64 格式的字符串。

此函数是一个异步函数，它的作用是捕获当前网页的截图，并将截图转换为 base64 编码的字符串。这通常用于在不需要直接存储图像文件的情况下，将图像数据以文本形式传输或存储。

函数定义中的 `self` 参数表明这是一个类的实例方法。`self.page` 是该实例中的一个属性，它代表了当前的网页对象。这个网页对象应该提供了一个 `screenshot` 方法来捕获网页的截图。

函数内部的 `await self.page.screenshot(type="jpeg")` 表示异步地调用 `screenshot` 方法，并指定截图的类型为 JPEG 格式。这个调用将返回一个包含 JPEG 图像数据的字节串。

接下来，`base64.b64encode(screenshot)` 将截图的字节串编码为 base64 格式。`base64` 是一个用于编码和解码 base64 数据的模块。`b64encode` 函数接受字节串作为输入，并返回 base64 编码的字节串。

最后，`.decode('utf-8')` 将 base64 编码的字节串解码为 UTF-8 格式的字符串，这样就可以作为文本处理和传输了。

**注意**:
- 由于 `_render_page_and_encode` 是一个异步函数，调用它时需要使用 `await` 关键字，或者在其他异步函数中调用。
- 确保在调用此函数之前，`self.page` 已经是一个有效的网页对象，并且已经加载了需要截图的页面。
- 如果在执行过程中网页内容发生变化，截图将捕获变化的瞬间状态，因此确保在稳定的页面状态下调用此函数。

**输出示例**:
```python
"data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAgGBgcGBQgHBwcJCQgKDBQNDAsLDBkSEw8UHRofHh0aHBwgJC4nICIsIxwcKDcpLDAxNDQ0Hyc5PTgyPC4zNDL/2wBDAQkJCQwLDBgNDRgyIRwhMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjL/wAARCAAKAAoDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJyAlKUJMEB4f/..."
```
这是一个经过 base64 编码的 JPEG 图像数据的字符串示例，实际输出将取决于网页截图的内容。
## AsyncFunctionDef _render_page_with_elems_bbox
**_render_page_with_elems_bbox 函数**: 此函数的功能是渲染当前页面为图像，并为页面元素绘制边界框（bbox），然后将图像编码为base64字符串。

该函数首先调用 `_render_page` 方法来获取当前页面的屏幕截图。然后，它使用 `ImageDraw.Draw` 创建一个可以在截图上绘制的对象。接下来，它调用 `_extract_elements` 方法来获取页面上所有元素的信息，包括它们的边界框（bbox）。

对于每个元素，函数会根据元素的边界框信息在截图上绘制一个矩形框。矩形框的颜色被设置为品红色（"magenta"），线宽为2像素。

接着，函数尝试加载 "OpenSans-Regular.ttf" 字体来绘制元素的索引号。如果加载失败，则使用默认字体。索引号被绘制在元素的边界框中心，并且背景也被填充为品红色以提高可读性。

最后，函数将绘制好的截图保存到一个字节流对象中，并将其编码为JPEG格式。然后，它将字节流对象转换为base64编码的字符串，并返回这个字符串。

在项目中，`_render_page_with_elems_bbox` 函数被 `_look_page` 方法调用。`_look_page` 方法用于查看当前页面，并等待页面加载完成。它返回一个包含三种类型数据的复合结构：一个是包含元素边界框的图像数据，一个是页面元素的简单信息列表，还有一个是页面的HTML内容。

**注意**：
- 在使用此函数时，需要确保 `_render_page` 和 `_extract_elements` 方法能够正确工作，因为它们为 `_render_page_with_elems_bbox` 提供必要的数据。
- 如果系统中没有 "OpenSans-Regular.ttf" 字体文件，将使用默认字体来绘制索引号，这可能会影响图像中文本的可读性。
- 由于函数使用了异步编程模式，调用此函数时需要在异步环境中使用 `await` 关键字。

**输出示例**：
函数返回的可能是一个长字符串，如下所示：
```
"data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAgGBgcG..."
```
这是一个JPEG图像的base64编码，可以直接在Web页面上以图像形式显示或用于其他需要图像数据的场合。
## AsyncFunctionDef _setup_browser
**_setup_browser 函数**: 此函数的功能是设置或重启一个浏览器实例。

_setup_browser 函数是一个异步函数，它负责在 ToolServerNode 的 web 环境中初始化或重新启动一个浏览器实例。该函数使用 Playwright 库来启动 Chromium 浏览器，并进行相应的配置。

详细代码分析如下：
1. 函数首先检查 `self.playwright` 是否已经存在。如果存在，它会关闭当前的浏览器实例并停止 Playwright。
2. 使用 `playwright.async_api.async_playwright().start()` 异步启动 Playwright，并将其实例保存到 `self.playwright`。
3. 接着，使用 Playwright 启动 Chromium 浏览器实例，配置为无头模式（headless=True），并传入浏览器参数 `self.web_cfg['browser_args']`。
4. 创建一个新的页面（Page），并设置用户代理（user_agent）和屏幕尺寸（screen），这些配置也来源于 `self.web_cfg`。
5. 函数返回创建的浏览器实例 `self.browser`。

在项目中，_setup_browser 函数被调用的情况如下：
- 在 `web.py` 文件的 `__init__` 方法中，通过 `asyncio.create_task` 创建了一个异步任务来执行 `_setup_browser` 函数，任务名称为 "setup_browser"。这样做可以在对象初始化时就开始准备浏览器实例，而不需要等待直到首次需要浏览器时才开始设置。
- 在 `_check_browser` 异步方法中，会检查 `_setup_browser` 函数对应的异步任务是否已完成。如果未完成，则会等待其完成。如果 `self.page` 为 None，也会调用 `_setup_browser` 函数来确保浏览器实例被正确设置。

**注意**：
- 由于 `_setup_browser` 是一个异步函数，调用它时需要使用 `await` 关键字。
- 在使用 `_setup_browser` 函数之前，确保相关的配置（如 `self.web_cfg`）已经正确设置。
- 由于 Playwright 和浏览器实例的启动可能会消耗较多资源和时间，应当合理安排其启动时机，避免不必要的性能开销。

**输出示例**：
由于 `_setup_browser` 函数返回的是 Playwright 的 Browser 实例，其具体的输出示例取决于 Playwright 库的实现。通常，它会是一个包含了浏览器操作方法和属性的对象，允许用户对浏览器进行进一步的操作，如打开新页面、关闭浏览器等。
## AsyncFunctionDef _extract_elements
**_extract_elements函数**: 该函数的功能是从当前页面中提取元素。

该函数`_extract_elements`是一个异步函数，它的主要作用是从当前网页中提取元素信息，并返回一个包含元素字典的列表。这个过程可能会因为超时而失败，如果发生超时，函数会记录一条错误日志，并抛出异常。

详细代码分析如下：
1. 函数首先尝试执行`self.page.evaluate(self.elements_extract_js_code)`，这是一个异步操作，用于执行页面中的JavaScript代码，该代码负责收集页面元素的信息。这里的`self.elements_extract_js_code`是一个属性，它应该在类的其他部分定义，包含了实际提取页面元素所需的JavaScript代码。
2. `asyncio.wait_for`用于设置一个超时时间`self.timeout`，如果在指定时间内页面元素没有被成功提取，将会抛出`asyncio.TimeoutError`异常。
3. 如果提取成功，函数会遍历所有元素，并给每个元素字典添加一个`'index'`键，其值为元素在列表中的索引。
4. 提取到的元素信息被保存在`self.page_elements`属性中，最后函数返回这个元素列表。

在项目中调用情况分析：
- `_extract_elements`函数在`_render_page_with_elems_bbox`函数中被调用，后者的功能是渲染当前页面为图片，并在元素上绘制边界框（bbox），然后将图片编码为base64字符串。
- 在`_render_page_with_elems_bbox`中，首先调用`_extract_elements`函数获取页面元素，然后遍历这些元素，并在图片上绘制边界框和索引号。

**注意**：
- 在使用`_extract_elements`函数时，需要确保`self.page`是一个有效的页面对象，并且`self.elements_extract_js_code`已经正确设置了提取元素所需的JavaScript代码。
- 函数执行过程中可能会因为网络延迟或页面加载速度而导致超时异常，调用者需要妥善处理这种异常情况。

**输出示例**：
```python
[
    {'index': 0, 'name': 'button', 'id': 'submit', 'class': 'btn btn-primary', ...},
    {'index': 1, 'name': 'input', 'id': 'email', 'class': 'form-control', ...},
    ...
]
```
上述示例展示了函数可能返回的元素列表的样子，每个元素是一个包含其属性和索引的字典。
## AsyncFunctionDef _look_page
**_look_page函数**：该函数用于查看当前页面。

该函数的具体代码如下：
```python
async def _look_page(self):
    """Look at the current page."""
    await self.page.wait_for_timeout(self.web_cfg["wait_time"])
    done, pending = await asyncio.wait([
        self.page.wait_for_load_state("domcontentloaded"),
        self.page.wait_for_load_state("load"),
        self.page.wait_for_load_state("networkidle")
    ], timeout=self.timeout/1000)
    for task in pending:
        task.cancel()

    return {
        "type": "composite",
        "data": [
            {
                "type": "binary",
                "media_type": "image/png",
                "data": await self._render_page_with_elems_bbox()
            },
            {
                "type": "simple",
                "data": [{"index": index, "tagName": elem["tagName"], "text": elem["text"], "xpath": elem["xpath"]} for index, elem in enumerate(self.page_elements)]
            },
            {
                "type": "simple",
                "data": await self.page.content()
            }
        ]
    }
```

**注意**：在使用该函数时需要注意以下几点：
- 该函数需要在已经初始化浏览器的情况下调用。
- 函数会等待页面加载完成后再进行截图和数据提取操作。
- 函数会返回一个包含页面截图和元素信息的字典。

**输出示例**：以下是该函数可能返回值的示例：
```python
{
    "type": "composite",
    "data": [
        {
            "type": "binary",
            "media_type": "image/png",
            "data": "base64_encoded_image_data"
        },
        {
            "type": "simple",
            "data": [
                {"index": 0, "tagName": "div", "text": "Hello", "xpath": "//div[1]"},
                {"index": 1, "tagName": "p", "text": "World", "xpath": "//p[1]"}
            ]
        },
        {
            "type": "simple",
            "data": "<html><body><div>Hello</div><p>World</p></body></html>"
        }
    ]
}
```

以上是对`_look_page`函数的详细分析和描述，该函数用于查看当前页面，并返回页面截图和元素信息。在使用该函数时需要注意已初始化浏览器，并等待页面加载完成后再进行操作。函数的返回值是一个包含页面截图和元素信息的字典。
## AsyncFunctionDef goto
**goto 函数**: 此函数的功能是导航到指定的网址。

goto 函数是一个异步方法，它的作用是让浏览器页面跳转到提供的网址（URL）。这个函数接受一个字符串参数 `url`，这个参数指定了浏览器需要导航到的网址。

函数的详细分析如下：
- 首先，函数通过 `await self._check_browser()` 确保浏览器实例是有效且已经启动的。这个内部方法可能会进行浏览器的初始化或检查浏览器的状态。
- 然后，函数使用 `await self.page.goto(url)` 异步调用页面的 `goto` 方法，实现页面的跳转。这里的 `self.page` 应该是一个表示浏览器页面的对象，而 `goto` 方法是用来导航到新的URL。
- 最后，函数通过 `return await self._look_page()` 返回页面的状态。`_look_page` 方法可能是用来获取页面的某些信息，比如页面加载完成后的截图或页面的HTML内容。

**注意**：
- 由于 `goto` 函数是异步的，调用它时需要使用 `await` 关键字，这意味着它必须在一个异步环境中使用，比如在 `async def` 定义的异步函数中。
- 函数调用者需要处理可能发生的异常，例如在页面加载时网络问题或URL无效可能会导致异常。
- 函数的返回值取决于 `_look_page` 方法的实现，因此在使用时需要了解该方法返回的具体内容。

**输出示例**：
由于文档中没有提供 `_look_page` 方法的具体实现，我们无法给出确切的返回值示例。然而，我们可以假设它可能返回页面的截图或者页面的HTML内容，例如：

```python
# 假设的返回值示例，实际返回值取决于 _look_page 方法的实现
{
    'screenshot': 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA...',
    'html': '<html><head><title>Example Page</title></head><body>...</body></html>'
}
```
## AsyncFunctionDef _click_by_xpath
**_click_by_xpath 函数**: 此函数的功能是点击给定xpath的元素。

_click_by_xpath 函数是一个异步函数，它的主要作用是在网页中点击指定的xpath元素。函数接收一个参数 `xpath`，这个参数是一个字符串，代表要点击的元素的xpath路径。

函数的执行流程如下：
1. 首先，函数会尝试在当前页面的元素列表 `self.page_elements` 中查找与给定 `xpath` 匹配的元素。
2. 如果没有找到匹配的元素，函数会记录一个错误日志，并尝试直接在页面中通过执行JavaScript脚本来查找该元素。脚本会获取元素的边界框（bounding box）信息，包括元素的x、y坐标和宽度、高度。
3. 如果通过JavaScript脚本仍然无法找到元素，函数会抛出一个 `ToolExecutionError` 异常，提示无法在给定的元素列表中找到指定的xpath。
4. 如果找到了元素，函数会获取该元素的边界框信息，然后计算元素中心的坐标。
5. 最后，函数会使用 `self.page.mouse.click` 方法，根据计算出的中心坐标来点击元素。

此函数在 `ToolServer/ToolServerNode/core/envs/web.py` 文件中被调用两次：
- 在 `click` 方法中，如果提供了 `elem_xpath` 参数，则会调用 `_click_by_xpath` 函数来点击指定的元素。
- 在 `typing` 方法中，如果提供了 `elem_xpath` 参数，则在输入文本或按键之前，会先调用 `_click_by_xpath` 函数来点击指定的元素。

**注意**：
- 在使用此函数时，需要确保传入的 `xpath` 参数是正确的，并且对应的元素存在于当前页面中。
- 由于此函数是异步的，调用时需要使用 `await` 关键字。
- 如果页面中没有找到对应的元素，函数将抛出异常，因此调用此函数的代码应当处理可能出现的异常。

**输出示例**：
由于 `_click_by_xpath` 函数的目的是触发点击事件，而不是返回值，因此它没有直接的返回值。但是，如果点击成功，页面上的元素可能会发生变化，例如触发表单提交或导航到新的页面。如果点击失败，函数将抛出异常。
## AsyncFunctionDef click
**click函数**: 该函数的功能是在网页上点击目标元素或位置，并在点击后返回当前页面的截图，包括元素的边界框和索引。

click函数是一个异步方法，用于在网页上执行点击操作。它可以根据提供的XPath表达式点击特定元素，或者根据给定的位置信息点击页面上的相对位置。如果既没有提供元素的XPath也没有提供位置信息，它将点击鼠标光标当前所在的元素。

- `elem_xpath` 参数是一个可选的字符串，用于指定要点击的元素的XPath。如果提供了此参数，函数将尝试点击XPath对应的元素。
- `position` 参数是一个可选的字典，用于指定要点击的位置。该字典包含两个键"x"和"y"，它们的值是相对于页面宽度和高度的比例。例如，`{"x": 0.5, "y": 0.3}`表示点击页面宽度的50%和高度的30%的位置。

函数首先会调用`_check_browser`方法来确保浏览器已经准备好。如果提供了`elem_xpath`，它会调用`_click_by_xpath`方法来点击指定的元素。如果提供了`position`，它会计算出实际的像素位置并通过页面的`mouse.click`方法来执行点击操作。如果两者都没有提供，它将模拟鼠标按下和释放的动作来执行点击。

无论哪种情况，函数最后都会调用`_look_page`方法来获取当前页面的截图，并返回这个截图。

**注意**:
- 在使用`position`参数时，应始终使用相对位置，即"x"和"y"的值应该是0到1之间的小数，代表相对于页面宽度和高度的比例。
- 函数是异步的，因此在调用时需要使用`await`关键字。
- 确保在调用此函数之前已经有一个有效的浏览器会话。

**输出示例**:
函数执行后可能返回的结果是一个包含页面截图的对象，其中可能还包含了页面上元素的边界框和索引信息。这个返回值的具体内容取决于`_look_page`方法的实现细节。
## AsyncFunctionDef scoll
**scoll函数**: 该函数的功能是滚动网页。

scoll函数是一个异步函数，用于在网页中进行滚动操作。它接受两个参数x和y，这两个参数分别代表要滚动到的页面宽度和高度的百分比。默认情况下，x参数为0.0，即页面不会在水平方向上滚动；y参数默认为0.6，即页面将在垂直方向上滚动到60%的位置。

函数首先调用`_check_browser`方法来确保浏览器已经正确初始化。然后，它会根据传入的x和y参数，以及配置中定义的屏幕宽度和高度，计算出需要滚动的具体像素值。计算方法是将屏幕宽度和高度乘以相应的百分比。

接下来，函数使用`page.mouse.wheel`方法来执行滚动操作，传入计算出的x和y像素值。最后，函数调用`_look_page`方法，可能是用于更新页面视图或进行其他后续处理，并将该方法的返回值作为scoll函数的返回值。

**注意**:
- 由于scoll函数是异步的，调用它时需要使用`await`关键字。
- x和y参数是可选的，如果不传入这两个参数，函数将使用默认值。
- x和y参数应该是介于0到1之间的浮点数，代表百分比。
- 在调用此函数之前，应确保已经有一个有效的浏览器会话和页面实例。

**输出示例**:
由于`_look_page`方法的具体实现未给出，无法提供确切的返回值示例。但可以假设该方法返回了页面的某种状态或者截图，那么scoll函数的返回值可能是一个包含页面状态信息的对象或者页面截图的数据。
## AsyncFunctionDef typing
**typing函数**: 该函数的功能是在网页中模拟键盘输入文本并按下指定的按键组合。

该`typing`函数是一个异步函数，用于在网页中模拟键盘输入文本，并且可以模拟按下组合键。在输入文本或者按下组合键之后，函数会返回当前页面的截图，包括点击元素的边界框（bbox）和索引。

函数的参数如下：
- `elem_xpath`（可选）: 如果提供，函数会先点击指定的元素，然后再进行键盘输入。这个参数应该是元素的XPath路径。
- `text`（可选）: 要输入的文本。
- `press`（可选）: 要按下的键，可以是任何键的组合。如果没有提供，默认不会按下任何键。例如：'Enter', 'Alt+ArrowLeft', 'Shift+KeyW'。

函数的使用示例包括：
- 输入'Hello World'并按下'Enter'键：`typing(text='Hello World', press='Enter')`
- 回退到上一个页面：`typing(press='Alt+ArrowLeft')`

在函数的实现中，首先会调用`_check_browser`方法来确保浏览器处于可用状态。如果`elem_xpath`参数被提供，会先调用`_click_by_xpath`方法来点击指定的元素。然后，使用`page.keyboard.type`方法输入文本，如果`press`参数被提供，会使用`page.keyboard.press`方法来模拟按键操作。最后，调用`_look_page`方法来获取当前页面的截图并返回。

**注意**：
- 由于`typing`函数是异步的，调用时需要使用`await`关键字。
- 在使用XPath选择元素时，需要确保XPath的正确性和唯一性，以便正确地定位到页面上的元素。
- 如果`text`和`press`参数都未提供，函数将不会执行任何键盘输入操作。

**输出示例**：
函数可能返回的截图对象，包含页面的截图以及点击元素的边界框和索引信息。这个返回值的具体格式取决于`_look_page`方法的实现细节，通常会是一个包含截图和其他相关信息的对象或字典。
***
