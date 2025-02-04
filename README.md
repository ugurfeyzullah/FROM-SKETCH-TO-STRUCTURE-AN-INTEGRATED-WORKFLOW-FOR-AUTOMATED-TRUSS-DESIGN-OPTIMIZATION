# From Sketch to Structure: An Integrated Workflow for Automated Truss Design Optimization Through BIM Tools
![Workspace](https://github.com/user-attachments/assets/8a6e58ed-76aa-480a-959a-6b8a9340f823)



## Table of Contents
- [Abstract](#abstract)
- [Introduction](#introduction)
- [Methodology](#methodology)
  - [Tools and Environment](#tools-and-environment)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Feature Extraction](#feature-extraction)
  - [Path Analysis and Line Extraction](#path-analysis-and-line-extraction)
  - [Data Structuring and Dynamo Conversion](#data-structuring-and-dynamo-conversion)
  - [Structural Analysis and Optimization](#structural-analysis-and-optimization)
  - [RSA Integration](#rsa-integration)
- [Usage](#usage)
- [Contributing](#contributing)
- [Citation](#citation)
- [Contact](#contact)
- [License](#license)

---

## Abstract

Although digital technologies have greatly enhanced structural designers' capabilities for optimal designs, reliance on manual workflows still poses obstacles when converting models across design, analysis, and optimization tools. This fragmented approach leads to time inefficiencies, compatibility errors, and limited design explorations. The proposed methodology presents an integrated BIM-based workflow for converting hand-sketched designs into optimized models. The process unfolds as follows:

1. **Geometry Extraction:** Combining different techniques to extract structural features.
2. **Interactive Modification:** Using natural language and command processing to adjust the design.
3. **Metaheuristic Optimization:** Employing AI-based algorithms for sizing, shape, and topology optimization.
4. **Model Validation:** Converting optimized sketches into validated structural models using Robot Structural Analysis (RSA).

Results on test problems demonstrate significant time savings, improved design outcomes, and a more efficient design-to-analysis workflow.

---

## Introduction

Modern structural design typically involves:

- **Manual Modeling:** Creating structural models in CAD software.
- **Separated Optimization:** Utilizing distinct optimization tools for sizing, shape, and topology.
- **Finite Element Analysis (FEA):** Validating models with FEM software like RSA.
- **Iterative Refinement:** Repeatedly modifying models across different platforms.

Recent trends point toward integrating these processes through digital technologies. However, disconnected tools and manual data transfers often lead to errors and inefficiencies. This study addresses these challenges by integrating image processing, BIM tools (e.g., Dynamo), and AI-based metaheuristic algorithms to transform hand-sketched designs into optimized digital models. The workflow enables automated scaling, modification, and optimization, effectively bridging the gap between conceptual design and professional analysis.

---

## Methodology

The study approaches the problem using two methods:

1. **Unassisted Sketches (Group 1):** Artistic sketches without pre-specified node markers, where algorithms infer connection points.
2. **Node-Specified Sketches (Group 2):** Sketches with marked nodes to simplify feature extraction.

### Tools and Environment

- **Dynamo (v3.0.4):** For visual programming within BIM.
- **Python (v3.9.12):** With libraries such as PyNiteFEA, PyMOO, scikit-image, OpenCV, SciPy, scikit-learn, Matplotlib, and NumPy.
- **Robot Structural Analysis (RSA):** For final structural validation.

### Prerequisites

Before running the codes in this repository, ensure that your system and development environment meet the following requirements:

#### Software Requirements
- **Autodesk Revit** with **Dynamo for Revit**  
  *Required for visual programming, BIM integration, and model modification.*
- **Robot Structural Analysis Professional (RSA)**  
  *Needed for advanced structural analysis and FEM validation. Ensure that the RSA API and the `Interop.RobotOM.dll` are installed and accessible.*
- **Python 3.9.12** (or a compatible version)  
  *The codes have been developed and tested using this version (an embedded distribution is used in some cases).*

#### Python Libraries  
Install the following packages (see the provided `requirements.txt`):

- **Pillow (PIL)** – For image processing.
- **OpenCV** – For computer vision operations.
- **NumPy** – For numerical computations.
- **SciPy** – For scientific computations (e.g., filtering, convolution).
- **scikit-image** – For image processing tasks including skeletonization and morphological operations.
- **scikit-learn** – For clustering algorithms (e.g., DBSCAN, HDBSCAN).
- **Matplotlib** – For plotting and visualization.
- **PyQt6** – For creating graphical user interfaces (GUIs).
- **ultralytics (YOLO)** – For deep learning–based node detection.
- **OpenAI Python Library** – To interface with OpenAI APIs (ChatGPT, Whisper, etc.).
- **speech_recognition**, **sounddevice**, **soundfile** – For audio recording and speech transcription.
- **pymoo** – For metaheuristic optimization algorithms (Differential Evolution, Genetic Algorithms, etc.).
- **PyNiteFEA** – For finite element analysis.
- **pythonnet** – For Dynamo's ProtoGeometry and .NET interoperability.

#### Additional Setup
- **Site-Packages Path Configuration:**  
  The scripts automatically append the local Python `site-packages` directory to `sys.path` to ensure all modules are accessible.
- **API Keys:**  
  Replace placeholder API keys (e.g., for OpenAI) in the code with your actual keys.
- **DLL and File Paths:**  
  Verify that the path to `Interop.RobotOM.dll` and other file paths used in the scripts are correct and accessible on your system.
- **Graphical Environment:**  
  A GUI is required for PyQt6 and Matplotlib; the code may not run in headless environments.

### Installation

1. **Clone the repository:**
    ```bash
    git clone https://github.com/ugurfeyzullah/Reliability-Constrained-Structural-Design-Optimization-Using-BIM-Based-Tools.git
    ```
2. **Install necessary dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
3. **Run the optimization script on Dynamo.**

### Feature Extraction

Four methods are implemented to detect nodes (connection points) in hand-sketched images:

1. **Harris Corner Detection:**  
   - Uses OpenCV's `cv2.goodFeaturesToTrack` to detect corners.
   - Clusters nearby points using DBSCAN to determine nodes.

2. **Skeletonization Method:**  
   - Converts images to a one-pixel-wide skeleton.
   - Analyzes local connectivity to identify branch points, which are then merged via DBSCAN.

3. **Heat Map Detection:**  
   - Creates a pixel density map with Gaussian smoothing.
   - Detects local maxima with the `peak_local_max` algorithm from the skimage library.

4. **Machine Learning Technique:**  
   - Utilizes a YOLO-based deep learning model trained on synthetic images.
   - Detects nodes and key structural features directly from the sketches.

### Path Analysis and Line Extraction

- **Intersection Identification:**  
  After node detection, the algorithm identifies intersections between skeleton lines and node clusters.
- **Path Tracing:**  
  Continuous paths between clusters are traced to create a skeletal representation of the truss, defining structural elements.

### Data Structuring and Dynamo Conversion

- **Scaling:**  
  The workflow calculates a scale factor by detecting dimension lines in the sketch (or via manual input through a GUI).
- **Geometry Conversion:**  
  Scaled coordinates are converted into Dynamo-native objects for further parametric modeling.
- **Interactive Modification:**  
  Users can modify the structure via text or voice commands (leveraging OpenAI’s Whisper and ChatGPT APIs), which update node positions and connectivity in real time.

### Structural Analysis and Optimization

- **FEM Modeling:**  
  A finite element model is built using PyNiteFEA, where members, supports, loads, and material properties are defined.
- **Optimization Process:**  
  Metaheuristic algorithms (e.g., Differential Evolution or Genetic Algorithms via PyMOO) optimize both sizing and shape variables, minimizing structural weight while satisfying displacement and stress constraints.

### RSA Integration

- **Automated Export:**  
  The optimized model is exported to Autodesk Robot Structural Analysis (RSA) via its API.
- **Model Reconstruction:**  
  RSA automatically creates nodes and elements, assigns section properties from a standard database, and performs FEA.
- **Validation:**  
  RSA runs a detailed FEM analysis, and results are saved for further inspection.

---

## Usage

1. **Input Hand-Sketched Image**  
   - Provide a hand-drawn truss sketch (with or without explicit node markers) as the input image.
   - Supported formats: PNG, JPEG, etc.

2. **Image Preprocessing and Feature Extraction**  
   - The workflow preprocesses the image using Gaussian/median filtering, contrast enhancement, and binary thresholding.
   - Choose a node detection method:
     - *Harris Corner Detection*
     - *Skeletonization Method*
     - *Heat Map Detection*
     - *Machine Learning Approach (YOLO)*

3. **Path and Geometry Reconstruction**  
   - The system identifies intersections between skeleton lines and extracted node clusters.
   - It traces continuous paths to create a skeletal representation of the truss, defining structural elements.

4. **Scaling and Data Conversion**  
   - Automatically detect dimension lines (or use manual input via the GUI) to calculate a scale factor.
   - Convert pixel measurements into real-world units.
   - Transform scaled coordinates into Dynamo-native objects for further modeling.

5. **Interactive Model Modification**  
   - Use the integrated text/voice command interface (leveraging OpenAI’s APIs) to modify the structural model.
   - Adjust node positions, add/remove elements, and refine connectivity in real time.

6. **Finite Element Analysis (FEA)**  
   - Build a finite element model using PyNiteFEA by defining members, supports, loads, and material properties.
   - Run FEA to compute stress, displacement, and reliability indicators.

7. **Optimization**  
   - Use metaheuristic algorithms (e.g., Differential Evolution or Genetic Algorithms via PyMOO) to optimize design variables.
   - The optimization adjusts both sizing (member cross-sectional areas) and shape (nodal coordinates) to minimize weight while satisfying structural constraints.

8. **RSA Integration and Validation**  
   - Automatically export the optimized model to Autodesk Robot Structural Analysis (RSA) via the RSA API.
   - RSA reconstructs the model by creating nodes, bars, and assigning section properties from a standard database.
   - Perform detailed FEM analysis in RSA to validate the design.

9. **Result Evaluation and Export**  
   - Visualize the optimization history and final truss design within Dynamo.
   - Evaluate structural performance (stress, displacement, weight).
   - Optionally, export the validated model back into Revit for final adjustments and broader BIM integration.

10. **Batch Processing and Data Handling**  
    - Use custom Python scripts for batch processing of multiple sketches.
    - Automate the retrieval of standard section properties from Excel and manage data for large-scale structural simulations.

---

## Contributing

Contributions are welcome! If you have improvements or bug fixes, please follow these steps:

1. Fork the repository.
2. Create your feature branch (`git checkout -b feature/AmazingFeature`).
3. Commit your changes (`git commit -am 'Add some AmazingFeature'`).
4. Push to the branch (`git push origin feature/AmazingFeature`).
5. Open a pull request.

---

## Citation

**MDPI and ACS Style**  
Yavan, F.; Maalek, R.; Toğan, V. Structural Optimization of Trusses in Building Information Modeling (BIM) Projects Using Visual Programming, Evolutionary Algorithms, and Life Cycle Assessment (LCA) Tools. *Buildings* 2024, 14, 1532. [https://doi.org/10.3390/buildings14061532](https://doi.org/10.3390/buildings14061532)

**AMA Style**  
Yavan F, Maalek R, Toğan V. Structural Optimization of Trusses in Building Information Modeling (BIM) Projects Using Visual Programming, Evolutionary Algorithms, and Life Cycle Assessment (LCA) Tools. *Buildings*. 2024; 14(6):1532. [https://doi.org/10.3390/buildings14061532](https://doi.org/10.3390/buildings14061532)

**Chicago/Turabian Style**  
Yavan, Feyzullah, Reza Maalek, and Vedat Toğan. 2024. "Structural Optimization of Trusses in Building Information Modeling (BIM) Projects Using Visual Programming, Evolutionary Algorithms, and Life Cycle Assessment (LCA) Tools." *Buildings* 14, no. 6: 1532. [https://doi.org/10.3390/buildings14061532](https://doi.org/10.3390/buildings14061532)

**BibTeX:**

@Article{buildings14061532,
  AUTHOR = {Yavan, Feyzullah and Maalek, Reza and Toğan, Vedat},
  TITLE = {Structural Optimization of Trusses in Building Information Modeling (BIM) Projects Using Visual Programming, Evolutionary Algorithms, and Life Cycle Assessment (LCA) Tools},
  JOURNAL = {Buildings},
  VOLUME = {14},
  YEAR = {2024},
  NUMBER = {6},
  ARTICLE-NUMBER = {1532},
  URL = {https://www.mdpi.com/2075-5309/14/6/1532},
  ISSN = {2075-5309},
  DOI = {10.3390/buildings14061532}
}


## Contact

- Feyzullah YAVAN - www.linkedin.com/in/ugurfey - feyzullah.yavan@kit.edu

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE.md) file for details.
