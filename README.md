# File Format Converter - Production Implementation

## Author

**Dhiraj Chauhan**  
Data Engineer

## Overview

Production-ready modular implementation for converting retail database CSV files to JSON format. This project provides reusable, well-structured code for both single-dataset and batch processing scenarios.

## Project Structure

```
.
├── Modularize File Format Converter for Dataset.ipynb  # Single dataset processor
├── Wrapper to Process all Data Sets.ipynb              # Batch processor
├── data/
│   ├── retail_db/              # Source CSV files
│   │   ├── schemas.json
│   │   ├── departments/
│   │   ├── categories/
│   │   ├── products/
│   │   ├── customers/
│   │   ├── orders/
│   │   └── order_items/
│   └── retail_db_json/         # Output JSON files
└── README.md
```

## Features

✅ **Schema-driven processing** - Automatic column detection  
✅ **Modular design** - Reusable functions  
✅ **Flexible processing** - Single or batch mode  
✅ **JSONL output** - Industry-standard format  
✅ **Error handling** - Safe directory creation  

## Notebooks

### 1. Modularize File Format Converter for Dataset.ipynb
Process a single dataset with modular functions.

**Functions:**

```python
get_columns_names(schemas, ds_name, sorting_key='column_position')
# Extracts sorted column names from schema

read_csv(file, schemas)
# Reads CSV with proper column names, auto-detects dataset name

convert_to_json(df, tgt_base_dir, ds_name, file_name)
# Converts DataFrame to JSONL format

file_converter(ds_name)
# Main function - processes all files for a dataset
```

**Usage:**
```python
ds_name = 'orders'
file_converter(ds_name)
```

---

### 2. Wrapper to Process all Data Sets.ipynb
Batch process all datasets automatically.

**Functions:**

```python
get_columns_name(schemas, ds_name, sorting_key='column_position')
# Schema column extraction

read_csv(file, schemas)
# CSV reading with schema application

to_json(df, tgt_base_dir, ds_name, file_name)
# JSON conversion and writing

file_converter(src_base_dir, tgt_base_dir, ds_name)
# Single dataset processing with configurable paths

process_files(ds_names=None)
# Main entry point - processes all or specified datasets
```

**Usage:**
```python
# Process all datasets
process_files()

# Process specific datasets
process_files(['orders', 'customers'])
```

## Installation

```bash
pip install pandas
```

## Quick Start

### Process All Datasets
```python
# Open: Wrapper to Process all Data Sets.ipynb
# Run all cells or execute:
process_files()
```

### Process Single Dataset
```python
# Open: Modularize File Format Converter for Dataset.ipynb
# Set dataset name and run:
ds_name = 'products'
file_converter(ds_name)
```

## Output Format

**JSONL (JSON Lines)** - One JSON object per line:
```json
{"order_id":1,"order_date":"2013-07-25 00:00:00.0","order_customer_id":11599,"order_status":"CLOSED"}
{"order_id":2,"order_date":"2013-07-25 00:00:00.0","order_customer_id":256,"order_status":"PENDING_PAYMENT"}
```

**Why JSONL?**
- Stream-processable for large files
- Compatible with Spark, Hadoop, and other big data tools
- Easy to append new records
- Line-by-line processing without loading entire file

## Design Patterns

### 1. Schema-Driven Processing
```python
# Column info externalized in schemas.json
columns = get_columns_names(schemas, ds_name)
```
Makes code dataset-agnostic and easy to extend.

### 2. Path-Based Dataset Detection
```python
file_path_list = Path(file).parts
ds_name = file_path_list[-2]
```
Eliminates hardcoded dataset names.

### 3. Safe Directory Creation
```python
os.makedirs(f'{tgt_base_dir}/{ds_name}', exist_ok=True)
```
Prevents errors if directories already exist.

## Technologies

- **Python 3.14.0**
- **pandas** - Data manipulation
- **pathlib** - Path operations
- **glob** - File discovery
- **json** - Schema parsing
- **os** - File system operations

## Configuration

Modify these variables in the notebooks:

```python
src_base_dir = 'data/retail_db'        # Source directory
tgt_base_dir = 'data/retail_db_json'   # Output directory
```

## Datasets Supported

- departments (6 records)
- categories (58 records)
- products (1,345 records)
- customers (12,435 records)
- orders (68,883 records)
- order_items (172,198 records)


## Related Projects

This is the production implementation. For the exploratory analysis and development journey, see the separate exploratory project.

---

**Note:** This production code is refactored from exploratory notebooks into modular, reusable functions suitable for production use.