# Apple Flower Segmentation Dataset

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](#changelog)

Comprehensive dataset for semantic segmentation of apple, peach, and pear flowers. Contains high-resolution images with pixel-level segmentation masks, enabling training and evaluation of segmentation models for agricultural AI applications.

- Project page: `https://agdatacommons.nal.usda.gov/download/articles/24852636/versions/1/`
- Original source: USDA Ag Data Commons

## TL;DR
- Task: segmentation (+ detection)
- Modality: RGB
- Platform: ground
- Real/Synthetic: real
- Images: Apples 165; Peaches 24; Pears 18
- Resolution: High-resolution (typically 5184×3456)
- Annotations: Pixel-level segmentation masks (PNG), per-image CSV, JSON, COCO format available
- License: CC BY 4.0 (see LICENSE)
- Citation: see below

## Table of contents
- [Download](#download)
- [Dataset structure](#dataset-structure)
- [Sample images](#sample-images)
- [Annotation schema](#annotation-schema)
- [Stats and splits](#stats-and-splits)
- [Quick start](#quick-start)
- [Evaluation and baselines](#evaluation-and-baselines)
- [Datasheet (data card)](#datasheet-data-card)
- [Known issues and caveats](#known-issues-and-caveats)
- [License](#license)
- [Citation](#citation)
- [Changelog](#changelog)
- [Contact](#contact)

## Download
- Original dataset: `https://agdatacommons.nal.usda.gov/download/articles/24852636/versions/1/`
- This repo hosts structure and conversion scripts only; place the downloaded folders under this directory.
- Local license file: see `LICENSE` (Creative Commons Attribution 4.0 International).

## Dataset structure
```
apple_flower_segmentation/
├── apples/
│   ├── csv/                      # CSV annotations per image
│   ├── json/                     # JSON annotations per image
│   ├── images/                   # JPG/BMP images
│   ├── segmentations/           # PNG segmentation masks
│   ├── labelmap.json            # Label mapping
│   └── sets/                     # Dataset splits
│       ├── train.txt
│       ├── val.txt
│       ├── test.txt
│       ├── all.txt
│       └── train_val.txt
├── peaches/
│   ├── csv/
│   ├── json/
│   ├── images/
│   ├── segmentations/
│   ├── labelmap.json
│   └── sets/
├── pears/
│   ├── csv/
│   ├── json/
│   ├── images/
│   ├── segmentations/
│   ├── labelmap.json
│   └── sets/
├── annotations/                  # COCO format JSON (generated)
│   ├── apples_instances_train.json
│   ├── apples_instances_val.json
│   ├── apples_instances_test.json
│   └── ...
├── scripts/
│   ├── convert_to_coco.py       # Convert CSV to COCO format
│   ├── generate_annotations.py  # Generate JSON annotations from masks
│   └── organize_data.py         # Organize data into standard structure
├── data/                         # Original data directory (optional)
│   ├── origin/
│   │   ├── AppleA/
│   │   ├── AppleB_1/
│   │   ├── Peach_1/
│   │   ├── Pear_1/
│   │   └── ...
├── LICENSE
├── README.md
└── requirements.txt
```
- Splits: `{category}/sets/train.txt`, `{category}/sets/val.txt`, `{category}/sets/test.txt` (and also `all.txt`, `train_val.txt`) list image basenames (no extension). If missing, all images are used.

## Sample images

Below are example images for each fruit category in this dataset. Paths are relative to this README location.

<table>
  <tr>
    <th>Category</th>
    <th>Sample</th>
  </tr>
  <tr>
    <td><strong>Apple Flower</strong></td>
    <td>
      <img src="apples/images/img_0251.jpg" alt="Apple flower example" width="260"/>
      <div align="center"><code>apples/images/img_0251.jpg</code></div>
    </td>
  </tr>
  <tr>
    <td><strong>Peach Flower</strong></td>
    <td>
      <img src="peaches/images/10.bmp" alt="Peach flower example" width="260"/>
      <div align="center"><code>peaches/images/10.bmp</code></div>
    </td>
  </tr>
  <tr>
    <td><strong>Pear Flower</strong></td>
    <td>
      <img src="pears/images/1_100.bmp" alt="Pear flower example" width="260"/>
      <div align="center"><code>pears/images/1_100.bmp</code></div>
    </td>
  </tr>
</table>

## Annotation schema
- CSV per-image schemas (stored under each category's `csv/` folder):
  - Columns include `#item, x, y, width, height, label` (bounding boxes in absolute pixel coordinates).
  - Bounding boxes are derived from segmentation masks.
- JSON per-image schemas (stored under each category's `json/` folder):
  - Each image has a corresponding JSON file with COCO-style format
  - Bounding boxes: `[x, y, width, height]` in absolute pixel coordinates
- Segmentation masks (stored under each category's `segmentations/` folder):
  - PNG format masks where white pixels (255) indicate flower regions
- COCO-style (generated):
```json
{
  "info": {"year": 2017, "version": "1.0.0", "description": "Apple Flower Segmentation apples train split", "url": "https://agdatacommons.nal.usda.gov/download/articles/24852636/versions/1/"},
  "images": [{"id": 1, "file_name": "apples/images/xxx.jpg", "width": 5184, "height": 3456}],
  "categories": [{"id": 1, "name": "apple_flower", "supercategory": "flower"}],
  "annotations": [{"id": 1, "image_id": 1, "category_id": 1, "bbox": [x, y, w, h], "area": 1234, "iscrowd": 0}]
}
```

- Label maps: each category folder includes a `labelmap.json` for category mapping.

## Stats and splits
- Apples: 165 images; Peaches: 24 images; Pears: 18 images.
- Splits provided via `{category}/sets/*.txt`. You may define your own splits by editing those files.

**Apples**:
- Training set: 100 images (`apples/sets/train.txt`)
- Validation set: 29 images (`apples/sets/val.txt`)
- Test set: 36 images (`apples/sets/test.txt`)

**Peaches**:
- Training set: 16 images (`peaches/sets/train.txt`)
- Validation set: 3 images (`peaches/sets/val.txt`)
- Test set: 5 images (`peaches/sets/test.txt`)

**Pears**:
- Training set: 12 images (`pears/sets/train.txt`)
- Validation set: 2 images (`pears/sets/val.txt`)
- Test set: 4 images (`pears/sets/test.txt`)

## Quick start
Python (COCO):
```python
from pycocotools.coco import COCO
coco = COCO("annotations/apples_instances_train.json")
img_ids = coco.getImgIds()
img = coco.loadImgs(img_ids[0])[0]
ann_ids = coco.getAnnIds(imgIds=img['id'])
anns = coco.loadAnns(ann_ids)
```

Convert to COCO JSON:
```bash
python scripts/convert_to_coco.py --root . --out annotations --categories apples peaches pears --splits train val test
```

Dependencies:
```bash
python -m pip install pillow
```

Optional for the COCO API example:
```bash
python -m pip install pycocotools
```

## Evaluation and baselines
- Metric: mIoU (mean Intersection over Union) for segmentation, mAP@[.50:.95] for detection
- Reference results: See citation paper for baseline results.

## Datasheet (data card)
- Motivation: Flower segmentation in orchards for yield estimation and agricultural monitoring
- Composition: High-resolution RGB images of fruit flowers (apple, peach, pear) with pixel-level segmentation masks
- Collection process: Images collected from USDA Ag Data Commons, containing high-quality images of fruit flowers for agricultural research
- Preprocessing: Images organized into standardized structure. Segmentation masks converted to bounding boxes for detection tasks.
- Distribution: Dataset available via USDA Ag Data Commons
- Maintenance: Maintained by USDA Ag Data Commons

## Known issues and caveats
- Original annotations were in pixel-level segmentation masks (PNG format), which have been converted to bounding boxes `[x, y, width, height]` for compatibility with detection frameworks.
- Image formats: Dataset contains both JPG and BMP format images. BMP images are processed identically to JPG images and included in all splits.
- Coordinate system: Origin at top-left corner, units in pixels.
- File naming: Images follow various naming patterns (e.g., `IMG_XXXX.JPG`, numeric names like `1.bmp`, `10.bmp`).
- Mask files may use numeric names while images use descriptive names (e.g., mask `248.png` corresponds to image `IMG_0248.JPG`).

## License
- Creative Commons Attribution 4.0 (`LICENSE`). Check the original dataset terms and cite appropriately.

## Citation
If you use this dataset in your research, please cite:

```bibtex
@misc{apple_flower_segmentation_2025,
  title={Apple Flower Segmentation Dataset},
  author={USDA Ag Data Commons},
  year={2025},
  howpublished={USDA Ag Data Commons},
  url={https://agdatacommons.nal.usda.gov/download/articles/24852636/versions/1/}
}
```

## Changelog
- V1.0.0: Initial standardized structure and COCO conversion utility

## Contact
- Maintainers: Dataset structure maintained by the standardization team
- Original authors: USDA Ag Data Commons
- Source: `https://agdatacommons.nal.usda.gov/download/articles/24852636/versions/1/`
