# DocumentParser_using_PredefinedPatterns(Nanonets)

This notebook provides a Python-based implementation of a document parser using the Nanonets OCR API. The script extracts and structures data from PDF or image files into JSON format, utilizing predefined dynamic patterns for key fields.

## Table of Contents

- [Installation](#installation)
- [Setup](#setup)
- [Usage](#usage)
  - [Converting PDF/Image to String](#converting-pdfimage-to-string)
  - [Formatting Options](#formatting-options)
  - [Extracting Data](#extracting-data)
  - [Building JSON Structure](#building-json-structure)
- [Input Formats](#input-formats)
- [Limitations](#limitations)
- [Conclusion](#conclusion)
## Installation

To use this script, you'll need to install the following dependencies:

```python
pip install ocr-nanonets-wrapper
```
The script relies on the ocr-nanonets-wrapper package, which interfaces with the Nanonets OCR API.

## Setup
Clone this repository or download the DocumentParser_using_PredefinedPatterns(Nanonets).ipynb file.

Obtain a free API key from Nanonets by creating an account here.

Set up your API key in the script by replacing 'd6fdc799-5806-11ef-909f-1676875ee840' with your own API key:

```python
model.set_token('your-api-key-here')
```
## Usage
### Converting PDF/Image to String
You can convert PDF or image files to text strings using the convert_to_string method:

```python
string1 = model.convert_to_string('/path/to/your/file.tiff')
print(string1)
```
### Formatting Options
The convert_to_string method supports different formatting options:

Default: Plain text output.
Lines: Retains line breaks.
None: Removes any formatting.

```python
string2 = model.convert_to_string('/path/to/your/file.tiff', formatting='lines')
print(string2)
```
### Extracting Data
The script uses regular expressions to dynamically extract key information from the text. The patterns for extraction are defined in the patterns dictionary:

```python
patterns = {
    'filing_id': r'Filing\s+ID\s*[:\-]?\s*(\d+)',
    # Add other patterns here...
}
```
You can extract data using the extract_data function:

```python
extracted_data = extract_data(string2, patterns)
```

### Building JSON Structure
After extracting the data, the script structures it into a JSON format using the build_json_structure function:

```python
json_data = build_json_structure(extracted_data)
```

Finally, you can convert the structured data into a JSON string:

```python
json_output = json.dumps(json_data, indent=4)
print(json_output)
```

## Input Formats
Supported Formats: The script supports TIFF, PNG, JPEG, and PDF formats.
OCR Requirements: The input file must contain recognizable text for OCR to extract information.
## Limitations
Pattern Sensitivity: The predefined patterns are designed for specific document layouts. They may not work effectively for different layouts or documents with varying structures.
OCR Accuracy: The quality of the OCR output depends on the clarity of the input image. Blurry or low-resolution images may result in inaccurate text extraction.
API Limitations: The Nanonets API may have rate limits or other usage restrictions depending on your subscription plan.
## Conclusion
This project provides a flexible solution for parsing documents and converting them into structured JSON formats using the Nanonets OCR API. While it is designed for specific document types, the dynamic pattern matching can be adapted to suit different needs.
