# AISudokuSolver: Computer Vision-Based (CNN) Sudoku Solver  and LLM Trace Analysis

![Python](https://img.shields.io/badge/Python-3.11-blue.svg)
![AI](https://img.shields.io/badge/AI-CNN_%26_LLM-blueviolet)
![Task](https://img.shields.io/badge/Task-Computer_Vision-orange)
![Coverage](https://img.shields.io/badge/Coverage-74%25-brightgreen)
![Last Updated](https://img.shields.io/badge/Last%20Updated-March%202026-brightgreen)

A pipeline to solve Sudoku puzzles from images, using computer vision for digit extraction, a backtracking algorithm for solving, and LLMs for analytical summaries.  

## Table of Contents

- [Introduction](#introduction)
- [Documentation](#documentation)
- [Key Features](#key-features)
- [Installation](#installation)
- [Usage](#usage)
- [Important Notes](#important-notes)
- [Final Words](#final-words)

## Introduction

AISudokuSolver is a fully autonomous pipeline that solves Sudoku puzzles from images, integrating computer vision, deep learning, and symbolic reasoning.

The system extracts and interprets the puzzle board from a photo or scan using a custom-trained Convolutional Neural Network (CNN) for digit recognition. The board is then solved using an optimized backtracking algorithm enhanced with constraint propagation and variable ordering heuristics.

In addition to solving the puzzle, AISudokuSolver records a detailed trace of the resolution process and generates a structured Markdown report enriched with a natural language summary powered by a Large Language Model (LLM). This summary interprets the solving strategy, highlights key decisions, and estimates puzzle complexity.

The project is designed as a robust, testable, and educational framework that combines traditional algorithmic techniques with modern AI capabilities. It is ideal for exploring computer vision pipelines, algorithm design, and LLM-based analytics in a real-world use case.

## Documentation

Additional technical documentation is available in the `/docs` directory for readers who want to explore the internal design and machine learning components of the project in more detail.

* **[Architecture Overview](docs/ARCHITECTURE.md)**
  Describes the internal structure of the system, module responsibilities, and the overall pipeline organization.

* **[Model Architecture](docs/model_architecture.md)**
  Explains the computer vision pipeline, dataset structure, CNN architecture, training configuration, and evaluation results.

These documents provide a deeper technical perspective on how AISudokuSolver is built and how its AI components operate internally.

## Key Features

- **End-to-End Sudoku Solving Pipeline**: Processes input images, extracts puzzles, solves them, and generates full analytical reports.
- **CNN-Based Digit Recognition**: Classifies digits (1–9) and empty cells using a trained Convolutional Neural Network.
- **Optimized Backtracking Solver**: Uses constraint propagation and MRV heuristic for efficient solving.
- **Trace Generation**: Records every solving step to produce a detailed and reproducible trace.
- **LLM-Powered Summary**: Generates a natural language explanation of the solving strategy using GPT-based models.
- **Markdown Reporting**: Produces structured reports with boards, trace, metrics, and LLM insights.
- **Modular and Maintainable Architecture**: Decoupled components for vision, logic, and reporting, each independently testable.
- **FastAPI Server Support**: Exposes the solver as a local REST API for integration into other systems or workflows.

## Installation

1. Clone this repository:
```bash
git clone https://github.com/YOUR_USERNAME/ai-sudoku-solver.git
cd ai-sudoku-solver
```

2. (Optional but recommended) Create and activate a virtual environment:
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

3. Install required Python packages:
```bash
pip install -r requirements.txt
```

4. Add your OpenAI key on the .env file:
```bash
OPENAI_API_KEY=your_openai_api_key_here
```

## Usage

You can use **AISudokuSolver** in two main ways, depending on whether you want an interactive CLI experience or access it programmatically via a local REST API.

### Option 1: Run via CLI (recommended for individual use)

Execute the full solving pipeline by running the main script:

```bash
python aisudokusolver.py
```

You will be prompted to select a Sudoku image (JPG or PNG). The pipeline will:

1. Parse and segment the board from the image. 
2. Use a CNN to classify each cell. 
3. Solve the puzzle using backtracking with MRV & forward checking. 
4. Save the outputs to your Downloads/AISudokuSolver/ folder, including:
   - Input image
   - Markdown report (MD)
   - Solving trace (JSON)
   - Console log (LOG)

### Option 2: Run the FastAPI server locally

Expose the functionality via a local REST API by launching the FastAPI app:
```bash
uvicorn app:app --reload
```

Then open the interactive documentation at:
```bash
http://127.0.0.1:8000/docs
```

You can test the /solve endpoint directly from the Swagger UI, or use curl:
```bash
curl -X 'POST' \
  'http://127.0.0.1:8000/solve' \
  -F 'image=@inputs/easy.jpg' \
  -H 'accept: application/json' \
  -H 'Content-Type: multipart/form-data'
```

A successful response will look like:
```bash
{
  "parsed_board": [[5,3,0,...]],
  "solved_board": [[5,3,4,...]],
  "steps": 34,
  "duration": 0.92
}
```

The complete output files will be saved in your Downloads/AISudokuSolver/ folder.

## Important Notes

- The CNN model for digit recognition was trained from scratch using manually labeled Sudoku cell images. You can retrain or improve this model by updating the dataset in `/datasets/train`, `/datasets/val`, and `/datasets/test`.
- Input images must clearly show a front-facing, well-lit Sudoku grid. While the system includes preprocessing steps like resizing, binarization, and grid orientation correction, highly skewed or low-contrast images may lead to errors in segmentation or digit classification.
- The solving algorithm combines an optimized backtracking approach with **Minimum Remaining Value (MRV)** heuristics and **forward checking** for constraint propagation. Only the final placements are included in the solving trace, not the full decision tree.
- To generate the optional LLM-based summary, an OpenAI API key must be provided via a `.env` file:
   ```bash
   OPENAI_API_KEY=your_openai_api_key
   ```
- All reports, traces, and log files are saved under the ~/Downloads/AISudokuSolver/ folder. Each output set is named based on the original input image filename and includes:
  - A .md Markdown report
  - A .json solving trace
  - A .log console output snapshot
- The FastAPI server is currently intended for local usage only. A public deployment for easier external access is planned as a future improvement. This could also include the development of a user-friendly graphical interface.

> This modular pipeline enables easy retraining, debugging, and extension of both the vision and logic components.

## Contributing & Contact

This project originated from my personal passion for Sudoku puzzles and the challenge of building a system capable of solving them end-to-end — from visual recognition to logical resolution.

It has been designed as a fully autonomous pipeline that blends classic algorithmic techniques with modern AI models.The result is a practical system that combines computer vision, deep learning, and LLM-based interpretation — all within a structured, testable, and reproducible framework.

Feel free to explore, extend, or integrate it into your own applications. Contributions, feedback, or improvements are always welcome.

**If you’ve found this project useful or inspiring — feel free to build on it, break it, or just drop a star 🌟.**

- Bugs / feature requests: please open an **Issue**.
- Direct contact: [inigo.rodsan@gmail.com](mailto:inigo.rodsan@gmail.com)

Developed & maintained by [Íñigo Rodríguez](https://github.com/irdsn).