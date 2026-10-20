# HTML Pretty Printer (Bash CLI)

## The Theme – CLI System Utilities
Parsing and formatting structured text is a fundamental task in software engineering. I chose to build this command-line utility to demonstrate how complex string manipulation and data streams can be handled efficiently using only native POSIX tools, without relying on heavy external libraries or full DOM parsers.

## About the project
This is a lightweight command-line utility written in Bash that takes minified or chaotically formatted HTML files and reconstructs their visual structure. It parses the raw text and outputs a clean, readable version of the code with standard 4-space indentation, making the DOM tree hierarchy easy to follow for debugging and code maintenance.

## Core Architecture & Implementation
* **Stream Processing:** Unlike traditional web parsers that load the entire document into RAM to build an abstract syntax tree, this script processes the input sequentially. This approach keeps the memory footprint extremely low and constant, regardless of the input file size.
* **Custom Tokenization:** The script utilizes PCRE (Perl Compatible Regular Expressions) via `grep -oP` to break down dense, single-line minified HTML into atomic tokens (tags and text content). It then uses `sed` for precise whitespace trimming.
* **Depth Tracking Logic:** An internal state variable acts as a depth counter for the DOM tree, dynamically incrementing on opening tags and decrementing on closing tags to calculate the exact padding needed.
* **Void Elements Handling:** A critical feature of the algorithm is the explicit handling of "self-closing" tags (e.g., `<img>`, `<br>`, `<input>`). The parser cross-references extracted tags against a predefined list of void elements to prevent infinite indentation drift.

## Performance
Tested on a WSL2 (Ubuntu) environment, the utility proves highly efficient. Relying entirely on native OS-level string manipulation, it maintains a minimal memory footprint (~18-22 MB) and can successfully format a dense, complex 500 KB minified HTML file in approximately 3.2 seconds.

## Quick Usage
```bash
# Grant execution rights
chmod +x pretty.sh

# Run the script (output file is optional; defaults to terminal stdout)
./pretty.sh input.html output.html
