# DocLayNet

DocLayNet is a human-annotated document layout segmentation dataset containing `80863` pages from a broad variety of document sources.


## Overview

DocLayNet provides page-by-page layout segmentation ground-truth using bounding-boxes for 11 distinct class labels on 80863 unique pages from 6 document categories. It provides several unique features compared to related work such as PubLayNet or DocBank:

1. *Humman Annotation*: DocLayNet is hand-annotated by well-trained experts, providing a gold-standard in layout segmentation through human recognition and interpretation of each page layout
2. *Large layout variability*: DocLayNet includes diverse and complex layouts from a large variety of public sources in Finance, Science, Patents, Tenders, Law texts and Manuals
3. *Detailed label set*: DocLayNet defines 11 class labels to distinguish layout features in high detail.
4. *Redundant annotations*: A fraction of the pages in DocLayNet are double- or triple-annotated, allowing to estimate annotation uncertainty and an upper-bound of achievable prediction accuracy with ML models
5. *Pre-defined train- test- and validation-sets*: DocLayNet provides fixed sets for each to ensure proportional representation of the class-labels and avoid leakage of unique layout styles across the sets.

## Download

| Dataset | Records | Size(GB) | URL  |
|------------------|---------|----------|-------------|
| DocLayNet core dataset| 80,863    | 28 GiB     | [Download](https://codait-cos-dax.s3.us.cloud-object-storage.appdomain.cloud/dax-doclaynet/1.0.0/DocLayNet_core.zip)|
| DocLayNet extra files| 80,863    | 7.5 GiB     | [Download](https://codait-cos-dax.s3.us.cloud-object-storage.appdomain.cloud/dax-doclaynet/1.0.0/DocLayNet_extra.zip)|

Additionally, we provide the labeling guideline used for training of the annotation experts [here](assets/DocLayNet_Labeling_Guide_Public.pdf). 

## Dataset structure

DocLayNet provides four types of data assets:

1. PNG images of all pages, resized to square `1025 x 1025px`
2. Bounding-box annotations in COCO format for each PNG image
3. Extra: Single-page PDF files matching each PNG image
4. Extra: JSON file matching each PDF page, which provides the digital text cells with coordinates and content

The dataset is organized in the following directory structure:

*Doclaynet core dataset*

```
├── COCO
│   ├── test.json
│   ├── train.json
│   └── val.json
├── PNG
│   ├── <hash>.png
│   ├── ...
```

*Doclaynet extra files*

```
├── PDF
│   ├── <hash>.pdf
│   ├── ...
├── JSON
│   ├── <hash>.json
│   ├── ...

```

## Data format details

### Pages
Document pages are provided as bitmap images (PNG) and in original PDF format. The layout annotations in COCO format are referring to the PNG images only.


### COCO annotations
For bounding-box annotations, DocLayNet provides standard COCO format annotations as defined [here](https://cocodataset.org/#format-data).
Each COCO image record contains additional custom fields to allow data sub-selection and provide provenance.


#### Example page with bounding-box annotations

![example_page](assets/132a855ee8b23533d8ae69af0049c038171a06ddfcac892c3c6d7e6b4091c642.png)

#### Example COCO image record

```js
    ...
    {
      "id": 1,
      "width": 1025,
      "height": 1025,
      "file_name": "132a855ee8b23533d8ae69af0049c038171a06ddfcac892c3c6d7e6b4091c642.png",

      // Custom fields:
      "doc_category": "financial_reports" // high-level document category
      "collection": "ann_reports_00_04_fancy", // sub-collection name
      "doc_name": "NASDAQ_FFIN_2002.pdf", // original document filename
      "page_no": 9, // page number in original document
      "precedence": 0, // Annotation order, non-zero in case of redundant double- or triple-annotation
    },
    ...
```

The `doc_category` field uses one of the following constants:

```
financial_reports,
scientific_articles,
laws_and_regulations,
government_tenders,
manuals,
patents
```


#### Example COCO annotation records

The annotation records shown below all belong to the page image shown above. Every bounding-box is a separate record, matched to the common `image_id`.

<details>
<summary>Click to expand...</summary>

```
  "annotations": [
    {
      "id": 8,
      "image_id": 1,
      "category_id": 1,
      "bbox": [
        210.06018382352943,
        31.14536268939389,
        173.9850743464052,
        39.270946654040586
      ],
      "segmentation": [
        [
          210.06018382352943,
          31.14536268939389,
          210.06018382352943,
          70.41630934343448,
          384.04525816993464,
          70.41630934343448,
          384.04525816993464,
          31.14536268939389
        ]
      ],
      "area": 6832.558573256964,
      "iscrowd": 0,
      "precedence": 0
    },
    {
      "id": 9,
      "image_id": 1,
      "category_id": 7,
      "bbox": [
        434.9334063800317,
        -0.4906348977078778,
        589.8585905504372,
        590.2337819428021
      ],
      "segmentation": [
        [
          434.9334063800317,
          -0.4906348977078778,
          434.9334063800317,
          589.7431470450942,
          1024.791996930469,
          589.7431470450942,
          1024.791996930469,
          -0.4906348977078778
        ]
      ],
      "area": 348154.46671203536,
      "iscrowd": 0,
      "precedence": 0
    },
    {
      "id": 10,
      "image_id": 1,
      "category_id": 8,
      "bbox": [
        66.99346405228758,
        112.10344760101009,
        290.869358251634,
        13.66279703282828
      ],
      "segmentation": [
        [
          66.99346405228758,
          112.10344760101009,
          66.99346405228758,
          125.76624463383837,
          357.8628223039216,
          125.76624463383837,
          357.8628223039216,
          112.10344760101009
        ]
      ],
      "area": 3974.0890048610913,
      "iscrowd": 0,
      "precedence": 0
    },
    {
      "id": 11,
      "image_id": 1,
      "category_id": 10,
      "bbox": [
        66.99346405228758,
        133.5865287247475,
        325.3560694444444,
        131.31064046717177
      ],
      "segmentation": [
        [
          66.99346405228758,
          133.5865287247475,
          66.99346405228758,
          264.89716919191926,
          392.34953349673196,
          264.89716919191926,
          392.34953349673196,
          133.5865287247475
        ]
      ],
      "area": 42722.71385863161,
      "iscrowd": 0,
      "precedence": 0
    },
    {
      "id": 12,
      "image_id": 1,
      "category_id": 10,
      "bbox": [
        66.99346405228758,
        272.84557828282834,
        325.4857017973857,
        131.3025
      ],
      "segmentation": [
        [
          66.99346405228758,
          272.84557828282834,
          66.99346405228758,
          404.14807828282835,
          392.4791658496732,
          404.14807828282835,
          392.4791658496732,
          272.84557828282834
        ]
      ],
      "area": 42737.08636025123,
      "iscrowd": 0,
      "precedence": 0
    },
    {
      "id": 13,
      "image_id": 1,
      "category_id": 10,
      "bbox": [
        66.99346405228758,
        414.8919160353536,
        325.69678145424837,
        80.83059406565656
      ],
      "segmentation": [
        [
          66.99346405228758,
          414.8919160353536,
          66.99346405228758,
          495.72251010101013,
          392.6902455065359,
          495.72251010101013,
          392.6902455065359,
          414.8919160353536
        ]
      ],
      "area": 26326.26433021921,
      "iscrowd": 0,
      "precedence": 0
    },
    {
      "id": 14,
      "image_id": 1,
      "category_id": 10,
      "bbox": [
        112.37566899509804,
        626.7556887626263,
        863.111772998366,
        310.9754294507576
      ],
      "segmentation": [
        [
          112.37566899509804,
          626.7556887626263,
          112.37566899509804,
          937.7311182133839,
          975.487441993464,
          937.7311182133839,
          975.487441993464,
          626.7556887626263
        ]
      ],
      "area": 268406.55427217163,
      "iscrowd": 0,
      "precedence": 0
    },
    {
      "id": 15,
      "image_id": 1,
      "category_id": 5,
      "bbox": [
        18.32874183006536,
        1005.2406660353536,
        7.4496732026143775,
        10.353535094696968
      ],
      "segmentation": [
        [
          18.32874183006536,
          1005.2406660353536,
          18.32874183006536,
          1015.5942011300506,
          25.77841503267974,
          1015.5942011300506,
          25.77841503267974,
          1005.2406660353536
        ]
      ],
      "area": 77.13045294729152,
      "iscrowd": 0,
      "precedence": 0
    },
    ...
  ]

```
</details>


### Extra: JSON files

DocLayNet provides auxiliary JSON files which contain the bounding-box coordinates and text for every PDF cell, and additional metadata. This data is generated only from the PDFs and is independent from the annotations.  

#### Example JSON data

The snippet below shows part of the JSON data for the page shown further above. The text cell shown is the section heading (index: 3). 

```js
{
  "metadata": {
    "page_hash": "132a855ee8b23533d8ae69af0049c038171a06ddfcac892c3c6d7e6b4091c642", // unique identifier, equal to filename
    "original_filename": "NASDAQ_FFIN_2002.pdf", // original document filename
    "page_no": 9, // page number in original document
    "num_pages": 28, // total pages in original document
    "original_width": 612, // width in pixels @72 ppi
    "original_height": 792, // height in pixels @72 ppi
    "coco_width": 1025, // with in pixels in PNG and COCO format
    "coco_height": 1025, // with in pixels in PNG and COCO format
    "collection": "ann_reports_00_04_fancy", // sub-collection name
    "doc_category": "financial_reports" // high-level document category
  },
  "cells": [ // all text cells in the digital PDF data
    {
      // Bounding-box coordinates of text cells
      // formatted as [x0,x1,y0,y1]
      // where (x0,y0) is the lower-left corner and
      //       (x1,y1) is the top-right corner
      // in the coordinate space of (0,0, original_width, original_height)
      "bbox": [
        66.99346405228758,
        112.10344760101009,
        290.869358251634,
        13.66279703282828
      ],
      "text": "Leigh Taliaferro, M.D., values consistency.", // string content of cell
      "font": {
        "color": [
          12,
          72,
          142,
          255
        ],
        "name": "/AAAAAC+HelveticaNeue-Medium",
        "size": 1
      }
    },
    ...
```


## Paper

**"DocLayNet: A Large Human-Annotated Dataset for Document-Layout Analysis"** (KDD 2022).

- Birgit Pfitzmann (bpf@zurich.ibm.com)
- Christoph Auer (cau@zurich.ibm.com)
- Michele Dolfi (dol@zurich.ibm.com)
- Ahmed Nassar (ahn@zurich.ibm.com)
- Peter Staar (taa@zurich.ibm.com)

ArXiv link: https://arxiv.org/abs/2206.01062

**Citation:**

```
@article{doclaynet2022,
  title = {DocLayNet: A Large Human-Annotated Dataset for Document-Layout Analysis},  
  doi = {10.1145/3534678.353904},
  url = {https://arxiv.org/abs/2206.01062},
  author = {Pfitzmann, Birgit and Auer, Christoph and Dolfi, Michele and Nassar, Ahmed S and Staar, Peter W J},
  year = {2022}
}
```
