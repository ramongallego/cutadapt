# Plan to Add Map File Output to Cutadapt

## Overview
The goal is to modify Cutadapt to include an option for generating map files instead of FASTA/FASTQ files. This will reduce disk space usage and allow for more flexible workflows, such as chaining multiple adapter-trimming operations or generating FASTA/FASTQ files only when needed.

## Steps

### 1. Understand the Current Codebase
- Identify where Cutadapt processes sequences and writes output files (FASTA/FASTQ).
- Locate the code responsible for adapter detection and trimming.

### 2. Define the Map File Format
- Each line in the map file should include:
  - Sequence ID
  - Adapter found
  - Start position of the adapter
  - End position of the adapter
- Example:
  ```
  seq1    adapter1    5    20
  seq2    adapter2    10   30
  ```

### 3. Add a Command-Line Option
- Introduce a new flag (e.g., `--output-mapfile`) to specify the map file path.
- Ensure this flag is mutually exclusive with the existing FASTA/FASTQ output options.

### 4. Modify the Processing Logic
- When the `--output-mapfile` flag is used:
  - Detect adapters as usual.
  - Instead of writing trimmed sequences to FASTA/FASTQ, write the mapping information to the specified map file.

### 5. Enable Map File as Input
- Add functionality to read a map file as input.
- Use the adapter positions from the map file to guide subsequent operations (e.g., trimming additional adapters).

### 6. Implement Fasta/Fastq Generation from Map File
- Add a utility to generate FASTA/FASTQ files from a map file and the original sequence file.

### 7. Update Documentation
- Document the new `--output-mapfile` option and its usage.
- Provide examples of workflows using map files.

### 8. Write Unit Tests
- Test the new functionality:
  - Generating map files.
  - Using map files as input.
  - Generating FASTA/FASTQ from map files.

### 9. Optimize Performance
- Ensure that generating and using map files is efficient and does not introduce significant overhead.

### 10. Release and Feedback
- Release the updated version of Cutadapt.
- Gather feedback from users to refine the feature.

## Example Workflow

1. **Generate a Map File**:
   ```bash
   cutadapt -a ADAPTER_SEQ --output-mapfile adapters.map input.fastq
   ```

2. **Use the Map File for a Second Trimming Operation**:
   ```bash
   cutadapt -a SECOND_ADAPTER --input-mapfile adapters.map --output-mapfile second_adapters.map input.fastq
   ```

3. **Generate FASTA/FASTQ from a Map File**:
   ```bash
   cutadapt --generate-fastq-from-mapfile adapters.map input.fastq -o output.fastq
   ```

This plan ensures flexibility and reduces unnecessary disk usage while maintaining compatibility with existing workflows.