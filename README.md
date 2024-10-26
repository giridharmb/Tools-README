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