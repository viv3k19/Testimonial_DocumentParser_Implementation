# DocumentParser_using_PyTesseract

This notebook provides a Python-based implementation of a document parser using the PyTesseract OCR library. The script extracts and structures data from PDF or image files into JSON format, utilizing various image processing techniques and Natural Language Processing (NLP) for enhanced accuracy.

## Table of Contents

- [Installation](#installation)
- [Setup](#setup)
- [Usage](#usage)
  - [Document Parsing](#document-parsing)
  - [Named Entity Recognition (NER)](#named-entity-recognition-ner)
  - [Transformers Integration](#transformers-integration)
  - [Image Processing](#image-processing)
- [Input Formats](#input-formats)
- [Limitations](#limitations)
- [Conclusion](#conclusion)

## Installation

To use this script, you'll need to install the following dependencies:

```python
pip install pytesseract opencv-python spacy transformers
sudo apt install tesseract-ocr
python -m spacy download en_core_web_sm
```
## Usage
### Document Parsing
You can parse an image document using the parse_document function. This function opens an image, extracts text using PyTesseract, and utilizes regular expressions to extract key information.
```python
image_path = '/content/your_image_file.tiff'
parsed_data = parse_document(image_path)
print(json.dumps(parsed_data, indent=4))
```
### Named Entity Recognition (NER)
The script incorporates SpaCy for Named Entity Recognition (NER) to enhance the extraction of relevant entities from the text.

You can extract entities using the extract_entities function, which processes the text with SpaCy:
```python
ner_data = extract_entities(text)
```
### Transformers Integration
You can also utilize transformers for advanced NER capabilities. The script initializes a text extraction pipeline using Hugging Face transformers:
```python
ner_pipeline = pipeline('ner')
```
### Image Processing
The image processing section employs OpenCV to preprocess images for better OCR performance. This includes resizing, thresholding, and morphological transformations.

```python
def preprocess_image(image_path):
    # Load image
    img = cv2.imread(image_path)

    # Convert to grayscale
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

    # Apply thresholding
    _, thresh = cv2.threshold(gray, 150, 255, cv2.THRESH_BINARY_INV)

    # Remove noise
    denoised = cv2.fastNlMeansDenoising(thresh, h=30)

    return denoised

#Define text extraction function
def extract_text(image):
    # Perform OCR using Tesseract
    text = pytesseract.image_to_string(image, lang='eng')
    return text

#Define entity extraction function
def extract_entities(text):
    # Use spaCy or transformer model for NER
    doc = nlp(text)
    entities = [(ent.text, ent.label_) for ent in doc.ents]

    # Further classify using transformers
    results = ner_pipeline(text)

    return entities, results

#Define function to structure data into JSON
def structure_data(entities):
    # Structure entities into a dynamic JSON format
    structured_data = {}

    for entity, label in entities:
        structured_data[label] = entity

    return structured_data

#Define main processing function
def process_images(file_list):
    final_results = []

    for file in file_list:
        preprocessed_image = preprocess_image(file)
        extracted_text = extract_text(preprocessed_image)
        entities, results = extract_entities(extracted_text)
        structured_output = structure_data(entities)

        final_results.append(structured_output)

    return final_results
```
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
## Input Formats
- Supported Formats: The script supports TIFF, PNG, JPEG, and PDF formats.

- OCR Requirements: The input file must contain recognizable text for OCR to extract information.

## Limitations
Pattern Sensitivity: The regular expressions used for data extraction are tailored for specific document layouts and may not generalize well across different layouts or documents.
OCR Accuracy: The quality of the OCR output is highly dependent on the clarity of the input image. Blurry or low-resolution images may yield inaccurate text extraction.
Library Limitations: Ensure that your environment meets the library requirements and configurations.
## Conclusion
This project provides a flexible solution for parsing documents and converting them into structured JSON formats using PyTesseract, SpaCy, and transformers. While the implementation is designed for specific document types, it can be adapted to suit different needs by modifying the extraction patterns and processing techniques.
