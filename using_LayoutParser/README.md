# DocumentParser_using_LayoutParser

This project provides a Python-based implementation of a document parser using the LayoutParser library. The script leverages deep learning models to detect and extract structured data from PDF or image files, converting them into JSON format.

## Table of Contents

- [Installation](#installation)
- [Setup](#setup)
- [Usage](#usage)
  - [Layout Detection](#layout-detection)
  - [Drawing Boxes](#drawing-boxes)
  - [Sorting the Text](#sorting-the-text)
  - [Performing OCR](#performing-ocr)
  - [Printing the Text](#printing-the-text)
  - [Converting Text Blocks to JSON](#converting-text-blocks-to-json)
- [Limitations](#limitations)
- [Conclusion](#conclusion)
## Installation

To use this script, you'll need to install the following dependencies:

```python
!apt-get install -qq poppler-utils  # pdf2image dependency
!apt-get install -qq tesseract-ocr  # Tesseract OCR Engine
!pip install layoutparser torchvision pdf2image
!pip install "detectron2@git+https://github.com/facebookresearch/detectron2.git@v0.5#egg=detectron2"
!pip install "layoutparser[ocr]"
```
## Setup
Clone this repository or download the DocumentParser_using_LayoutParser.ipynb file.

Adjust the file paths and model configurations as needed.

## Usage
### Layout Detection
The script uses multiple models from LayoutParser to detect various layout elements like text, titles, lists, tables, and figures in the input PDF or image file:
```python
pdf_file = '/content/tiff2pdf.pdf'  # Adjust the filepath of your input image accordingly
img = np.asarray(pdf2image.convert_from_path(pdf_file)[0])

model1 = lp.Detectron2LayoutModel('lp://PubLayNet/mask_rcnn_X_101_32x8d_FPN_3x/config',
                                  extra_config=["MODEL.ROI_HEADS.SCORE_THRESH_TEST", 0.5],
                                  label_map={0: "Text", 1: "Title", 2: "List", 3: "Table", 4: "Figure"})

layout_result1 = model1.detect(img)
```
### Drawing Boxes
After detecting layout elements, the script draws bounding boxes around detected blocks:
```python
lp.draw_box(img, layout_result1, box_width=5, box_alpha=0.2, show_element_type=True)
```
### Sorting the Text
The script sorts text blocks based on their positions on the page:
```python
def sort_blocks(text_blocks, img):
    image_width = len(img[0])

    left_interval = lp.Interval(0, image_width / 2, axis='x').put_on_canvas(img)
    left_blocks = text_blocks.filter_by(left_interval, center=True)._blocks
    left_blocks.sort(key=lambda b: b.coordinates[1])

    right_blocks = [b for b in text_blocks if b not in left_blocks]
    right_blocks.sort(key=lambda b: b.coordinates[1])

    sorted_blocks = lp.Layout([b.set(id=idx) for idx, b in enumerate(left_blocks + right_blocks)])
    return sorted_blocks
```
### Performing OCR
The script uses Tesseract OCR to extract text from the detected and sorted blocks:
```python
ocr_agent = lp.TesseractAgent(languages='eng')

def perform_ocr(text_blocks, img, ocr_agent):
    for block in text_blocks:
        segment_image = (block
                         .pad(left=15, right=15, top=5, bottom=5)
                         .crop_image(img))

        text = ocr_agent.detect(segment_image)
        block.set(text=text, inplace=True)
```
### Printing the Text
The extracted text can be printed for review:
```python
def print_text_blocks(text_blocks):
    for txt in text_blocks:
        print("Text =", txt.text)
        print("x_1 =", txt.block, end='\n---\n')
```
### Converting Text Blocks to JSON
Finally, the script converts the detected text blocks into JSON format:
```python
import json

def text_blocks_to_json(text_blocks):
    json_data = []
    for block in text_blocks:
        block_data = {
            "text": block.text,
            "coordinates": {
                "x1": block.coordinates[0],
                "y1": block.coordinates[1],
                "x2": block.coordinates[2],
                "y2": block.coordinates[3]
            }
        }
        json_data.append(block_data)
    return json_data

# Convert and save JSON
json_output = text_blocks_to_json(text_blocks1)
with open('output.json', 'w') as json_file:
    json.dump(json_output, json_file, indent=4)
```
## Limitations
Model Sensitivity: The predefined models are trained on specific datasets and may not generalize well to all document types.
OCR Accuracy: The quality of the OCR output depends on the clarity and resolution of the input image.
Resource Intensive: Running deep learning models for layout detection may require a powerful machine, especially for large documents.
## Conclusion
This project provides a robust framework for parsing and extracting structured data from documents using LayoutParser. 
It can be adapted to various document layouts by adjusting model configurations and extraction patterns.
