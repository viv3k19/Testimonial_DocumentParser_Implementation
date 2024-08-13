# DocumentParser_using_docTR

This project provides a Python-based implementation of a document parser using the docTR library. The script is designed to run in a Colab environment and facilitates Optical Character Recognition (OCR) by leveraging docTR's capabilities with either TensorFlow or PyTorch backends.

## Table of Contents

- [Installation](#installation)
- [Setup](#setup)
- [Usage](#usage)
  - [Basic Inference](#basic-inference)
  - [Exporting Results](#exporting-results)
- [Limitations](#limitations)
- [Conclusion](#conclusion)

## Installation

To use this script, you'll need to install the following dependencies:

**docTR with PyTorch**:
```python
!pip install python-doctr[torch]
```
To use docTR with TensorFlow, uncomment the following line and comment out the PyTorch line:
```python
# !pip install python-doctr[tf]
```
Colab-related installations:
Install dependencies to handle pyproject.toml projects correctly:
```python
!sudo apt install libcairo2-dev pkg-config
!pip3 install pycairo
```
Install the latest version from GitHub:
```python
# PyTorch
!pip3 install python-doctr[torch]@git+https://github.com/mindee/doctr.git
```
For TensorFlow:
```python
# !pip install python-doctr[tf]@git+https://github.com/mindee/doctr.git
```
Install Free Fonts (for rendering results):
```python
!sudo apt-get install fonts-freefont-ttf -y
```
Important: After each installation, restart the Colab runtime for the changes to take effect.

## Setup
Choose the desired backend:

By default, this script uses the PyTorch backend. If you wish to use TensorFlow, set the environment variable accordingly:
```python
# os.environ['USE_TF'] = '1'
os.environ['USE_TORCH'] = '1'
```
Import necessary libraries:
```python
import matplotlib.pyplot as plt
from doctr.io import DocumentFile
from doctr.models import ocr_predictor
```
Read the document:
```python
doc = DocumentFile.from_pdf("/content/tiff2pdf.pdf")
print(f"Number of pages: {len(doc)}")
```
Instantiate a pretrained model:
```python
predictor = ocr_predictor(pretrained=True)
print(predictor)
```
## Usage
Basic Inference
To perform OCR on a document:

Run the predictor on the document:
```python
result = predictor(doc)
```
Display the results:
```python
!pip install mplcursors
result.show()
```
Visualize the results:
```python
synthetic_pages = result.synthesize()
plt.imshow(synthetic_pages[0]); plt.axis('off'); plt.show()
```
Exporting Results
To export the OCR results in JSON format:
```python
json_export = result.export()
print(json_export)
```
## Limitations
Backend Specific: The script defaults to using the PyTorch backend. To switch to TensorFlow, manual adjustments in the environment variables are needed.
Resource Intensive: The script may require significant computational resources, especially for processing large documents with complex layouts.
## Conclusion
This project provides a comprehensive setup for using docTR in a cloud-based environment like Colab. By following the steps outlined in this guide, 
you can successfully install dependencies, set up the document parser, and perform OCR on your documents.
