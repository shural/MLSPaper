# GPU Architecture Survey - File Organization Plan

## Current Status Analysis

### Existing Structure
```
gpu_architecture_survey/
├── paper/                    ✅ Well-organized (LaTeX, PDFs, Overleaf packages)
├── QUIZ/                     ✅ Well-organized (just reorganized)
├── figures/                  ✅ Appropriate location
├── research/                 ✅ Appropriate location
├── overleaf_optionA/         ⚠️ Redundant (extracted files)
├── overleaf_optionB/         ⚠️ Redundant (extracted files)
└── [12 .md files in root]    ❌ Needs organization
└── [5 old .zip/.pdf files]   ❌ Should be archived or deleted
```

---

## Recommended Organization

### Option 1: Comprehensive Organization (Recommended)

Create three new folders to organize root-level files:

#### 1. **validation/** - Quality Assurance Documents
Move all validation and testing reports:
```
validation/
├── FINAL_VALIDATION_10_10.md          (Final Gemini 10/10 validation)
├── FINAL_VALIDATION_SUMMARY.md        (Validation summary)
├── VALIDATION_REPORT.md               (Detailed validation report)
└── VALIDATION_RESULTS.md              (Validation results)
```

#### 2. **docs/** - Process Documentation & Analysis
Move development documentation and analysis:
```
docs/
├── development/
│   ├── SESSION_SUMMARY.md             (Session notes)
│   ├── UPDATE_SUMMARY.md              (Update logs)
│   ├── TABLE_OPTIMIZATION_SUMMARY.md  (Table optimization notes)
│   ├── TRIMMING_OPTIONS.md            (Old trimming options)
│   └── TRIMMING_OPTIONS_V2.md         (New trimming options)
├── comparison/
│   └── OPTION_A_VS_B_COMPARISON.md    (Detailed A vs B comparison)
└── guides/
    └── OVERLEAF_PACKAGES_README.md    (Overleaf upload guide)
```

#### 3. **archive/** - Old/Deprecated Files
Move superseded files:
```
archive/
├── old_packages/
│   ├── gpu_survey_optionA_overleaf.zip  (old, superseded by paper/)
│   ├── gpu_survey_optionB_overleaf.zip  (old, superseded by paper/)
│   ├── gpu_survey_overleaf.zip          (old)
│   ├── gpu_survey_overleaf.pdf          (old)
│   └── gpu_survey_optionB_overleaf.pdf  (old)
└── extracted_overleaf/
    ├── overleaf_optionA/                (extracted files, redundant)
    └── overleaf_optionB/                (extracted files, redundant)
```

#### Final Structure:
```
gpu_architecture_survey/
├── README.md                 (Main documentation - stays in root)
├── paper/                    (LaTeX papers, PDFs, current Overleaf packages)
├── QUIZ/                     (All quiz preparation materials)
├── validation/               (Quality assurance, validation reports)
├── docs/                     (Process docs, comparisons, guides)
├── archive/                  (Old files, superseded versions)
├── figures/                  (Figures for papers)
└── research/                 (Research materials)
```

---

### Option 2: Minimal Organization (Simpler)

Create just two folders:

#### 1. **validation/** - All validation files
Same as Option 1

#### 2. **archive/** - Everything else that's not actively used
```
archive/
├── OPTION_A_VS_B_COMPARISON.md
├── SESSION_SUMMARY.md
├── UPDATE_SUMMARY.md
├── TABLE_OPTIMIZATION_SUMMARY.md
├── TRIMMING_OPTIONS.md
├── TRIMMING_OPTIONS_V2.md
├── OVERLEAF_PACKAGES_README.md  (or move to paper/)
├── [all old .zip/.pdf files]
├── overleaf_optionA/
└── overleaf_optionB/
```

---

## Files to Keep in Root

Only these should remain in root:
- **README.md** - Main project documentation
- **.gitignore** - Git configuration
- (Optionally) **ORGANIZATION_PLAN.md** - This file

---

## Deletion Candidates

Consider deleting (after confirming no dependencies):

### Definitely Delete:
- `.DS_Store` files (macOS metadata, should be in .gitignore)
- Old zip files if paper/ has current versions
- Old PDF files if paper/ has current versions

### Probably Delete:
- `overleaf_optionA/` and `overleaf_optionB/` directories
  - These are extracted versions of the zip packages
  - Redundant if you have the source .tex files in paper/
  - Can always re-extract from .zip if needed

### Keep (Archive):
- All .md documentation files (historical value)
- Move to archive/ rather than delete

---

## Implementation Steps

### For Option 1 (Comprehensive):

```bash
# 1. Create folders
mkdir -p validation docs/development docs/comparison docs/guides archive/old_packages

# 2. Move validation files
git mv FINAL_VALIDATION_10_10.md validation/
git mv FINAL_VALIDATION_SUMMARY.md validation/
git mv VALIDATION_REPORT.md validation/
git mv VALIDATION_RESULTS.md validation/

# 3. Move documentation
git mv SESSION_SUMMARY.md docs/development/
git mv UPDATE_SUMMARY.md docs/development/
git mv TABLE_OPTIMIZATION_SUMMARY.md docs/development/
git mv TRIMMING_OPTIONS.md docs/development/
git mv TRIMMING_OPTIONS_V2.md docs/development/
git mv OPTION_A_VS_B_COMPARISON.md docs/comparison/
git mv OVERLEAF_PACKAGES_README.md docs/guides/

# 4. Move old files to archive
mv gpu_survey_*.zip archive/old_packages/
mv gpu_survey_*.pdf archive/old_packages/
mv overleaf_optionA archive/extracted_overleaf/
mv overleaf_optionB archive/extracted_overleaf/

# 5. Add archive to .gitignore (optional)
echo "archive/" >> .gitignore

# 6. Commit
git add -A
git commit -m "Reorganize project structure: validation/, docs/, archive/"
git push
```

---

## Recommendation

**Use Option 1 (Comprehensive Organization)**

**Rationale:**
1. **Clarity**: Clear separation of concerns (validation, docs, archive, active work)
2. **Maintainability**: Easy to find related files
3. **Professional**: Shows thoughtful project organization
4. **Scalability**: Easy to add more files to appropriate folders
5. **Clean root**: Only README.md in root = easy to navigate

**Benefits:**
- Quiz materials → `QUIZ/`
- Paper work → `paper/`
- Validation → `validation/`
- Documentation → `docs/`
- Old stuff → `archive/` (or delete)

This makes the repository publication-ready and easy for collaborators to navigate.

---

## Alternative: Update .gitignore

If you choose to delete archived files entirely, update `.gitignore`:

```
# Build artifacts
*.aux
*.log
*.out
*.toc
*.bbl
*.blg

# OS files
.DS_Store
**/.DS_Store

# OMC state
.omc/

# Archived/old files
archive/
overleaf_optionA/
overleaf_optionB/
*.zip
*.pdf
```

Then remove from git:
```bash
git rm --cached archive/ overleaf_optionA/ overleaf_optionB/
git commit -m "Remove archived files from version control"
```

---

**Next Steps:**
1. Choose Option 1 or Option 2
2. Execute the reorganization commands
3. Verify structure with `tree` or `ls -la`
4. Commit and push
5. Update README.md to document new structure
