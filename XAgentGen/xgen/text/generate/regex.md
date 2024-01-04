# FunctionDef to_hash
**to_hash函数**: 该函数的功能是生成一个字符串的哈希值。

该函数接收三个参数：`vocabulary`（词汇表），`regex_str`（正则表达式字符串），和`eos_token`（结束符标记）。它首先构造一个特定格式的字符串，该字符串包含词汇表中的所有词汇、正则表达式字符串以及结束符标记。然后，它返回这个字符串的哈希值。

在项目中，`to_hash`函数被用于创建一个唯一的键（`hash_key`），这个键用于缓存正则表达式有限状态机（FSM）和与之相关的映射。这样做的目的是为了避免在每次实例化`regex.py`中的类时重复计算相同的正则表达式FSM和映射。

具体来说，在`regex.py`文件中的类的构造函数中，如果`states_to_token_maps`（状态到令牌映射）、`empty_token_ids`（空令牌ID集）、`initial_state`（初始状态）或`final_states`（最终状态集）中的任何一个为`None`，则会调用`to_hash`函数来生成一个哈希键。这个键用于检查是否已经有缓存的FSM和映射。如果缓存中存在这个键，则直接使用缓存的值，否则，将会根据提供的正则表达式和模型的词汇表来创建新的FSM和映射，并将其存储在缓存中。

**注意**：
- 哈希值的生成依赖于输入的字符串，因此相同的输入将会产生相同的哈希值。这意味着，如果词汇表、正则表达式或结束符标记有任何变化，都会产生不同的哈希值。
- 在Python中，不同的运行环境可能会导致不同的哈希值，因为Python的哈希算法可能因版本或平台而异。如果需要跨环境一致的哈希值，可能需要使用如`hashlib`库提供的算法。

**输出示例**：
假设有以下输入：
- `vocabulary` = ['apple', 'banana', 'cherry']
- `regex_str` = 'a.*'
- `eos_token` = '
***
# ClassDef Regex
**Regex类的功能**：Regex类表示基于正则表达式的生成模型。Regex实例是受限的生成模型，只生成与给定正则表达式匹配的序列。

```python
import outlines.text as text
generator = text.generate.regex(model, "(0|[1-9][0-9]+)")
```

可以使用以下方式从提示中生成序列：

```python
sequence_1 = generator("Return an integer between 0 and 10")
sequence_2 = generator("Rate the movie "Hackers" on a scale from 0 to 10")
```

**注意**：尽可能重用这些引导生成器的实例（例如上面的`generator`），因为构建它们的开销比从它们生成令牌序列的开销更大。

**构造函数参数**：
- `model`：用于生成下一个令牌概率的模型实例。
- `regex_string`：用于引导/约束令牌采样过程的正则表达式。
- `max_tokens`：要采样的最大令牌数。
- `sampler`：用于绘制样本的函数。默认为`outlines.text.generate.sample.multinomial`。有关此类函数的预期形式，请参见`outlines.text.generate.sample.Sampler`。
- `stop`：可选的停止字符串。
- `allow_empty_tokens`：是否允许采样空字符串对应的令牌。
- `initial_state`：预先计算的有限状态机（FSM）的初始状态。
- `final_states`：预先计算的FSM的终止状态集合。
- `states_to_token_maps`：预先计算的FSM的开始状态到令牌ID和对应的FSM结束状态的映射。
- `empty_token_ids`：预先计算的令牌ID集合，用于表示空字符串。

**create_proposal方法**：修改下一个令牌的logits，以便只能生成有效的令牌。

**postprocess_completions方法**：清除最后的FSM状态，然后调用父类的postprocess_completions方法。

**注意**：在使用该代码时需要注意以下几点：
- 尽量重用Regex实例，因为构建它们的开销比从它们生成令牌序列的开销更大。
- 确保提供的正则表达式与模型的词汇表兼容，否则会引发ValueError异常。

**输出示例**：
```python
generator = Regex(model, "(0|[1-9][0-9]+)")
sequence = generator("Return an integer between 0 and 10")
print(sequence)
# Output: "5"
```
## FunctionDef __init__
**__init__ 函数**: 该函数的功能是初始化一个基于正则表达式约束的文本生成器。

该`__init__`函数是一个构造函数，用于创建一个新的文本生成器实例，该实例能够根据给定的正则表达式来生成文本。这个生成器通常用于自然语言处理任务，例如文本自动补全或者其他需要按照特定模式生成文本的场景。

详细代码分析如下：

- `model`参数是用于生成下一个token概率的模型实例。
- `regex_string`参数是一个字符串，表示用于指导/约束token采样过程的正则表达式。
- `max_tokens`参数是一个可选的整数，表示要采样的最大token数量。
- `sampler`参数是一个可选的函数，用于抽取样本，默认使用`outlines.text.generate.sample.multinomial`。`outlines.text.generate.sample.Sampler`定义了这类函数的预期形式。
- `stop`参数是一个可选的停止字符串或字符串列表。
- `allow_empty_tokens`参数是一个布尔值，表示是否允许采样对应于空字符串的tokens。
- `initial_state`参数是一个可选的整数，表示有限状态机（FSM）的初始状态。
- `final_states`参数是一个可选的整数集合，表示FSM的最终状态。
- `states_to_token_maps`参数是一个可选的字典，预先计算的FSM起始状态到token id与其对应FSM结束状态之间的映射。
- `empty_token_ids`参数是一个可选的整数集合，预先计算的对应于空字符串tokens的token id。

在函数体内部，首先检查是否提供了`states_to_token_maps`、`empty_token_ids`、`initial_state`和`final_states`这些参数。如果没有提供，将使用模型的tokenizer来计算这些值。计算过程中使用了`to_hash`函数来生成一个缓存键，以便在缓存中查找或存储相关数据。

如果缓存中存在对应的键，则直接从缓存中获取FSM及其相关映射和id集合。否则，将使用`interegular.parse_pattern`来解析正则表达式，并通过`make_deterministic_fsm`和`create_fsm_index_tokenizer`函数来创建FSM和相关映射。

最后，函数检查是否存在从FSM的初始状态到其终止状态的路径，如果不存在，则抛出`ValueError`异常。此外，还初始化了一些用于记录状态和缓存的变量。

**注意**：
- 在使用这个构造函数时，需要确保提供的`model`具有有效的`tokenizer`属性，且该tokenizer能够提供一个词汇表。
- 正则表达式`regex_string`需要是有效的正则表达式，并且能够与模型的词汇表相匹配，否则可能会抛出异常。
- 如果提供了`states_to_token_maps`、`empty_token_ids`、`initial_state`和`final_states`参数，将不会进行额外的计算，这可以用于优化性能。
- 该函数使用了缓存来提高性能，避免重复计算相同的FSM和映射。
## FunctionDef create_proposal
**create_proposal函数**：该函数的功能是修改下一个令牌的logits，以确保只能生成有效的令牌。

create_proposal函数是一个用于控制令牌生成过程的函数，它通过修改下一个令牌的logits来确保在特定的有限状态机（FSM）约束下生成有效的令牌。这个函数主要用于基于transformer模型的文本生成过程中，以确保生成的文本符合预定义的规则或模式。

具体来说，该函数接收两个参数：`generated_token_ids`和`logits`。`generated_token_ids`是到目前为止生成的令牌ID序列，`logits`是下一个令牌的logits，即模型预测的下一个令牌的概率分布。

函数首先检查`generated_token_ids`的维度是否为2，然后根据当前的FSM状态和已生成的令牌序列来计算下一个状态和有效的下一个令牌ID。如果生成的序列中包含空令牌，FSM状态不会改变；如果序列以EOS（End of Sequence）令牌结束，则只能生成EOS令牌；如果当前状态没有后续转移，则假定已经处于FSM的最终状态，并且从此状态只能生成EOS令牌。

对于每个生成的令牌序列，函数都会计算一个掩码（mask），该掩码将在logits上应用，以便只有有效的令牌ID对应的logits保持不变，而其他的logits则会被设置为一个非常小的值（通常是负无穷大），这样在后续的采样过程中，这些令牌的生成概率将会非常低。

最后，函数将所有掩码拼接起来，并将其与原始的logits相加，返回修改后的logits。

**调用情况**：
1. 在`XAgentGen/app.py`中，`create_proposal`函数被用于处理生成的令牌ID列表和logits张量，返回掩码后的logits张量。
2. 在`XAgentGen/xgen/parser/function_parser.py`中，`create_proposal`函数被用于从给定的上下文ID中生成下一个有效令牌ID的列表。

**注意**：
- 使用此函数时，需要确保传入的`generated_token_ids`和`logits`具有正确的形状和类型。
- 函数依赖于内部的FSM状态，因此在连续调用时需要注意状态的维护。
- 函数中的断言用于确保输入的合法性和FSM的正确性，如果断言失败，可能需要检查FSM的定义或调用函数的方式。

**输出示例**：
假设模型的tokenizer有一个特殊的EOS令牌ID为2，且当前FSM状态允许生成ID为[5, 10]的令牌，那么函数可能返回如下的logits张量：
```
tensor([[ -inf,  -inf,  -inf,  -inf,  -inf, 0.1,  -inf,  -inf,  -inf,  -inf, 0.9,  -inf,  -inf, ...]])
```
在这个张量中，只有ID为5和10的位置是有效的logits值（例如0.1和0.9），其他位置都是负无穷大，表示这些令牌不应该在下一步生成。
## FunctionDef _get_mask_for_state
**_get_mask_for_state函数**: 该函数的功能是生成一个遮罩张量（mask tensor），用于在生成文本时限制下一个可能生成的token，确保生成的序列符合预定义的状态机（finite state machine, FSM）规则。

_get_mask_for_state函数接收三个参数：状态（state），大小（size），以及下一个token的ID列表（next_token_ids）。它返回一个torch.LongTensor类型的遮罩张量，该张量的长度与参数size相同。

函数首先尝试从内部的mask_cache缓存中获取遮罩张量。如果缓存中不存在对应的遮罩，则会创建一个新的遮罩张量，并将其值初始化为负无穷（-math.inf），这意味着在没有特别指定的情况下，所有的token都将被视为不可能生成的。

接下来，函数根据allow_empty_tokens标志决定是否将empty_token_ids列表中的token ID包含在内。如果允许空token，则将这些ID与next_token_ids合并；否则，只使用next_token_ids。

然后，函数将遮罩张量中对应于token_ids中的ID的位置设置为0，表示这些token是可以生成的。最后，将遮罩张量扩展一个维度（从1D变为2D），并将其存入mask_cache缓存中，以便下次可以直接使用。

在项目中调用该函数的场景中，_get_mask_for_state用于在生成文本的过程中动态调整每一步可能生成的token。它根据当前的FSM状态和已生成的token序列来确定下一步可能的token，并生成相应的遮罩张量来调整生成模型的logits，从而确保生成的文本序列符合预定义的规则。

**注意**：
- 使用该函数时，需要确保传入的state和size参数与预期的FSM状态和token序列长度相匹配。
- mask_cache缓存的使用可以提高函数的效率，避免重复计算相同状态和大小的遮罩张量。
- 函数内部使用了torch.full来创建遮罩张量，并使用了unsqueeze来扩展维度，这些操作都需要与PyTorch框架兼容。

**输出示例**：
假设函数调用如下：
```python
mask = _get_mask_for_state(1, 5, [2, 3])
```
可能的返回值为一个2D的torch.LongTensor，如下所示：
```
tensor([[  -inf,   -inf,   0.,   0.,   -inf]])
```
在这个例子中，只有ID为2和3的token被允许生成，其余位置的token被设置为负无穷，表示不可生成。
## FunctionDef postprocess_completions
**postprocess_completions函数**: 该函数的功能是对完成项列表进行后处理。

该`postprocess_completions`函数是一个成员方法，它接受一个字符串列表`completions`作为参数，并返回一个经过处理的字符串列表。函数的主要任务是清除`self.last_fsm_states`列表中的所有元素，并调用其父类的`postprocess_completions`方法来完成后处理工作。

在详细分析中，首先，函数通过`self.last_fsm_states.clear()`清空了`last_fsm_states`列表。`last_fsm_states`可能是用来存储有限状态机（Finite State Machine, FSM）的状态信息。清空这个列表可能是为了重置状态信息，以便于下一次处理时不会受到上一次处理的影响。

接着，函数通过`super().postprocess_completions(completions)`调用了父类的`postprocess_completions`方法，并将原始的`completions`列表传递给它。这表明该函数的后处理逻辑部分依赖于父类的实现，具体的后处理逻辑需要查看父类中该方法的实现细节。

**注意**：在使用这个函数时，需要注意以下几点：
1. 该函数依赖于其父类的`postprocess_completions`方法，因此在没有父类方法的实现或者父类方法实现有变动的情况下，可能会影响到该函数的行为。
2. 在调用这个函数之前，确保`completions`列表包含了需要进行后处理的完成项。
3. 由于函数清空了`last_fsm_states`列表，如果其他部分的代码依赖于这个状态列表，那么在调用这个函数后可能需要重新设置状态。

**输出示例**：由于这个函数的输出依赖于父类的`postprocess_completions`方法，没有具体的实现细节，因此无法提供一个具体的输出示例。不过，可以假设如果父类的方法不做任何修改，那么输出将会是一个与输入相同的字符串列表。
***
# FunctionDef regex
**regex 函数**: 此函数的功能是生成与输入正则表达式匹配的文本序列。

此函数`regex`接受一个语言模型、一个正则表达式字符串以及其他可选参数，用于生成与给定正则表达式匹配的文本序列。它主要用于在自然语言处理任务中，根据特定的模式生成文本。

参数详解：
- `model`：用于计算下一个词的概率分布的语言模型。这个模型是生成文本的核心，它基于给定的上下文预测下一个最可能的词。
- `regex_string`：生成的表达式必须匹配的正则表达式。这个字符串定义了文本生成的模式，生成的文本将会尝试符合这个正则表达式的规则。
- `max_tokens`：生成的最大词数。这个参数限制了生成文本的长度，以词为单位。
- `sampler`：用于抽样的函数，默认为`outlines.text.generate.sample.multinomial`。这个参数允许用户指定一个自定义的抽样函数，用于从模型的预测中选择下一个词。
- `allow_empty_tokens`：是否允许生成对应于空字符串的词。这个参数控制生成过程中是否可以包含空的词，这可能会影响生成文本的连贯性和可读性。

**注意**：
- 使用这些引导生成器时，建议尽可能重用实例，因为构建它们的开销比从它们生成词序列的开销要大。有关`Regex`的详细信息，请参阅其文档字符串。
- 此函数返回的是一个`Regex`实例，而不是直接生成的文本。要获取生成的文本，需要调用此实例的相应方法。

**输出示例**：
假设我们有一个语言模型`model`和一个正则表达式字符串`regex_string`，调用`regex`函数可能返回一个`Regex`实例。然后，可以使用这个实例来生成文本序列，例如：

```python
# 假设的语言模型实例
model = SomeLanguageModel()

# 调用 regex 函数
regex_generator = regex(model, r"\d{3}-\d{2}-\d{4}", max_tokens=10)

# 使用 regex_generator 生成文本
generated_text = regex_generator.generate()  # 假设的生成方法
print(generated_text)  # 输出可能是符合正则表达式的文本，例如 "123-45-6789"
```

在这个示例中，`generated_text`将是一个符合正则表达式`\d{3}-\d{2}-\d{4}`的文本序列，例如一个格式正确的社会安全号码。
***
# FunctionDef integer
**integer函数**: 该函数的功能是生成整数。

integer函数用于生成符合特定正则表达式规则的整数字符串。它使用一个语言模型来计算下一个可能的token，并可以通过正则表达式来限制生成的内容，确保输出的是整数形式的字符串。该函数支持生成带有或不带有正负号的整数，并且避免生成前导零的整数字符串。

详细代码分析如下：

- `model` 参数是用于计算下一个token概率的语言模型。这个模型是生成过程中必须的，因为它决定了每一步生成的内容。
- `max_tokens` 参数指定了生成的最大token数量。这个参数可以控制生成整数的最大长度，如果不设置，则默认没有限制。
- `sampler` 参数是一个用于抽样的函数，它决定了如何从模型的概率分布中选择下一个token。如果没有提供，将使用默认的多项式抽样方法。
- `allow_empty_tokens` 参数允许在生成过程中包含空字符串对应的token。这意味着生成的整数可能会包含空格或其他不可见字符。

函数内部调用了`Regex`类，传入了正则表达式`r"[-+]?\d+"`，这个正则表达式匹配可选的正负号后跟一个或多个数字，这样可以确保生成的是整数形式的字符串。

**注意**：
- 使用这个函数时，应当重用已经创建的实例，因为构造这些生成器的开销比从它们生成token序列的开销要大。参考`Regex`类的文档字符串了解更多信息。
- 正则表达式中的`\d+`确保了至少生成一个数字，而`[-+]?`允许生成的整数可选地带有一个正号或负号。
- 如果`max_tokens`设置得太小，可能无法生成有效的整数。

**输出示例**：
```python
# 假设model是一个已经初始化好的语言模型实例，sampler是一个抽样函数
generated_integer = integer(model, max_tokens=5)
print(generated_integer)
# 输出可能是：'12345', '-678', '+90' 等符合整数格式的字符串
```
***
# FunctionDef float
**float函数**: 该函数的功能是生成浮点数。

float函数是一个用于生成符合特定正则表达式模式的浮点数的函数。它利用一个语言模型来计算下一个可能的token（即字符或字符组合），并根据提供的正则表达式约束生成的序列。

详细代码分析如下：

- `model` 参数是用于计算下一个token logits的语言模型。这个模型是生成过程中的核心，负责预测接下来最可能出现的token。
- `max_tokens` 参数指定生成的最大token数量。这个参数限制了生成序列的长度，以防生成过长的序列。
- `sampler` 参数是一个用于抽样的函数，默认值是`outlines.text.generate.sample.multinomial`。这个参数决定了如何从模型预测的概率分布中选择token，是生成过程中的一个关键环节。
- `allow_empty_tokens` 参数允许生成对应于空字符串的token。这意味着在生成过程中，可以选择不生成任何字符作为一个合法的选择。

函数内部调用了`Regex`类，该类使用提供的正则表达式来生成符合条件的字符串。在这个函数中，使用的正则表达式是`r"([+-]?((0|[1-9]+)([.][0-9]*)?)|([.][0-9]+))"`，这个表达式匹配的是标准的浮点数格式，包括可选的正负号、整数部分不允许前导零（除非整数部分就是零）、可选的小数点和小数部分。

**注意**：
- 使用这个函数时，应该尽可能重用已有的`Regex`实例，因为构造这些实例比从它们生成token序列有更多的开销。
- 生成的浮点数字符串格式遵循正则表达式的规则，不会出现Python中允许的前导零。

**输出示例**：
假设模型和采样器都已经正确设置，调用`float`函数可能会返回如下的`Regex`实例：
```python
Regex(model, "([+-]?((0|[1-9]+)([.][0-9]*)?)|([.][0-9]+))", max_tokens, sampler, allow_empty_tokens)
```
这个实例可以用来生成符合浮点数格式的字符串，例如`"3.14"`、`"-0.001"`或`"+10.0"`。
***
# FunctionDef choice
**choice 函数**: 该函数的功能是在不同的序列中进行选择。

`choice` 函数用于从提供的字符串列表中选择一个序列，并构建一个正则表达式，然后使用这个正则表达式来引导语言模型生成文本。这个函数是在文本生成过程中，用来指导模型沿着特定的路径生成文本的工具。

详细代码分析与描述：
- `model` 参数是用来计算下一个词的概率分布的语言模型。
- `choices` 参数是一个字符串列表，函数会从中选择序列。
- `max_tokens` 参数指定了生成的最大令牌数。
- `sampler` 参数是一个用于抽样的函数，默认情况下使用 `outlines.text.generate.sample.multinomial`。`Sampler` 应该符合特定的形式，以便用于生成过程。
- `allow_empty_tokens` 参数允许在生成过程中包含对应于空字符串的令牌。

函数内部首先构建了一个正则表达式 `regex_str`，该正则表达式是通过将 `choices` 列表中的字符串用管道符号 `|` 连接起来，并用括号包围起来形成的。然后，函数返回一个 `Regex` 对象，该对象使用构建的正则表达式、语言模型和其他参数来生成文本。

在项目中的调用情况：
- 在 `XAgentGen/app.py` 文件中，`choice` 函数被用来创建一个文本生成器，该生成器根据 `regex_list`（由函数解析器生成的正则表达式列表）来引导模型生成文本。
- 在 `XAgentGen/xgen/parser/function_parser.py` 文件中，`choice` 函数同样被用于创建一个文本生成器，它接收一个模型、函数信息和生成参数，然后根据这些信息生成文本。

**注意**：在使用 `choice` 函数时，应当重用这些引导生成器的实例，因为构建它们比从它们生成令牌序列有更多的开销。另外，需要确保提供的 `choices` 列表中的字符串是有效的，并且能够正确地构建成正则表达式。

**输出示例**：
假设 `choices` 列表包含字符串 ["hello", "world"]，`max_tokens` 设置为 5，那么 `choice` 函数可能返回的 `Regex` 对象将能够生成类似 "hello" 或 "world" 这样的文本序列。
***
# FunctionDef json
**json函数**: 该函数的功能是根据JSON模式或Pydantic模型生成遵循特定模式的文本序列。

该函数接受一个语言模型、一个JSON模式或Pydantic模型，以及其他一些可选参数，用于生成一个符合指定模式的文本序列。这个过程通常用于自动生成数据或者进行数据验证。

详细代码分析和描述如下：

- `model` 参数是用于计算下一个token对数概率的语言模型。这个模型是生成文本序列的核心，它决定了文本生成的质量和风格。
- `schema` 参数可以是一个JSON模式的字符串，或者是一个Pydantic模型。这个模式或模型定义了生成的文本序列应该遵循的结构和规则。
- `max_tokens` 参数指定了要生成的最大token数量。这个参数限制了输出文本的长度。
- `sampler` 参数是一个用于抽样的函数，默认情况下使用`outlines.text.generate.sample.multinomial`。这个函数决定了如何从模型的概率分布中选择下一个token。
- `allow_empty_tokens` 参数允许生成对应于空字符串的token。这可以用于生成可选字段或允许字段为空的情况。

函数首先检查`schema`参数是否为Pydantic模型的类型，如果是，则尝试获取其JSON模式表示。如果模型有`model_json_schema`属性，则使用该属性的值；否则，使用Pydantic模型的`schema()`方法获取模式。

接下来，函数调用`build_regex_from_schema`函数，将JSON模式转换为正则表达式字符串。

最后，函数返回一个`Regex`实例，该实例使用提供的参数来生成符合正则表达式的文本序列。

**注意**：在使用该函数时，应当尽可能重用这些指导生成器的实例，因为构建它们比从它们生成token序列有更多的开销。请参考`Regex`的文档字符串了解更多信息。

**输出示例**：以下是一个可能的函数返回值的模拟示例：

```python
# 假设已经有一个语言模型和一个JSON模式
generated_text = json(model, '{"type": "object", "properties": {"name": {"type": "string"}, "age": {"type": "integer"}}}')
# generated_text 是一个Regex实例，可以用来生成符合上述JSON模式的文本序列
```

在这个示例中，`generated_text`将是一个Regex对象，它被配置为生成一个包含"name"（字符串类型）和"age"（整数类型）属性的对象的JSON表示。
***
