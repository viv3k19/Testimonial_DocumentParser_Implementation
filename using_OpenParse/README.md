# DocumentParser_using_OpenParse

This notebook demonstrates how to use OpenParse, a document parsing library, to extract and process various elements from PDF files. The notebook covers basic document parsing, table data extraction, metadata extraction, and advanced parsing with custom processing steps.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
  - [Basic PDF Parsing](#basic-pdf-parsing)
  - [TableData Parsing](#tabledata-parsing)
  - [Metadata Parsing](#metadata-parsing)
  - [Advanced Parsing for Combined Data](#advanced-parsing-for-combined-data)
- [Customization](#customization)
- [Limitations](#limitations)
- [Conclusion](#conclusion)

## Installation

To use this script, you'll need to install the OpenParse library with machine learning support:

```python
!pip install openparse[ml]
```
## Usage
### Basic PDF Parsing
Import the OpenParse Library:
```python
import openparse
```
Parse a Basic Document:
- Set the path to your PDF document:
  ```python
  basic_doc_path = "/content/your.pdf"
  ```
- Initialize the document parser:
  ```python
  parser = openparse.DocumentParser()
  ```
- Parse the document:
  ```python
  parsed_basic_doc = parser.parse(basic_doc_path)
  ```
- Display the parsed nodes:
  ```python
  for node in parsed_basic_doc.nodes:
    display(node)
  ```
- Visualize the document with bounding boxes:
  ```python
  pdf = openparse.Pdf(basic_doc_path)
  pdf.display_with_bboxes(parsed_basic_doc.nodes)
  ```
Export the parsed document as JSON:
```python
parsed_basic_doc.model_dump()
```
### TableData Parsing
For documents with tables, you can use a specialized parser:

Set the path to your PDF document:
```python
doc_with_tables_path = "/content/your.pdf"
```
Initialize the document parser with table parsing algorithm:
```python
parser = openparse.DocumentParser(
    table_args={"parsing_algorithm": "table-transformers"}
)
```
Parse and display the document:
```python
parsed_doc2 = parser.parse(doc_with_tables_path)

for node in parsed_doc2.nodes:
    display(node)

pdf = openparse.Pdf(doc_with_tables_path)
pdf.display_with_bboxes(parsed_doc2.nodes)
```
Export the parsed document as JSON:
```python
parsed_doc2.model_dump()
```
### Metadata Parsing
You can extract metadata from your documents:

Set the path to your PDF document:
```python
meta_path = "/content/your.pdf"
```
Initialize the document parser with a metadata parsing algorithm:
```python
parser = openparse.DocumentParser(table_args={"parsing_algorithm": "pymupdf"})
```
Parse and display the metadata:
```python
parsed = parser.parse(meta_path)
doc = openparse.Pdf(file=meta_path)
doc.display_with_bboxes(parsed.nodes)
```
### Advanced Parsing for Combined Data
For more advanced use cases, you can create custom processing steps:

Import necessary libraries:
```python
from openparse import processing, Node
from typing import List
```
Create a custom processing step:
```python
class CustomCombineTables(processing.ProcessingStep):
    """
    Let's combine tables that are next to each other
    """
    def process(self, nodes: List[Node]) -> List[Node]:
        new_nodes = []
        print("Combining concurrent tables")
        for i in range(len(nodes) - 1):
            if "table" in nodes[i].variant and "table" in nodes[i + 1].variant:
                new_node = nodes[i] + nodes[i + 1]
                new_nodes.append(new_node)
            else:
                new_nodes.append(nodes[i])
        return new_nodes
```
Create a custom pipeline:
```python
custom_pipeline = processing.BasicIngestionPipeline()
custom_pipeline.append_transform(CustomCombineTables())
```
Parse the document using the custom pipeline:
```python
parser = openparse.DocumentParser(
    table_args={"parsing_algorithm": "pymupdf"}, processing_pipeline=custom_pipeline
)
custom = parser.parse(meta_path)

doc = openparse.Pdf(file=meta_path)
doc.display_with_bboxes(custom.nodes)
doc.model_dump()
```
## Customization
You can customize the document parsing process by modifying the parsing algorithms and processing pipelines according to your specific requirements.

## Limitations
Reliance on specific PDF structures, which may lead to incomplete data extraction and misinterpretation of complex tables. Processing large documents can be slow and resource-intensive, while the custom pipeline requires a deep understanding of OpenParse, making it prone to configuration errors. Limited support for non-English text may affect OCR accuracy, and compatibility issues with external libraries can arise. Additionally, the notebook lacks robust error handling, basic visualization capabilities may not suffice for detailed inspections, and scalability for batch processing is challenging. Lastly, limited documentation and community support for OpenParse may complicate troubleshooting and customization efforts.

## Conclusion
This project provides a comprehensive approach to parsing and extracting data from PDF documents using OpenParse. 
With various parsing algorithms and the ability to create custom processing steps, OpenParse is a versatile tool for document processing tasks.
