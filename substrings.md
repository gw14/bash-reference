In Bash, substring manipulation is commonly done using parameter expansion, cut, awk, or other tools. Below is a concise guide to the most common Bash substring commands and techniques, with examples. These are useful for extracting or manipulating parts of strings, which is often needed in DevOps scripting.
1. Parameter Expansion (Built-in Bash)
Bash provides substring extraction using ${variable:offset:length} syntax.

    Syntax: ${variable:offset:length}
        offset: Starting position (0-based index).
        length: Number of characters to extract (optional; if omitted, extracts until end).
        Negative offset counts from the end of the string.
    Examples:
    bash

    str="Hello, DevOps World!"

    # Extract substring from position 7, length 6
    echo ${str:7:6}  # Output: DevOps

    # Extract from position 7 to end
    echo ${str:7}  # Output: DevOps World!

    # Extract last 6 characters (negative offset)
    echo ${str: -6}  # Output: World!

    # Skip first 2 characters
    echo ${str:2}  # Output: llo, DevOps World!

    Notes:
        Spaces in the string count as characters.
        Negative offsets require a space before the minus sign to avoid confusion with other syntax.

2. Remove Substring (Prefix/Suffix)
Use ${variable#pattern} or ${variable%pattern} to remove parts of a string.

    Shortest match:
        ${variable#pattern}: Remove shortest match of pattern from the start.
        ${variable%pattern}: Remove shortest match of pattern from the end.
    Longest match:
        ${variable##pattern}: Remove longest match of pattern from the start.
        ${variable%%pattern}: Remove longest match of pattern from the end.
    Examples:
    bash

    str="prefix-middle-suffix"

    # Remove prefix (shortest match)
    echo ${str#prefix-}  # Output: middle-suffix

    # Remove suffix (shortest match)
    echo ${str%-suffix}  # Output: prefix-middle

    # Remove longest match from start
    echo ${str##*middle-}  # Output: suffix

    # Remove longest match from end
    echo ${str%%-middle*}  # Output: prefix

3. Using cut Command
The cut command extracts sections of text based on delimiters or character positions.

    Syntax:
        cut -c range: Extract characters by position.
        cut -d delimiter -f field: Extract fields by delimiter.
    Examples:
    bash

    str="Hello,DevOps,World"

    # Extract characters 1-5
    echo $str | cut -c 1-5  # Output: Hello

    # Extract second field with comma delimiter
    echo $str | cut -d',' -f2  # Output: DevOps

    # Extract characters 7-12
    echo $str | cut -c 7-12  # Output: DevOps

4. Using awk Command
awk is powerful for text processing and can extract substrings by fields or patterns.

    Syntax:
        Use substr(string, start, length) in awk.
        Split by delimiters to extract fields.
    Examples:
    bash

    str="Hello,DevOps,World"

    # Extract substring from position 7, length 6
    echo $str | awk '{print substr($0, 7, 6)}'  # Output: DevOps

    # Extract second field with comma delimiter
    echo $str | awk -F',' '{print $2}'  # Output: DevOps

5. Using expr substr (Legacy)
The expr command can extract substrings but is less common in modern Bash.

    Syntax: expr substr string start length
    Example:
    bash

    str="Hello, DevOps World!"
    expr substr "$str" 8 6  # Output: DevOps

    Note: expr is slower and less flexible; prefer parameter expansion or cut.

6. Replace Substring
Use ${variable/pattern/replacement} to replace parts of a string.

    Syntax:
        ${variable/pattern/replacement}: Replace first occurrence.
        ${variable//pattern/replacement}: Replace all occurrences.
    Example:
    bash

    str="Hello, DevOps World!"

    # Replace "DevOps" with "SRE"
    echo ${str/DevOps/SRE}  # Output: Hello, SRE World!

    # Replace all spaces with underscores
    echo ${str// /_}  # Output: Hello,_DevOps_World!

7. Extract with Regular Expressions (Regex)
Use =~ operator or grep/sed for regex-based substring extraction.

    Example with =~:
    bash

    str="version: 1.23.4"
    [[ $str =~ [0-9]+\.[0-9]+\.[0-9]+ ]]
    echo ${BASH_REMATCH[0]}  # Output: 1.23.4

    Example with sed:
    bash

    echo "Hello, DevOps World!" | sed 's/.*\(DevOps\).*/\1/'  # Output: DevOps

Quick Tips for DevOps

    Use in Scripts: Parameter expansion is fastest for simple substring tasks in CI/CD pipelines or automation scripts.
    Combine Tools: Chain cut, awk, or sed for complex parsing (e.g., log file processing).
    Test Carefully: Always test substring operations with edge cases (empty strings, missing delimiters).

Example Script for DevOps
bash

#!/bin/bash
log="2025-05-08 10:15:32 [INFO] Deployment successful for app-v1.2.3"

# Extract date
date=${log:0:10}  # Output: 2025-05-08

# Extract version using regex
[[ $log =~ app-v([0-9]+\.[0-9]+\.[0-9]+) ]]
version=${BASH_REMATCH[1]}  # Output: 1.2.3

# Extract message with cut
message=$(echo $log | cut -d'[' -f3 | cut -d']' -f2)  # Output: Deployment successful for app-v1.2.3

echo "Date: $date, Version: $version, Message: $message"

Notes

    Parameter expansion is preferred for performance and simplicity.
    Use cut or awk for delimited data (e.g., CSV-like logs).
    Regex with =~ or sed is great for pattern matching.
    Ensure proper quoting to handle spaces or special characters.

If you have a specific substring task (e.g., parsing logs, extracting versions), share it, and I can provide a tailored example. Want more examples or help with a particular use case?
