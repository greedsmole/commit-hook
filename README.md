# Commit Hook: Automatic Ticket Prefixer

This commit hook script automatically prefixes your commit messages with a ticket identifier extracted from your current Git branch name. If your branch name follows a specific ticket format (e.g., `feature/ABC-123` or simply `ABC-123`), the script extracts the ticket number and prepends it to your commit message in square brackets.

## How It Works

1. **Extract Branch Name**  
   The script retrieves the current branch name using:
   ```bash
   git symbolic-ref --short HEAD
   ```

2. **Match Ticket Identifier**  
   A regular expression is used to capture the ticket from the branch name:
   ```bash
   TICKET_PATTERN='^(.*\/)?([A-Za-z]+-[0-9]+)'
   ```
   - This pattern optionally matches any prefix ending with a slash.
   - It captures the ticket identifier (e.g., `ABC-123`) in the second capture group.

3. **Format the Ticket**  
   If a valid ticket is found, it is converted to uppercase:
   ```bash
   TICKET_NAME=$(echo "$TICKET_NAME" | tr '[:lower:]' '[:upper:]')
   ```

4. **Prepend to Commit Message**  
   The ticket identifier is prepended in square brackets to the first line of the commit message using:
   ```bash
   sed -i.bak -e "1s/^/[$TICKET_NAME] /" $COMMIT_MSG_FILE
   ```

## Installation

1. **Save the Script**  
   Place the script in your repository’s `.git/hooks` directory with the filename `commit-msg`.

2. **Make It Executable**  
   Grant execution permissions by running:
   ```bash
   chmod +x .git/hooks/commit-msg
   ```

## Customization

- **Regex Pattern:**  
  Adjust the `TICKET_PATTERN` variable if your branch naming convention is different.

- **Prefix Format:**  
  By default, the ticket is wrapped in square brackets. Modify the `sed` command if you prefer a different format.

## Example

- **Branch Name:** `feature/XYZ-456`  
- **Original Commit Message:**
  ```
  Fix issue with user login
  ```
- **Modified Commit Message:**
  ```
  [XYZ-456] Fix issue with user login
  ```

## Limitations

- The hook only prefixes the commit message if a valid ticket identifier is detected in the branch name.
- If no ticket is found, the commit message remains unchanged.

---

This commit hook helps maintain consistency by automatically linking commit messages to their corresponding tickets, making it easier to track changes related to specific tasks.
