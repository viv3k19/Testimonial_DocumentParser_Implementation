# Testimonial_DocumentParser_Implementation
The repository contains open-source use cases for image and document parsing, including example notebooks and various techniques for parsing data into JSON, CSV, text, Markdown, and more.


## Research & Experiment List

This repository contains various Jupyter notebooks demonstrating different approaches to document parsing. Each notebook showcases a unique method or library, highlighting its strengths and potential use cases. These notebooks are part of my research experiments conducted during the implementation phase.

I have implemented a document parser, beginning with extensive research into various open-source tools and libraries. My exploration led me to discover resources such as OmniParse, OpenParse, LlamaParse, Nanonets, DocTR, Tesseract, Tesseract OCR, EasyOCR, OCRmyPDF, PyTesseract, Keras-OCR, PyPDF, PyMuPDF, PDFMiner, and Detectron2. Additionally, I experimented with Llama API and Anthropic API, ultimately consolidating all findings into a single repository. This repository includes notebooks and a README file to provide clear guidance on implementation.

Despite the thorough research conducted, I have encountered challenges in achieving effective parsing from noisy images and PDFs. I remain committed to this endeavor, and the latest 2024-synced repository is accessible for review.

1. **Document Parsing using PyTesseract**  
   This notebook provides a Python script to parse and extract structured data from an image using the pytesseract library. The script uses Optical Character Recognition (OCR) to read text from images and applies regular expressions to identify and extract specific information, converting it into a structured JSON format.

2. **Document Parsing using OmniParse**  
   This project provides a Python-based implementation of a document parser using the OmniParse library. The script is designed to run in a Colab environment and automates the setup process for working with OmniParse, enabling users to extract structured data from documents.

3. **Document Parsing using LlamaParse**  
   This notebook demonstrates how to use LlamaParse, a document parsing library, to extract text and images from PDF files. It leverages Anthropic APIs for embedding and multimodal processing.

4. **Document Parsing using docTR**  
   This project provides a Python-based implementation of a document parser using the docTR library. The script is designed to run in a Colab environment and facilitates Optical Character Recognition (OCR) by leveraging docTR's capabilities with either TensorFlow or PyTorch backends.

5. **Document Parsing using LayoutParser**  
   This notebook provides a Python-based implementation of a document parser using the LayoutParser library. The script leverages deep learning models to detect and extract structured data from PDF or image files, converting them into JSON format.

6. **Document Parsing using PredefinedPatterns**  
   This notebook provides a Python-based implementation of a document parser using the Nanonets OCR API. The script extracts and structures data from PDF or image files into JSON format, utilizing predefined dynamic patterns for key fields.

7. **Document Parsing using OpenParse**  
   This notebook demonstrates how to use OpenParse, a document parsing library, to extract and process various elements from PDF files. The notebook covers basic document parsing, table data extraction, metadata extraction, and advanced parsing with custom processing steps.



## Edgecases 
- Varied Layouts
- Noisy Backgrounds
- Mixed Font Styles and Sizes
- Low-Quality Scans
- Handwritten Text
- Complex Tables
- Language and Character Set Variability
- Embedded Graphics and Charts
- Incorrect Document Orientation
- Inconsistent Terminology
- Variable Page Sizes
- Missing Data
- Dynamic Content
- Noise in Input Data
- Overlapping Text

## Testcases
- Layout - blockwise extraction
- Metadata extraction
- RegEx based extraction
- OCR based extraction
- API based extraction
- Pattern based extraction
- NER based extraction
- LLM based extraction
- Predefined python based extraction
