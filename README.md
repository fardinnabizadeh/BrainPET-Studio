
Open-source desktop app for quantitative PET neuroimaging analysis — atlas-based SUVR, MNI-space registration, PVC, and interactive QC. Windows installer available.


================================================================
BrainPET Studio

================================================================

<img width="588" height="643" alt="Logo" src="https://github.com/user-attachments/assets/d18a6bdf-c3d1-44d3-8176-620981cbf9f8" />


OVERVIEW
--------
BrainPET Studio is an open-source desktop application for
quantitative analysis of PET neuroimaging data. It is designed
for researchers and clinicians working with tau, amyloid, and
metabolic PET tracers who need a transparent, flexible, and
reproducible quantification workflow.

The tool operates entirely in MNI standard space, enabling
atlas-based regional analysis without requiring subject-specific
FreeSurfer parcellations. It combines automated image
registration, optional partial volume correction, interactive
visual quality control, and flexible SUVR calculation in a
single integrated platform.

BrainPET Studio is particularly suited for:
- Multi-site studies requiring a standardized MNI-space pipeline
- Researchers without access to FreeSurfer or SPM licenses
- Rapid prototyping of quantification workflows
- Educational use in PET neuroimaging methodology


INSTALLATION
------------
Windows (Recommended):
  1. Download BrainPET_Studio_Setup.exe from the Releases page:
     (https://file.kiwi/1dbedf61#Zl5oZDMSZGx4qb_YcfzesA)
  2. Run the installer and follow the setup wizard.
  3. Launch BrainPET Studio from the Desktop or Start Menu.

  No Python installation required. All dependencies are
  bundled within the installer.

From Source (Advanced):
  git clone https://github.com/your-username/brainpet-studio.git
  cd brainpet-studio
  pip install -r requirements.txt
  python main.py


CORE FEATURES
-------------

Image Processing:
- Affine registration of PET and T1 MRI to MNI standard space
- Isotropic Gaussian smoothing with adjustable FWHM
- Muller-Gartner (MG) partial volume correction
- Support for .nii and .nii.gz input formats

Interactive Viewer:
- Linked orthogonal viewer (axial, coronal, sagittal)
- Real-time manual alignment refinement (translation + rotation)
- Adjustable PET/MRI overlay transparency and intensity windowing
- Atlas overlay for visual parcellation QC

Quantification:
- Atlas-based regional extraction (any integer-labeled NIfTI atlas)
- Two modes: Mean Uptake and SUVR
- User-defined reference region via binary NIfTI mask
- Per-region output: mean, SD, voxel count, volume (mm3)

Multi-Tracer Support:
- AV1451 (flortaucipir), MK-6240, PI-2620, RO-948
- FDG, PIB, florbetapir, florbetaben, flutemetamol
- Tracer-specific default smoothing and reference region recommendations

Output:
- CSV quantification tables (mean uptake or SUVR)
- Processed NIfTI images (normalized, smoothed, PVC-corrected)
- Saved transformation matrices

Batch Processing:
- Command-line interface for multi-subject pipelines
- Per-subject processing logs


INSTALLED FILE STRUCTURE
------------------------
After installation, the following structure is created:

  Drive:\Program Files\BrainPET Studio\
  ├── BrainPET Studio.exe       Main application
  ├── atlases\                  Included NIfTI atlas files
  ├── references\               Reference region masks (MNI space)
  └── README.txt                This file

  Note: Do not move or rename files in this directory.
  The application expects this structure to function correctly.


QUICK START
-----------
1. Launch BrainPET Studio from the Desktop or Start Menu shortcut.

2. Load your data:
   - Select PET image (.nii / .nii.gz)
   - Select T1 MRI image
   - Select atlas file and label CSV
   - Choose tracer from dropdown

3. Register to MNI:
   - Click "Register to MNI"
   - Inspect alignment in the orthogonal viewer
   - Use manual alignment sliders if refinement is needed

4. Configure quantification:
   - Select "Mean Uptake" or "SUVR" mode
   - If SUVR: browse and select reference region mask
   - Set output directory

5. Run quantification:
   - Click "Quantify Atlas"
   - Results saved as CSV in output directory

6. Batch processing (CLI, source install only):
   python batch_process.py \
     --subjects subjects.txt \
     --atlas atlas.nii.gz \
     --labels labels.csv \
     --tracer av1451 \
     --mode suvr \
     --ref-mask cereb_mask.nii.gz \
     --output-dir ./results


VALIDATION & ACCURACY
---------------------
BrainPET Studio has been benchmarked against the UC Berkeley
ADNI AV1451 pipeline. Expected correlations vs. ADNI reference:

  Large cortical composites (e.g., meta-temporal): r = 0.85-0.90
  Individual cortical regions:                     r = 0.80-0.88
  Small medial temporal regions (e.g., hippocampus): r = 0.75-0.85

Methodological differences vs. UC Berkeley ADNI pipeline:

  Aspect               BrainPET Studio         UC Berkeley ADNI
  ----------------     --------------------    --------------------
  Analysis space       MNI standard space      Native subject space
  Parcellation         Atlas-based (NIfTI)     FreeSurfer 7.1.1
  PVC method           Muller-Gartner (MG)     Geometric Transfer Matrix
  Reference region     User-defined MNI mask   FreeSurfer inf. cereb. GM
  Composite ROIs       simple mean             volume-weighted




TECHNICAL SPECIFICATIONS
------------------------
Language:           Python 3.8+
GUI framework:      Tkinter
Core libraries:     NumPy, SciPy, NiBabel, scikit-image
Atlas format:       Integer-labeled NIfTI (.nii / .nii.gz)
Label table format: CSV (columns: index, name)
Output format:      CSV (quantification), NIfTI (processed images)
Platform:           Windows (installer), macOS/Linux (source)
License:            MIT


SYSTEM REQUIREMENTS
Minimum            
OS: Windows 10          
RAM:            8 GB
Disk space:     2 GB
GPU:            Not required        




ROADMAP
-------
v1.1  Logan graphical analysis for DVR quantification
v1.2  SRTM/SRTM2 kinetic modeling support
v1.3  BIDS-compatible input/output structure
v1.4  Longitudinal change score computation
v2.0  Deep learning-based registration and tissue segmentation
v2.1  Cloud batch processing integration
Future: Dynamic PET frames and parametric mapping


LIMITATIONS
-----------
- MNI-space analysis: normalization error increases variability
  for subjects with atypical anatomy or significant atrophy.
- Atlas dependency: accuracy depends on atlas quality and
  resolution; standard atlases may not capture individual
  anatomical variability.
- PVC method: MG assumes uniform gray matter activity and may
  underperform in regions with high spatial heterogeneity.
- Reference region: choice significantly affects SUVR values;
  users are responsible for selecting an appropriate mask.
- No kinetic modeling: static PET quantification only.
- No longitudinal pipeline: change scores must be computed
  externally from CSV outputs.
- Manual QC required: automated registration should always be
  visually inspected.


LICENSE
-------
Fardin Nabizadeh

Copyright (c) 2026 BrainPET Studio 

Permission is hereby granted, free of charge, to any person
obtaining a copy of this software and associated documentation
files (the "Software"), to deal in the Software without
restriction, including without limitation the rights to use,
copy, modify, merge, publish, distribute, sublicense, and/or
sell copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following
conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.


CONTACT & SUPPORT
-----------------
Bug reports & feature requests:
  https://github.com/fardinnabizadeh/brainpet-studio/issues

Discussions & questions:
  (https://github.com/fardinnabizadeh/BrainPET-Studio/issues/)

Email: <fardinnabizade1378@gmail.com>

When reporting a bug, please include:
- Operating system version
- Full error message or screenshot
- Input file types and dimensions
- Steps to reproduce the issue

================================================================


---

Key changes from your original:
- Added a proper **Installation** section at the top with the `.exe` installer as the primary method
- Added **Installed File Structure** section showing what gets installed where
- Quick Start now starts with "Launch from Desktop shortcut" instead of `git clone`
- System Requirements now lists Windows as primary, macOS/Linux as source-only
- Batch processing CLI noted as source-install only
- GitHub About description updated to mention the Windows installer
