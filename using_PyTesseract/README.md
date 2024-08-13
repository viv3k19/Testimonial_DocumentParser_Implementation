# DocumentParser_using_PyTesseract

This project provides a Python script to parse and extract structured data from an image using the `pytesseract` library. The script uses Optical Character Recognition (OCR) to read text from images and applies regular expressions to identify and extract specific information, converting it into a structured JSON format.

- [Installation](#installation)
- [Usage](#usage)
  - [Import Required Libraries](#import-required-ibraries)
  - [Parse Document Function](#parse-document-function)
  - [Run the Script](#run-the-script)
- [Limitations](#limitations)
- [Conclusion](#conclusion)
- 
## Installation

To use this script, you need to install the following dependencies:

```python
pip install pytesseract
sudo apt install tesseract-ocr
```
## Usage
### Import Required Libraries:

The script imports pytesseract, PIL (for image processing), re (for regular expressions), and json (for formatting output).

### Parse Document Function:

The parse_document function takes the path of an image file as input and performs the following steps:

- Opens the image using PIL.
- Extracts text from the image using pytesseract.
- Applies regular expressions to extract specific fields like Filing ID, Document ID, Bank Name, Debtor's Name and Address, Secured Party's Name and Address, and Collateral Information.
- The extracted data is returned in a structured dictionary.
- 
### Run the Script:
Specify the path to your image file and call the parse_document function to extract and print the structured data in JSON format.
```python
# Path to the image file
image_path = 'your_image_here'

# Parse the document and print the result
parsed_data = parse_document(image_path)
print(json.dumps(parsed_data, indent=4))
```
## Limitations
OCR Accuracy: The quality of the extracted text is highly dependent on the clarity and resolution of the input image.
Regular Expression Matching: The script uses predefined regular expressions, which may not generalize well to documents with different formats or layouts.
## Conclusion
This project demonstrates how to use pytesseract in combination with regular expressions to parse and structure information from images. 
The output JSON format makes the extracted data easy to work with for further processing or analysis.
