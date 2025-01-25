# AI Template Engine User Manual

## Introduction

This user manual provides detailed instructions on how to use the AI template engine based on the Mako template system. The engine integrates various data sources, processes them with predefined templates, and generates dynamic content. Below is a breakdown of how to use the provided templates, input data, and generate the required outputs.

---

## Project Structure

Here is a typical project structure for the AI template engine:

```
test
├── data.json
├── input_form.csv
├── output.txt
├── prompt1.txt
├── template.txt
└── test.txt
```

### File Descriptions:

- **data.json**: Contains structured input data used for rendering the template.
- **input_form.csv**: Provides user input in CSV format to guide the content generation.
- **output.txt**: The generated output, an example of what the engine produces when the template is rendered (optional in the template directory).
- **prompt1.txt**: An additional prompt file used to provide context to the template rendering (not mandatory).
- **template.txt**: The core Mako template that drives the content generation.
- **test.txt**: A simple test file for validation or additional static content.

---

## Example Workflow

### 1. Input Data

The input data is stored in `data.json`, which is a JSON file with user-specific information. For example:

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

The `data.json` contains fields such as:

- `title`: The title of the document (e.g., business plan).
- `name`, `age`, `city`: User information.
- `question`: A specific query for the AI to answer (can be extended for complex queries).
- `input`: The input field for dynamic text generation.

### 2. User Input Form

The user input can be provided through `input_form.csv`. This file guides the template with necessary content for generating reports. For example:

```csv
类别,用户填写内容,填写说明
项目名称,小型无人机自动巡查监管系统,与产品或项目功能相关联的名称，不是系列或型号名
项目基本功能,采用小型旋翼无人机及机载摄像机，匹配GPS电子地图导航，对独立房屋及周边设施空中自动巡查，实现安全监视管理。项目为软硬件整合产品。,组成产品或项目的软硬件基本技术、基本原理、基本构成描述。
...
```

This CSV guides the user through various content sections for the business plan, including project functionality, targeted pain points, potential users, and more.

### 3. Template Rendering

The core functionality of the template engine is controlled by `template.txt`. This file uses Mako syntax to integrate dynamic content. Below is an example of the template:

```txt
<%!
    # Import the greet function into the template
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

#### Key Components:

- **Imports**: External functions such as `generate_response`, `read_file`, and `json_dumps` are imported into the template.
- **Dynamic Content**: Content is injected into the template via Mako expressions. For instance, `${title}` and `${name}` dynamically pull information from `data.json`.
- **AI Integration**: The function `generate_response` fetches AI-generated answers based on queries (like the capital of France).
- **File Reading**: The template reads files like `test.txt` and `prompt1.txt`, merging their contents into the generated output.
- **JSON and CSV Integration**: The template also reads `input_form.csv`, integrates it into the output, and formats data into JSON with `json_dumps`.

### 4. Output

The output generated after running the template is stored in `output.txt`. This file will show the complete generated content, including dynamic text, AI responses, and the formatted business plan.

Example output (from the given input data):

```txt
无人机智能监控项目商业计划书

Hello John Doe, 

You are 30 years old and live in New York.

The capital of France is Paris.

Paris, the capital city of France, is renowned for its rich history, art, culture, and architecture. Here are some key aspects of the city:

### History
- **Foundation**: Paris was founded in the 3rd century BC by a Celtic tribe known as the Parisii. It became a significant city during the Roman Empire.
- **Medieval Period**: The city grew in importance during the Middle Ages, becoming a center of learning and culture.
- **French Revolution**: Paris played a crucial role in the French Revolution (1789-1799), which led to significant political and social changes in France and beyond.

### Landmarks
- **Eiffel Tower**: Completed in 1889 for the Exposition Universelle, the Eiffel Tower is one of the most recognizable structures in the world.
- **Louvre Museum**: Originally a royal palace, the Louvre is now the world's largest art museum, housing thousands of works, including the Mona Lisa and the Venus de Milo.
- **Notre-Dame Cathedral**: A masterpiece of French Gothic architecture, Notre-Dame has been a central religious and cultural site in Paris since its completion in the 14th century.
...
```

### 5. Additional Files

You can modify and extend your templates using additional files like `prompt1.txt`, which can be used for additional prompts or instructions that guide the generation process. For example, it might contain a prompt like:

```txt
请按照下列商业计划书框架，根据项目基础资料，编写项目商业计划书相关内容。
```

These files help enrich the content and give more control over the generated output.

---

## Conclusion

This AI template engine based on Mako allows you to automate the generation of dynamic, structured content by integrating various data sources. By combining the power of AI with flexible template design, you can produce business reports, marketing documents, or any content type based on user input and predefined templates.