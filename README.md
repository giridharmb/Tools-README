# Directory Encryption Guide for macOS

This guide explains how to:
1. Create a tar archive of a directory
2. Encrypt it with GPG using a password
3. Decrypt the encrypted archive
4. Extract the original directory

## Prerequisites

1. Install GPG if you haven't already:
```bash
brew install gnupg
```

2. Verify GPG installation:
```bash
gpg --version
```

## Encryption Process

### Step 1: Create a tar archive

```bash
# Syntax: tar -czf output.tar.gz directory_name
tar -czf my_archive.tar.gz /path/to/your/directory

# Example:
tar -czf documents.tar.gz ~/Documents/important_folder
```

Options explained:
- `-c`: Create a new archive
- `-z`: Compress the archive with gzip
- `-f`: Specify the filename

### Step 2: Encrypt the tar archive with GPG

```bash
# Syntax: gpg -c archive_name.tar.gz
gpg -c my_archive.tar.gz

# Example:
gpg -c documents.tar.gz
```

This will:
1. Prompt you for a password
2. Create an encrypted file named `my_archive.tar.gz.gpg`
3. Leave the original tar.gz file intact

After verifying the .gpg file exists, you can remove the original tar.gz:
```bash
rm my_archive.tar.gz
```

## Decryption Process

### Step 1: Decrypt the GPG file

```bash
# Syntax: gpg -d encrypted_file.tar.gz.gpg > decrypted_file.tar.gz
gpg -d my_archive.tar.gz.gpg > my_archive.tar.gz

# Example:
gpg -d documents.tar.gz.gpg > documents.tar.gz
```

This will:
1. Prompt you for the password
2. Create a decrypted tar.gz file

### Step 2: Extract the tar archive

```bash
# Syntax: tar -xzf archive_name.tar.gz
tar -xzf my_archive.tar.gz

# Example:
tar -xzf documents.tar.gz
```

Options explained:
- `-x`: Extract files
- `-z`: Decompress using gzip
- `-f`: Specify the filename

## Complete One-Liners

### Encryption
```bash
# Create tar and encrypt in one command:
tar -czf - /path/to/directory | gpg -c > encrypted_archive.tar.gz.gpg
```

### Decryption
```bash
# Decrypt and extract in one command:
gpg -d encrypted_archive.tar.gz.gpg | tar -xzf -
```

## Tips

1. Password Safety
   - Use a strong password
   - Store your password securely
   - Consider using a password manager

2. Backup
   - Always keep a backup of the encrypted file
   - Test the decryption process before deleting originals
   - Verify the contents after extraction

3. File Management
   ```bash
   # Check file sizes
   ls -lh *.gpg

   # Verify tar contents without extracting
   tar -tvf archive.tar.gz

   # Create archive excluding certain files
   tar -czf archive.tar.gz --exclude='*.log' directory/
   ```

## Troubleshooting

1. If you get "gpg: decryption failed: No secret key" error:
   - This usually means incorrect password
   - Try the decryption process again

2. If tar shows "tar: Error is not recoverable: exiting now":
   - Check if you have enough disk space
   - Verify the integrity of the encrypted file

3. Permission Issues:
   ```bash
   # Add execute permission to directory
   chmod +x directory_name

   # Use sudo for system directories
   sudo tar -xzf archive.tar.gz
   ```

## Example Directory Structure After Encryption

```
Before:
my_project/
├── docs/
├── src/
└── config.json

After encryption:
my_project.tar.gz.gpg (single encrypted file)
```

## Security Notes

1. The encrypted file is secure as long as:
   - Your password is strong
   - Your system is secure
   - You don't share the password insecurely

2. Consider using GPG keys instead of passwords for better security

3. Securely delete the original files if needed:
   ```bash
   # Install secure-delete first
   brew install secure-delete

   # Securely delete files
   srm -r original_directory
   ```

## Additional Options

### For larger directories:
```bash
# Use parallel compression
tar -I pigz -cf archive.tar.gz directory/
```

### For better compression:
```bash
# Use XZ compression instead of gzip
tar -cJf archive.tar.xz directory/
```

### For splitting large archives:
```bash
# Split into 1GB chunks
split -b 1G archive.tar.gz.gpg archive.part.
```

# Simple Directory Encryption Guide for macOS (No Compression)

A straightforward guide to create encrypted archives without compression.

## Prerequisites

```bash
# Install GPG if not already installed
brew install gnupg
```

## Basic Commands

### Creating and Encrypting

```bash
# Step 1: Create tar file (without compression)
tar -cf my_archive.tar /path/to/directory

# Step 2: Encrypt the tar file
gpg -c my_archive.tar

# Step 3: (Optional) Remove the original tar file
rm my_archive.tar
```

### Decrypting and Extracting

```bash
# Step 1: Decrypt the file
gpg -d my_archive.tar.gpg > my_archive.tar

# Step 2: Extract the tar file
tar -xf my_archive.tar

# Step 3: (Optional) Clean up
rm my_archive.tar
```

## One-Line Commands

### Encrypt Directory
```bash
# Create tar and encrypt in one go
tar -cf - /path/to/directory | gpg -c > my_archive.tar.gpg
```

### Decrypt and Extract
```bash
# Decrypt and extract in one go
gpg -d my_archive.tar.gpg | tar -xf -
```

## Command Options Explained

### tar commands
- `-c` : Create a new archive
- `-f` : Specify the filename
- `-x` : Extract files
- `-v` : (Optional) Verbose mode to see progress

### gpg commands
- `-c` : Encrypt with symmetric cipher (password)
- `-d` : Decrypt

## Examples

```bash
# Example 1: Encrypt a Documents folder
tar -cf - ~/Documents/important_folder | gpg -c > important.tar.gpg

# Example 2: Decrypt to a specific location
cd ~/Desktop && gpg -d ~/important.tar.gpg | tar -xf -
```

## Notes

1. The `.tar.gpg` extension indicates an encrypted tar file
2. Always remember your password - there's no way to recover it
3. Test the decryption process before deleting originals
4. No compression means larger files but faster processing

## Troubleshooting

1. If you get a "gpg: decryption failed" error:
   - Double-check your password
   - Ensure the .gpg file isn't corrupted

2. If you see "tar: Error is not recoverable":
   - Check if you have enough disk space
   - Verify the file permissions

# Quick Reference Card

## Create & Encrypt
```bash
# Basic (two steps)
tar -cf docs.tar Documents/
gpg -c docs.tar

# One-liner
tar -cf - Documents/ | gpg -c > docs.tar.gpg
```

## Decrypt & Extract
```bash
# Basic (two steps)
gpg -d docs.tar.gpg > docs.tar
tar -xf docs.tar

# One-liner
gpg -d docs.tar.gpg | tar -xf -
```

## Check Contents (after decrypting)
```bash
tar -tf docs.tar
```