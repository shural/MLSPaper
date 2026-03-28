# Overleaf Package Guide

**Created**: 2026-03-28
**Location**: `/Users/media/gpu_architecture_survey/`

---

## 📦 Two Packages Created

### 1. **gpu_survey_optionA_overleaf.zip** (36 KB)
**Version**: Density Optimization (325 lines)
- **Focus**: Concise, streamlined narrative
- **Style**: Bottleneck-driven analysis with efficient presentation
- **Accuracy**: Verified and aligned with Option B (10/10 ground truth)
- **Best for**: Conference submissions with strict page limits

### 2. **gpu_survey_optionB_overleaf.zip** (40 KB)
**Version**: Comprehensive Coverage (550+ lines)
- **Focus**: In-depth technical details
- **Style**: Thorough explanations with extensive context
- **Accuracy**: 10/10 verified (used as ground truth reference)
- **Best for**: Journal submissions, technical reports, reference material

---

## 📋 Package Contents (Both Identical Structure)

Each zip contains:
```
overleaf_option[A|B]/
├── gpu_architecture_survey.tex    ← Main paper (Option A or B)
├── icml2026.sty                    ← ICML 2026 conference style
├── icml2026.bst                    ← Bibliography style
├── references.bib                  ← Bibliography database
├── fancyhdr.sty                    ← Header/footer formatting
├── algorithm.sty                   ← Algorithm environment
├── algorithmic.sty                 ← Algorithm formatting
└── README.txt                      ← Upload instructions
```

---

## 🚀 Overleaf Upload Instructions

### Step-by-Step:

1. **Go to Overleaf**: https://www.overleaf.com/
2. **Create New Project**:
   - Click "New Project"
   - Select "Upload Project"
3. **Upload Zip**:
   - Choose either `gpu_survey_optionA_overleaf.zip` or `gpu_survey_optionB_overleaf.zip`
   - Overleaf will automatically extract all files
4. **Set Main File**:
   - Overleaf should auto-detect `gpu_architecture_survey.tex` as main file
   - If not, click Menu → Main document → select `gpu_architecture_survey.tex`
5. **Compile**:
   - Click "Recompile"
   - First compilation may take 30-60 seconds
   - PDF will appear in right pane

### Compiler Settings:
- **Compiler**: pdfLaTeX (default)
- **Main document**: gpu_architecture_survey.tex
- **Auto compile**: On (recommended)

---

## 📊 Key Differences Between Options

| Feature | Option A | Option B |
|---------|----------|----------|
| **Length** | ~325 lines | ~550 lines |
| **Emphasis** | Bottleneck evolution | Comprehensive technical detail |
| **Tables** | Minimal, focused | Extensive, detailed |
| **Specifications** | All verified ✅ | All verified ✅ (ground truth) |
| **Style** | Dense, efficient | Thorough, explanatory |
| **Page count** | ~8-10 pages | ~12-15 pages |

---

## ✅ Verified Specifications (Both Options)

All specifications have been cross-verified and are consistent:

| Specification | Verified Value |
|---------------|----------------|
| Hopper bandwidth | 3.35 TB/s |
| Hopper compute ratio | 597 FLOPs/byte |
| Ampere bandwidth (40GB) | 1.6 TB/s |
| Ampere bandwidth (80GB) | 2.039 TB/s |
| Volta specialization | ~20% |
| Hopper specialization | ~15% |
| Turing INT8 | 65 TOPS |
| Fermi FP32 | 1.03 TFLOPS |
| Fermi compute ratio | 5.8 FLOPs/byte |

---

## 🔍 Quality Assurance

### Option A:
- ✅ Aligned with Option B ground truth
- ✅ Internal consistency verified (no conflicting specs)
- ✅ All quiz materials updated to match

### Option B:
- ✅ 10/10 Gemini validation score
- ✅ Used as authoritative reference
- ✅ Maximum technical depth

---

## 📝 Compilation Notes

### Expected Output:
- **Title**: "GPU Architecture Evolution Under ML Workload Pressure: A Causal, Bottleneck-Driven Analysis from Fermi to Rubin"
- **Format**: ICML 2026 conference style
- **Bibliography**: Automatic processing via BibTeX

### Common Issues:
1. **Bibliography not appearing**:
   - Run: pdfLaTeX → BibTeX → pdfLaTeX → pdfLaTeX
   - Overleaf does this automatically on recompile

2. **Missing packages**:
   - All required packages (.sty files) are included
   - No additional packages needed

3. **Compilation errors**:
   - Check error log (bottom of Overleaf interface)
   - All packages tested and verified

---

## 🎯 Recommended Usage

### For Conference Submission (ICML, NeurIPS, ICLR):
- **Use**: Option A (Density Optimization)
- **Why**: Meets strict page limits, focused narrative

### For Journal Submission (JMLR, TPAMI):
- **Use**: Option B (Comprehensive Coverage)
- **Why**: No page limits, reviewers expect depth

### For Technical Report:
- **Use**: Option B (Comprehensive Coverage)
- **Why**: Maximum detail for reference

### For Exam/Quiz Preparation:
- **Use**: Either (both have same verified facts)
- **Reference**: Quiz materials already updated

---

## 📂 Related Files

In the main project directory:
- `UPDATE_SUMMARY.md` - Complete change history
- `QUIZ_STUDY_GUIDE.md` - Updated with verified specs
- `ONE_PAGE_CHEATSHEET.md` - Quick reference (updated)
- `SUPPLEMENTARY_QUESTIONS.md` - Extended Q&A (updated)

---

## 🎓 Academic Integrity

Both papers contain:
- Proper citations (references.bib)
- ICML 2026 formatting
- Author/affiliation placeholders (update before submission)
- Abstract and keywords

**Before submission**: Update author names, affiliations, and contact info in the .tex file.

---

## 💾 Backup

Both zip files are saved in:
```
/Users/media/gpu_architecture_survey/
├── gpu_survey_optionA_overleaf.zip
└── gpu_survey_optionB_overleaf.zip
```

---

## ✨ Summary

Two publication-ready packages created for Overleaf upload. Both contain verified, accurate specifications. Choose based on your needs:
- **Option A**: Concise and focused (conferences)
- **Option B**: Comprehensive and detailed (journals/reports)

Both are ready to upload and compile immediately in Overleaf!

---

**Questions?** Refer to README.txt inside each zip package for detailed instructions.
