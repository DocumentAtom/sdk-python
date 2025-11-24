# Python SDK for Document Atom

<!-- [![PyPI version](https://badge.fury.io/py/sdk-python.svg)](https://badge.fury.io/py/sdk-python)
[![Downloads](https://static.pepy.tech/badge/sdk-python)](https://pepy.tech/project/sdk-python) -->

Document Atom enables organizations to extract structured data (atoms) from various document formats. This SDK provides a simplified interface for consuming Document Atom services in Python applications.

## License

This software is licensed under the [MIT License](https://mit-license.org/). Use of the software requires acceptance of the license terms found in the file `LICENSE.txt`.

## Requirements

- Python 3.8 or higher
- pip package manager

### Dependencies

- `httpx`: For making HTTP requests
- `pydantic[email]`: For data validation and serialization
- `typing`: For type hints

## Installation

Install the SDK using pip:

```bash
pip install sdk-python
```

## Getting Started

### Basic Configuration

```python
import document_atom_sdk
from document_atom_sdk import sdk_logging

# Optional: Enable debug logging
sdk_logging.set_log_level(level="DEBUG")

# Configure the Document Atom SDK
document_atom_sdk.configure(
    endpoint="http://YOUR_SERVER_URL_HERE:PORT",
)
```

## Available Services

The SDK provides access to the following services:

- **Connectivity**: Validate connectivity to the Document Atom API
- **TypeDetection**: Detect the type of files and documents
- **AtomExtraction**: Extract structured data (atoms) from various document formats
  - CSV, Excel, HTML, JSON, Markdown
  - OCR (for images)
  - PDF (with optional OCR support)
  - PNG (image files)
  - PowerPoint (with optional OCR support)
  - RTF (with optional OCR support)
  - Text, Word, XML

### Example Usage

```python
import document_atom_sdk
from document_atom_sdk.exceptions import SdkException

# Configure the SDK
document_atom_sdk.configure(
    endpoint="http://localhost:8000",
)

try:
    # Validate connectivity
    result = document_atom_sdk.Connectivity.validate_connectivity()
    print(f"Connectivity check: {result}")

    # Detect file type
    result = document_atom_sdk.TypeDetection.detect_type("path/to/your/file.pdf")
    print(f"Detected file type: {result}")

    # Extract atoms from PDF with OCR
    result = document_atom_sdk.AtomExtraction.extract_atoms_pdf(
        "path/to/your/file.pdf", ocr=True
    )
    print(f"Extracted atoms: {result}")

    # Extract atoms from Word document
    result = document_atom_sdk.AtomExtraction.extract_atoms_word(
        "path/to/your/file.docx"
    )
    print(f"Extracted atoms: {result}")

    # Extract atoms from Excel
    result = document_atom_sdk.AtomExtraction.extract_atoms_excel(
        "path/to/your/file.xlsx"
    )
    print(f"Extracted atoms: {result}")

    # Extract atoms using generic method
    result = document_atom_sdk.AtomExtraction.extract_atoms(
        "path/to/your/file.pdf", format_type="pdf", ocr=True
    )
    print(f"Extracted atoms: {result}")

except SdkException as e:
    print(f"Error: {e}")
```

### Supported File Formats

The SDK supports atom extraction from the following formats:

- **CSV**: `extract_atoms_csv()`
- **Excel**: `extract_atoms_excel()`
- **HTML**: `extract_atoms_html()`
- **JSON**: `extract_atoms_json()`
- **Markdown**: `extract_atoms_markdown()`
- **OCR**: `extract_atoms_ocr()` (for images)
- **PDF**: `extract_atoms_pdf()` (with optional OCR support)
- **PNG**: `extract_atoms_png()`
- **PowerPoint**: `extract_atoms_powerpoint()` (with optional OCR support)
- **RTF**: `extract_atoms_rtf()` (with optional OCR support)
- **Text**: `extract_atoms_text()`
- **Word**: `extract_atoms_word()`
- **XML**: `extract_atoms_xml()`

### Input Methods

The SDK supports multiple ways to provide file input:

```python
import io
from pathlib import Path

# Method 1: File path (string)
result = document_atom_sdk.AtomExtraction.extract_atoms_pdf(
    "path/to/your/file.pdf", ocr=True
)

# Method 2: Absolute path using Path
file_path = Path(__file__).parent / "sample.pdf"
result = document_atom_sdk.AtomExtraction.extract_atoms_pdf(
    str(file_path), ocr=True
)

# Method 3: Bytes
with open("path/to/your/file.pdf", "rb") as f:
    file_bytes = f.read()

result = document_atom_sdk.AtomExtraction.extract_atoms_pdf(
    file_bytes, filename="file.pdf", ocr=True
)

# Method 4: BytesIO
with open("path/to/your/file.pdf", "rb") as f:
    file_data = io.BytesIO(f.read())
    file_data.name = "file.pdf"

result = document_atom_sdk.AtomExtraction.extract_atoms_pdf(file_data, ocr=True)

# Method 5: BinaryIO (file handle)
with open("path/to/your/file.pdf", "rb") as file_handle:
    result = document_atom_sdk.AtomExtraction.extract_atoms_pdf(
        file_handle, ocr=True
    )
```

## Error Handling

The SDK provides comprehensive error handling with specific exception types:

```python
try:
    # Your SDK operations here
    result = document_atom_sdk.AtomExtraction.extract_atoms_pdf("file.pdf")
except document_atom_sdk.exceptions.AuthenticationError as e:
    print(f"Authentication failed: {e}")
except document_atom_sdk.exceptions.AuthorizationError as e:
    print(f"Authorization failed: {e}")
except document_atom_sdk.exceptions.ResourceNotFoundError as e:
    print(f"Resource not found: {e}")
except document_atom_sdk.exceptions.BadRequestError as e:
    print(f"Bad request: {e}")
except document_atom_sdk.exceptions.TimeoutError as e:
    print(f"Request timed out: {e}")
except document_atom_sdk.exceptions.ServerError as e:
    print(f"Server error: {e}")
except document_atom_sdk.exceptions.SdkException as e:
    print(f"General SDK error: {e}")
```

## Logging

The SDK provides flexible logging capabilities:

```python
from document_atom_sdk import sdk_logging

# Set console logging level
sdk_logging.set_log_level("DEBUG")  # Options: DEBUG, INFO, WARNING, ERROR, CRITICAL

# Add file logging
sdk_logging.add_file_logging("document_atom_sdk.log", level="INFO")
```

## Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

## Support

For support, please:
1. Check the [documentation](docs/)
2. Open an issue on GitHub
3. Contact Document Atom support

## Development

### Setting up Pre-commit Hooks

This project uses pre-commit hooks to ensure code quality. To set up pre-commit:

```bash
# Install pre-commit
pip install pre-commit

# Install the pre-commit hooks
pre-commit install

# (Optional) Run pre-commit on all files
pre-commit run --all-files
```

The pre-commit hooks will run automatically on `git commit`. They help maintain:

- Code formatting (using ruff)
- Import sorting
- Code quality checks
- And other project-specific checks

### Running Tests

The project uses `tox` for running tests in isolated environments. Make sure you have tox installed:

```bash
pip install tox
```

To run the default test environment:

```bash
tox
```

To run specific test environments:

```bash
# Run only the tests
tox -e default

# Run tests with coverage report
tox -- --cov document_atom_sdk --cov-report term-missing

# Build documentation
tox -e docs

# Build the package
tox -e build

# Clean build artifacts
tox -e clean
```

### Development Installation

For development, you can install the package with all test dependencies:

```bash
# Install with testing dependencies
pip install -e ".[testing]"
```

### Viewing Documentation

To build and view the SDK documentation locally:

```bash
# Build the documentation
tox -e docs

# Navigate to the built documentation
cd docs/_build/html

# Start a local server
python -m http.server
```

Then open your web browser and visit `http://localhost:8000` to view the documentation.

### Publishing

The project uses `tox` to automate the publishing process. To publish a new version:

1. Update version in `setup.cfg`
2. Run the publishing environments:

```bash
# Run tests and build checks before publishing
tox -e test,build

# Build and publish to PyPI
tox -e publish

# Build and publish to Test PyPI
tox -e publish-test
```

The publish environments handle:
- Running all tests
- Building source and wheel distributions
- Checking package metadata and README
- Uploading to PyPI/Test PyPI

Make sure you have the following environment variables set for authentication:
```bash
# For PyPI
export TWINE_USERNAME=__token__
export TWINE_PASSWORD=your_pypi_token

# For Test PyPI
export TWINE_TEST_USERNAME=__token__
export TWINE_TEST_PASSWORD=your_testpypi_token
```

For more information about publishing Python packages, see the [Python Packaging User Guide](https://packaging.python.org/tutorials/packaging-projects/).

## Feedback and Issues

Have feedback or found an issue? Please file an issue in our GitHub repository.

## Documentation

For detailed API documentation and examples, visit the [Document Atom Documentation](https://docs.documentatom.io).

## Version History

Please refer to [CHANGELOG.md](CHANGELOG.md) for a detailed version history.
