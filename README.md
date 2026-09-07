# Document Field Extraction: OCR vs. Vision-Language Model

Compares an EasyOCR pipeline against a vision-language model (BLIP) for extracting
store names, dates, and totals from real-world receipt photos with varied lighting,
angles, and print quality.

## Method
- **OCR:** EasyOCR (English)
- **VLM:** Salesforce BLIP (blip-vqa-base)
- **Dataset:** 20 real-world receipt photos (Walmart, Trader Joe's, Whole Foods,
  Costco, WinCo, and others), intentionally varied — angled shots, folds, highlighter
  marks — rather than clean scans

## Results

**OCR — Store Name Recognition:** Correctly identified across the large majority of
20 receipts, with two consistent misreads reproduced across repeated runs: Costco
(stylized logo, misread as "cexcg") and Walmart ("WALAMART" — the star icon disrupting
character segmentation).

**OCR — Total Amount Extraction:** Evaluated on 6 receipts, revealing two distinct
failure modes:
1. Character-level misreads (e.g., "$" consistently misread as "8")
2. Field-level dropout — the total missing entirely, correlated with poor image
   conditions (highlighter marks, glare, blur)

**VLM — Store Name:** BLIP correctly identified the store in 3 of 4 test images,
suggesting it can recognize coarse visual patterns like branding or logo style.

**VLM — Date Extraction:** BLIP was incorrect on all 4 images, tested both as part
of a compound question and asked in isolation — ruling out question phrasing as the
cause and pointing to a genuine capability gap in fine-grained text reading.

## Conclusion
OCR substantially outperformed the vision-language model for extracting precise
document text. BLIP-vqa-base, trained for general scene understanding, can
approximate coarse visual categories (store branding) but lacks the fine-grained
text-reading precision needed for small printed fields like dates. OCR, purpose-built
for character-level recognition, is the correct tool for structured document field
extraction — a general-purpose VQA model can supplement coarse classification tasks
but is not a reliable substitute for reading precise text fields.

## Repository Contents
- `receipt_ocr_vlm_comparison.ipynb` — full pipeline: OCR extraction, accuracy
  review, VLM comparison (compound and single-focus questions), and findings
