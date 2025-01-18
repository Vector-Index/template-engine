### AI模板引擎用户手册（基于Mako）

#### 项目路径：`test`

#### 源代码树结构：

```
test
├── data.json
├── input_form.csv
├── output.txt
├── prompt1.txt
├── template.txt
└── test.txt
```

#### 文件说明：

1. **`data.json`**  
   存储了项目所需的基础数据，格式为JSON文件。该文件中的数据会在模板中被动态引用。

2. **`input_form.csv`**  
   一个CSV文件，用于填写与项目相关的内容，例如项目名称、功能描述等。

3. **`output.txt`**  
   用于展示模板渲染的输出结果。在示例中显示了最终的商业计划书内容，但该文件在实际使用时并非必需。

4. **`prompt1.txt`**  
   一个示例文件，可以在模板中引用，用于生成模板时的提示内容。它本身不是必需的，但可以作为模板的一部分进行动态加载。

5. **`template.txt`**  
   这是模板文件，包含了如何渲染最终输出的逻辑。模板中包含了Mako语法和其他自定义代码。

6. **`test.txt`**  
   该文件用于存储测试数据，内容为纯文本。在渲染过程中会被读取并插入到模板中。

---

#### `data.json` 示例：

```json
{
    "title": "无人机智能监控项目商业计划书",
    "name": "John Doe",
    "age": 30,
    "city": "New York",
    "question": "What is the capital of France?",
    "input": "What is the capital of France?"
}
```

#### `input_form.csv` 示例：

```csv
类别,用户填写内容,填写说明
项目名称,小型无人机自动巡查监管系统,与产品或项目功能相关联的名称，不是系列或型号名
项目基本功能,采用小型旋翼无人机及机载摄像机，匹配GPS电子地图导航，对独立房屋及周边设施空中自动巡查，实现安全监视管理。项目为软硬件整合产品。硬件为商用小型旋翼无人机及机载摄像机，软件为GPS电子地图导航系统及远端飞航、自动巡查、自动起降等控制系统。,组成产品或项目的软硬件基本技术、基本原理、基本构成描述。
针对痛点,解决地面设施监管不足问题，可取代固定摄像头，避免固定监视系统被破坏、遮蔽等情况，防范房屋及周边设施开工典礼遭破坏、被入侵或被偷盗现象发生，提高房屋及周边设施安全性。,产品或项目可以解决的主要问题
潜在用户,美国城乡区域独立房屋及周边设施所有者。,用户所处的地理分布、年龄、层级，例如：国内/国外，城市/乡村，青少年/中老年，精英/工薪阶层；
...
```

#### `output.txt` 示例：

```txt
无人机智能监控项目商业计划书

Hello John Doe, 

You are 30 years old and live in New York.

The capital of France is Paris.

Paris, the capital city of France, is renowned for its rich history, art, culture, and architecture. Here are some key aspects of the city:
...
```

#### `prompt1.txt` 示例：

```txt
请按照下列商业计划书框架，根据项目基础资料，编写项目商业计划书相关内容。具体要求如下：
1、撰写“愿景和目标分析”内容。这部分内容分成多个段落，采取总体概括、分别详述的写作风格。内容必须简明清晰，准确合理，段落间具有很强的关联性、逻辑性，不要解释说明和结论总结。各段落把握重点，不能重复。论述内容深入、细致，利用常见的原理、模型、效应、事例加强论点和表现内涵。
...
```

#### `template.txt` 示例：

```txt
<%!
    # 导入函数到模板中
    from langchain_cli.utils import generate_response
    from langchain_cli.utils import read_file
    from langchain_cli.utils import json_dumps
%>
${title}

Hello ${name}, 

You are ${age} years old and live in ${city}.

<% answer = generate_response(question) %>
${answer}

${generate_response(prompt=f"Tell me more about {answer[-6:-1]}")}

${read_file("test.txt")}

${read_file("prompt1.txt") + "\n" + read_file("input_form.csv")}

${read_file("input_form.csv")}

${json_dumps(input_form)}

${input_form.get("项目基本功能")}
```

#### `test.txt` 示例：

```txt
hello, world
```

---

### 使用说明：

1. **数据文件格式要求：**
   - `data.json` 中应包含项目相关的基础信息，模板会通过引用这些字段来生成输出内容。
   - `input_form.csv` 用于存储项目的详细信息，可以通过表格的形式填写。

2. **模板渲染：**
   - `template.txt` 是核心模板文件，其中包含Mako语法，通过动态加载数据文件和生成相应的内容来进行最终的输出。
   - 模板文件可以通过 `${}` 来引用JSON文件中的数据，或者调用自定义的函数（如 `generate_response`）。
   
3. **如何使用：**
   - 将所有文件放置在相同的文件夹路径下，确保 `template.txt` 文件中引用的数据路径正确。
   - 执行模板渲染后，输出将根据 `data.json` 中的数据动态生成。您可以通过阅读 `output.txt` 来查看生成的内容。

4. **模板语法：**
   - 使用 `Mako` 模板引擎，可以在模板文件中进行逻辑操作、引用数据和函数。
   - 支持使用自定义函数，如 `generate_response` 来处理问题并返回相应的答案。

---

### 提示：

- 确保所有的文件路径都是正确的，模板中引用的文件（如 `test.txt`、`prompt1.txt`、`input_form.csv`）都在模板目录中。
- 如果需要扩展模板功能，可以通过修改 `template.txt` 和引入新的Python函数来实现。

---

以上是AI模板引擎的用户手册，旨在帮助您了解如何使用该系统并高效生成所需的文档。