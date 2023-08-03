# FileHandler
## Overview
The FileHandler library provides a set of functions for handling file-related operations in C++. It encapsulates common file operations, such as reading from and writing to files, copying, moving, and deleting files, and more.

## How to Use
To use the FileHandler library, follow the steps below:

1. Include the necessary headers in your C++ code.

2. The library is contained within the FileHandler namespace. You can access its functions using the FileHandler::<function_name> syntax.

3. Available Functions:

- ```cpp
  std::size_t GetFileSize(const fs::path& path) - Get the size of a file.
  ```
- ```cpp
  std::size_t GetFileSize(std::ifstream& inStream) - Get the size of a file from an open input stream.
  ```
- ```cpp
  bool CreateFile(const fs::path& path) - Create a new file if it does not exist.
  ```
- ```cpp
  bool DeleteFile(const fs::path& path) - Delete a file.
  ```
- ```cpp
  bool RenameFile(const fs::path& path, const std::string& newFileName) - Rename a file.
  ```
- ```cpp
  bool CopyFile(const fs::path& src, const fs::path& dst) - Copy a file from the source to the destination.
  ```
- ```cpp
  bool MoveFile(const fs::path& src, const fs::path& dst) - Move a file from the source to the destination.
  ```
- ```cpp
  bool WriteToFile(const fs::path& path, const std::string& buffer, const std::ios_base::openmode openMode = std::ios::out) - Write a string to a file.
  ```
- ```cpp
  bool ReadFromFile(const fs::path& path, std::string& buffer) - Read a string from a file.
  ```
- ```cpp bool
  WriteBinaryToFile(const fs::path& path, const std::vector<uint8_t>& buffer, const std::ios_base::openmode openMode = std::ios::binary) - Write binary data (vector of uint8_t) to a file.
  ```
- ```cpp
  bool WriteBinaryToFile(const fs::path& path, const void* buffer, const std::streamsize& streamSize, const std::ios_base::openmode openMode = std::ios::binary) - Write binary data (from a buffer) to a file.
  ```
- ```cpp
  bool ReadBinaryFromFile(const fs::path& path, std::vector<uint8_t>& buffer) - Read binary data (vector of uint8_t) from a file.
  ```
- ```cpp
  bool ReadBinaryFromFile(const fs::path& path, void* buffer, const std::streamsize& streamSize) - Read binary data (into a buffer) from a file.
  ```
- ```cpp
  bool SimpleEncryptFile(const fs::path& inputPath, const fs::path& outputPath, const std::string& key) - Encrypt a file using a simple XOR encryption with a given key.
  ```
- ```cpp bool
  SimpleDecryptFile(const fs::path& inputPath, const fs::path& outputPath, const std::string& key) - Decrypt a file encrypted using the SimpleEncryptFile function.
  ```
- ```cpp
  bool CompareFiles(const fs::path& firstPath, const fs::path& secondPath) - Compare two files to check if their content is the same.
  ```
- ```cpp
  bool CreateBackupFromFile(const fs::path& path, const bool& dontOverride = false) - Create a backup of a file.
  ```
- ```cpp
  bool CreateBackupFromFile(const fs::path& path, const fs::path& backupPath, const bool& dontOverride = false) - Create a backup of a file with a custom backup path.
  ```
- ```cpp bool
  GetLinesFromFile(const fs::path& path, std::vector<std::string>& buffer) - Read lines from a file into a vector of strings.
  ```
- ```cpp bool
  GetLineFromFile(const fs::path& path, std::string& buffer, const std::size_t& line) - Read a specific line from a file into a string.
  ```
4. Each function provides a return value to indicate the success or failure of the operation. In case of failure, error messages are printed to std::cerr.

5. Ensure to handle any potential exceptions or errors when using these functions, such as when files do not exist or when access permissions are insufficient.

## Example
Below is a simple example demonstrating how to use some of the functions in the FileHandler library:
### Read and write
```cpp
#include <iostream>
#include "FileHandler.h"

int main() {
    std::string filePath = "example.txt";
    std::string content = "Hello, FileHandler Library!\n";

    // Write content to a file
    if (FileHandler::WriteToFile(filePath, content)) {
        std::cout << "Content written to file successfully." << std::endl;
    } else {
        std::cerr << "Failed to write content to file." << std::endl;
        return 1;
    }

    // Read content from the file and display it
    std::string readContent;
    if (FileHandler::ReadFromFile(filePath, readContent)) {
        std::cout << "Read content from file: " << readContent << std::endl;
    } else {
        std::cerr << "Failed to read content from file." << std::endl;
        return 1;
    }

    return 0;
}
```

### Encrypt file
```cpp
#include <iostream>
#include "FileHandler.h"

int main() {
    std::string inputFile = "input.txt";
    std::string outputFile = "encrypted.txt";
    std::string key = "my_secret_key";

    // Create a sample input file
    std::string content = "Hello, FileHandler Library! This is a sample input file.";
    if (FileHandler::WriteToFile(inputFile, content)) {
        std::cout << "Sample input file created: " << inputFile << std::endl;
    } else {
        std::cerr << "Failed to create the input file." << std::endl;
        return 1;
    }

    // Encrypt the file
    if (FileHandler::SimpleEncryptFile(inputFile, outputFile, key)) {
        std::cout << "File encrypted and saved as: " << outputFile << std::endl;
    } else {
        std::cerr << "Failed to encrypt the file." << std::endl;
        return 1;
    }

    return 0;
}
```

### Decrypt file
```cpp
#include <iostream>
#include "FileHandler.h"

int main() {
    std::string inputFile = "encrypted.txt";
    std::string outputFile = "decrypted.txt";
    std::string key = "my_secret_key";

    // Decrypt the file
    if (FileHandler::SimpleDecryptFile(inputFile, outputFile, key)) {
        std::cout << "File decrypted and saved as: " << outputFile << std::endl;
    } else {
        std::cerr << "Failed to decrypt the file." << std::endl;
        return 1;
    }

    // Read and display the decrypted content
    std::string decryptedContent;
    if (FileHandler::ReadFromFile(outputFile, decryptedContent)) {
        std::cout << "Decrypted content: " << decryptedContent << std::endl;
    } else {
        std::cerr << "Failed to read decrypted content from file." << std::endl;
        return 1;
    }

    return 0;
}
```

### Backup a file
```cpp
std::string filePath = "data.txt";
    std::string backupFilePath = "data_backup.txt";

    // Create a backup of the file
    if (FileHandler::CreateBackupFromFile(filePath)) {
        std::cout << "Backup of the file created successfully." << std::endl;
    } else {
        std::cerr << "Failed to create a backup of the file." << std::endl;
        return 1;
    }

    // Alternatively, you can specify a custom backup path
    std::string customBackupPath = "backups/";
    if (FileHandler::CreateBackupFromFile(filePath, customBackupPath)) {
        std::cout << "Custom backup of the file created successfully." << std::endl;
    } else {
        std::cerr << "Failed to create a custom backup of the file." << std::endl;
        return 1;
    }

    // With timestemp
    if (FileHandler::CreateBackupFromFile(filePath), true) {
        std::cout << "Backup of the file created successfully." << std::endl;
    } else {
        std::cerr << "Failed to create a backup of the file." << std::endl;
        return 1;
    }

    std::string customBackupPath = "backups/";
    if (FileHandler::CreateBackupFromFile(filePath, customBackupPath), true) {
        std::cout << "Custom backup of the file created successfully." << std::endl;
    } else {
        std::cerr << "Failed to create a custom backup of the file." << std::endl;
        return 1;
    }

    return 0;
```
Please note that this example assumes you have already created the FileHandler.h header file containing the FileHandler namespace and its functions. Also, ensure that you have the necessary permissions to read from and write to the specified file.
