# OCR Receipts

This project reads JPEG ride receipts with Tesseract OCR, extracts receipt details, and writes them to an Excel workbook. The workflow is in [OCRScript.ipynb](OCRScript.ipynb).

The notebook looks for `.jpg` and `.jpeg` files in a `cab_receipts/` folder (or in the project folder itself). For each image, it attempts to read the date, amount, ride/receipt number, and time. Successfully processed images are moved to `processed_receipts/`, and the results are saved to `processed_receipts/cab_receipts.xlsx`.

> Receipt images and generated spreadsheets can contain personal information. Both `cab_receipts/` and `processed_receipts/` are excluded from Git by `.gitignore`. Keep them out of public repositories and only share them with people who should see the trip details.

## Requirements

- Python 3.9 or newer
- [uv](https://docs.astral.sh/uv/getting-started/installation/) for Python environment and package management
- Tesseract OCR, a separate system application used by the Python `pytesseract` package
- VS Code with the Python and Jupyter extensions, if you want to run the notebook in VS Code

## Fork This Repository

1. Sign in to GitHub and open [PriyanshChaudhary/OCRReceipts](https://github.com/PriyanshChaudhary/OCRReceipts).
2. Select **Fork**, choose your GitHub account, and create the fork.
3. Clone your fork. Replace `YOUR-USERNAME` with your GitHub username:

```bash
git clone https://github.com/YOUR-USERNAME/OCRReceipts.git
cd OCRReceipts
```

If you already cloned the original project, you can instead fork it on GitHub and then change the clone's remote to your fork:

```bash
git remote set-url origin https://github.com/YOUR-USERNAME/OCRReceipts.git
```

## Install Tesseract

Installing Python packages does not install Tesseract itself. `pytesseract` calls the separate Tesseract application.

On macOS with Homebrew:

```bash
brew install tesseract
```

On Debian or Ubuntu:

```bash
sudo apt update
sudo apt install tesseract-ocr
```

On Windows, install Tesseract using a trusted Windows installer, then make sure its installation folder is on `PATH`. The notebook checks common macOS install locations and also checks `PATH`.

## Set Up Python

From the cloned project folder, run:

```bash
uv sync
```

This creates the project virtual environment and installs the dependencies listed in `pyproject.toml` and `uv.lock`.

For a Jupyter kernel in VS Code, install `ipykernel` into the project environment and register it:

```bash
uv add --dev ipykernel
uv run python -m ipykernel install --user --name ocrreceipts --display-name "Python (OCRReceipts)"
```

In VS Code, open the cloned project folder, open `OCRScript.ipynb`, choose **Select Kernel**, and select **Python (OCRReceipts)** (or the project's `.venv` interpreter).

## Process Receipts

1. Create a folder named `cab_receipts` in the project folder.
2. Copy the receipt JPEG images you want to process into `cab_receipts/`.
3. Open `OCRScript.ipynb` and run its cells from top to bottom.
4. Check `processed_receipts/cab_receipts.xlsx` for the results. Successfully read images are moved into `processed_receipts/`; images without a detected date or amount are left in the input folder and reported as skipped.

The notebook uses the current working directory as the project folder. Open VS Code at the cloned repository root before running it so the input and output folders are found in the expected place.

## Troubleshooting

- **Tesseract is not available:** Install the Tesseract application for your operating system. Installing `pytesseract` with `uv` alone is not enough.
- **No receipts were processed:** Confirm the JPEGs are in `cab_receipts/`, Tesseract is installed, and the notebook is running with the project environment.
- **The notebook kernel is missing:** In VS Code, select the project's `.venv` interpreter. If it is not listed, rerun the `ipykernel` setup commands above and reload the notebook.

## Project Files

- `OCRScript.ipynb`: image OCR, receipt field extraction, and Excel export
- `pyproject.toml`: project metadata and Python dependencies
- `uv.lock`: locked dependency versions used by `uv sync`
- `src/ocrreceipts/`: Python package entry point; the current console command is a starter placeholder, so use the notebook for receipt processing
