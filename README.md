# 🧠 Feature Extraction from Images - Amazon ML Challenge Hackathon

This repository contains our solution for the **Feature Extraction from Images** challenge, where the task is to extract key product entity values (e.g., weight, dimensions, volume, etc.) from product images using computer vision and OCR techniques.

---

## 📌 Problem Statement

In digital marketplaces, many product listings lack complete textual descriptions. This project focuses on extracting **entity values** such as weight, volume, voltage, wattage, and dimensions directly from product images, which is critical for e-commerce accuracy and automation.

The primary goal is to predict the **entity_value** (e.g., `2.5 kilogram`, `12.5 centimetre`, etc.) for a given product image using computer vision models.

---

## 🗃️ Dataset

The dataset includes product images and metadata:

- `train.csv`: Labeled training data with ground truth `entity_value`s.
- `test.csv`: Unlabeled test data for prediction.
- `sample_test.csv`: Sample test input.
- `sample_test_out.csv`: Sample formatted output.

### Columns:

- `index`: Unique ID
- `image_link`: URL to the product image
- `group_id`: Product category code
- `entity_name`: The type of entity (e.g., `item_weight`)
- `entity_value`: The actual entity value (only in train set)

---

## 🧰 File Structure

.
├── AIvengers Code 1.ipynb   
├── EASYOCR preliminary testing.ipynb  
├── Explaination for Submission 1.docx  
├── combined_results.csv  
├── halfresults_part_1.csv  
├── halfresults_part_2.csv  
├── halfresults_part_3.csv  
├── halfresults_part_4.csv  
├── halfresults_part_5.csv  
├── halfresults_part_6.csv  
├── halfresults_part_7.csv  
├── submission.csv # 🔍 Final output predictions for test set  
├── testhalf.csv  
├── .gitignore  
├── notebooks/  
│ ├── Attempt 1.ipynb  
│ ├── EASYOCR.ipynb  
│ ├── HeightWidthEdited.ipynb  
│ ├── Program 1.ipynb  
│ ├── Tesseract.ipynb  
├── Amazon ML.docx  


---

## 🚀 Approach

We used a combination of OCR (Optical Character Recognition) tools and rule-based post-processing to extract and normalize the values.

### ✅ Tools & Techniques:

- **OCR Engines**:
  - [EasyOCR](https://github.com/JaidedAI/EasyOCR)
  - [Tesseract OCR](https://github.com/tesseract-ocr/tesseract)

- **Image Preprocessing**:
  - Resizing, thresholding, denoising
  - Region of Interest (ROI) filtering for text-rich zones

- **Text Cleaning and Extraction**:
  - Regex and string parsing to extract numeric values and units
  - Unit normalization based on `constants.py` constraints

- **Post-Processing**:
  - Format predictions as `"x unit"` (e.g., `34 gram`)
  - Discard invalid or ambiguous predictions
  - Ensure formatting compatibility with sanity checker

---

## ✅ Output Format

The `submission.csv` must contain:

| index | prediction         |
|-------|--------------------|
| 1     | 12.5 centimetre    |
| 2     | 100 ml             |
| 3     | 250 gram           |

**Important**:  
- All values must match one of the allowed units defined in `constants.py`.  
- Output must pass the sanity checker using `src/sanity.py`.

---

## 📊 Evaluation Criteria

Submissions are evaluated using **F1 Score**, computed as follows:

1. **True Positives**: OUT ≠ "" and GT ≠ "" and OUT == GT  
2. **False Positives**:  
   - OUT ≠ "" and GT ≠ "" and OUT ≠ GT  
   - OR OUT ≠ "" and GT == ""  
3. **False Negatives**: OUT == "" and GT ≠ ""  
4. **True Negatives**: OUT == "" and GT == ""

**F1 Score** = `2 * Precision * Recall / (Precision + Recall)`

Where:  
- Precision = TP / (TP + FP)  
- Recall = TP / (TP + FN)

---

## 🔄 How to Run

1. Clone the repo and install dependencies:

```
pip install -r requirements.txt
```
2. Download images using:

``` 
from src.utils import download_images
download_images(image_link_list, output_dir)
```

3. Run OCR-based extraction from notebooks (EASYOCR.ipynb, Tesseract.ipynb, etc.).

4. Combine and clean outputs. Save final predictions in submission.csv.

5. Validate using the provided sanity checker:

```
python src/sanity.py --file submission.csv
```
You should see:

```
Parsing successful for file: submission.csv
```

## 🧠 Learnings
OCR on real-world product images requires robust preprocessing.

OCR accuracy varies significantly across fonts and lighting.

Entity extraction requires careful rule design and regex patterns.

Unit normalization and strict formatting are essential for evaluation.

## 👥 Team
🧑‍💻 Akshay Kumar
Team AIvengers | B.Tech in Robotics and AI, VIT Chennai

## 📄 License
This repository is open-sourced for learning purposes.
Check individual dependencies and external tools for their respective licenses.

## 📬 Acknowledgements
Amazon ML Challenge organizers for the opportunity

Open-source contributors of EasyOCR, Tesseract, and Python libraries
