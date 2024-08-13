# Document Parser Using LlamaParse

This notebook demonstrates how to use **LlamaParse**, a document parsing library, to extract text and images from PDF files. It leverages Anthropic APIs for embedding and multimodal processing.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Environment Variables](#environment-variables)
- [Functions](#functions)
- [Limitations](#limitations)
- [Conclusion](#conclusion)

## Installation

To set up the environment, you can use the following pip commands. Uncomment these lines in your Jupyter Notebook to install the required packages:

```python
# %pip install llama-index
# %pip install llama-index-core
# %pip install llama-index-llms-anthropic llama-index-multi-modal-llms-anthropic
# %pip install llama-index-embeddings-huggingface
# %pip install llama-parse
```
Make sure to run the following code to handle async operations:
```python
import nest_asyncio
nest_asyncio.apply()
```
## Usage
Set up API keys for LlamaCloud and Anthropic:
```python
import os

os.environ["LLAMA_CLOUD_API_KEY"] = "your_llama_cloud_api_key"
os.environ["ANTHROPIC_API_KEY"] = "your_anthropic_api_key"
```
Initialize LlamaParse:
```python
from llama_index.llms.anthropic import Anthropic

llm = Anthropic(model="claude-3-opus-20240229", temperature=0.0)

from llama_index.core import Settings

Settings.llm = llm
Settings.embed_model = "local:BAAI/bge-small-en-v1.5"

from llama_parse import LlamaParse

parser = LlamaParse(verbose=True)
```
Parse a PDF document:
```python
json_objs = parser.get_json_result("/content/your.pdf")
json_list = json_objs[0]["pages"]
```
Extract text nodes from the parsed JSON:
```python
from llama_index.core.schema import TextNode
from typing import List

def get_text_nodes(json_list: List[dict]):
    text_nodes = []
    for idx, page in enumerate(json_list):
        text_node = TextNode(text=page["text"], metadata={"page": page["page"]})
        text_nodes.append(text_node)
    return text_nodes

text_nodes = get_text_nodes(json_list)
```
Extract text from images:
```python
!mkdir llama2_images

from llama_index.core.schema import ImageDocument
from llama_index.multi_modal_llms.anthropic import AnthropicMultiModal

def get_image_text_nodes(json_objs: List[dict]):
    anthropic_mm_llm = AnthropicMultiModal(max_tokens=300)
    image_dicts = parser.get_images(json_objs, download_path="llama2_images")
    image_documents = []
    img_text_nodes = []
    for image_dict in image_dicts:
        image_doc = ImageDocument(image_path=image_dict["path"])
        response = anthropic_mm_llm.complete(
            prompt="Describe the images as alt text",
            image_documents=[image_doc],
        )
        text_node = TextNode(text=str(response), metadata={"path": image_dict["path"]})
        img_text_nodes.append(text_node)
    return img_text_nodes

image_text_nodes = get_image_text_nodes(json_objs)
```
## Environment Variables
LLAMA_CLOUD_API_KEY: Your API key for LlamaCloud.
ANTHROPIC_API_KEY: Your API key for Anthropic.
## Limitations
Requires internet access for API calls.
The performance may vary based on document complexity and size.
Limited support for specific file formats or structures.
## Conclusion
This project provides a robust approach to extracting text and images from PDF documents using LlamaParse and Anthropic APIs. 
With the ability to handle both textual and visual data, it is suitable for various document processing tasks.
