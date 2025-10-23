**PROJECT STATE DOCUMENT**  
**Title:** Software Architecture Proposal — Current State Overview  
**Date:** July 2025

---
### 1. **Project Introduction**

This project is a modular, file- and folder-based **scientific data analysis platform** written in Python. It is designed to support physicists and engineers working with data from instruments such as oscilloscopes and time-domain measurement systems (TDMS). The architecture emphasizes clean separation of concerns across modules, reusability of code, and extensibility for new analysis or visualization methods.

The project is built with the goal of being **future-proof, testable, and scalable**, targeting local instrument-based workflows and future database-backed setups (e.g., PostgreSQL, InfluxDB).

---
### 2. **Current High-Level Architecture**

#### Core Components:

- `analyze_tdms_file.py`: Main entry point for analyzing TDMS files (either single file or folder-based).
- `file_loader`: Responsible for reading TDMS files from disk.
- `data_utils`: Cleans and processes loaded data into a standard format.
- `plotting`: Contains separate plotting functions, e.g., `plot_intensity`, `plot_xy`.
- `filtering`: Holds data preprocessing methods like normalization and smoothing.
- `analysis`: Placeholder for data analysis techniques (e.g., peak finding, FFT).
- `config.py`: Centralizes global constants and default settings.

#### Design Patterns and Practices:

- Modular structure across folders (e.g., `utils/`, `analysis/`, `plotting/`).
- Functional approach to avoid stateful complexity.
- Plotting separated from analysis and preprocessing.
- Designed to work without database, but compatible for future integration.

---

### 3. **Functional Capabilities Completed**

#### ✅ File Handling:

- Loads both single `.tdms` files and folders containing TDMS files.
- Automatically detects sweep parameters from folder-based structure.

#### ✅ Data Processing:

- Data is structured into three columns: `param`, `channel`, `value`.
- NaN rows are dropped during processing.
- Normalization function implemented in `filtering` module.
    - Normalizes each sweep parameter group independently.
- Grouping by sweep parameter (`param`) works as intended.

#### ✅ Plotting:

- `plot_intensity()` supports intensity heatmap creation with:
    - X-axis: Channel
    - Y-axis: Sweep parameter
    - Color: Normalized data value
- Plotting now fully decoupled from processing logic.
- Keyword argument bug in `plot_intensity()` has been fixed.

#### ✅ Modular Preprocessing:

- Filters and normalizers operate independently from plotting.
- Can preprocess data before visualization or saving.
- Can easily extend with filters like smoothing or detrending.

---

### 4. **Partially Completed / In Progress**

- **Error Handling**: Not comprehensive. Needs robust try-except blocks for file loading and data inconsistencies.
- **Analysis Module**: Currently just a placeholder. Needs implementation for feature extraction like FFT, baseline correction, peak detection, etc.
- **Metadata Handling**: No current mechanism for saving or using experiment metadata (e.g., timestamp, instrument config).
- **Database Mode**: No support yet for streaming to or from a database backend.

---

### 5. **Planned / To-Do Next**

#### 1. **Analysis Tools**

- FFT for each sweep
    
- Peak finding per channel
    
- Time-domain statistical summaries
    

#### 2. **Metadata Management**

- Add session tracking (date, instrument, user)
    
- Auto-tag datasets based on filename or metadata files
    

#### 3. **Database Integration (Future)**

- Create plug-in system for DB modes (PostgreSQL, InfluxDB)
    
- Insert experiment logs into metadata tables
    

#### 4. **GUI or CLI Enhancements**

- Optional GUI for drag-drop file analysis
    
- More robust CLI flags for output formats and filters
    

#### 5. **Testing and Packaging**

- Write unittests for each module
    
- Set up `setup.py` or Poetry for packaging
    
- Add version control metadata (`__version__` etc.)
    

---

### 6. **Dependencies and Environment**

- **Core Libraries**: `numpy`, `pandas`, `matplotlib`, `nptdms`
    
- **Optional / Future**: `scipy`, `seaborn`, `plotly`, `dash`, `sqlalchemy`
    

---

### 7. **Current Data Workflow**

**Single File Mode:**

css

CopyEdit

`[TDMS File] → [file_loader] → [data_utils] → [filtering] → [plotting]`

**Folder Mode:**

scss

CopyEdit

`[Folder of TDMS Files] → [file_loader] (tag each with param) → [data_utils] → [filtering] (grouped) → [plotting]`

---

### 8. **Known Issues or Bugs**

- Group normalization logic may misbehave if param column is missing or inconsistent in folder-based mode.
    
- Current plots assume 3-column shape; edge cases may cause crashes.
    
- No logging or debug output for step-by-step data flow.
    

---

This document serves as a **checkpoint** for the current state of the project. You can upload this to any AI assistant or project management tool to **resume from here** and continue development.

Let me know if you’d like this in Markdown, PDF, or Overleaf format.