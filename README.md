# FileHandler
A simple file handler in c++.

## Features
- Creation of files and directories(indirectly)
- Deletion of files
- Renaming of files
- File copy/move
- Read and write (string based)
- Read and write (binary)
- Read (as lines/single line)
- Simple encryption and decryption of files
- Creation of file backups (with and without timestemp)
- Comparing of files

## How to use
```cpp
#include <FileHandler.hpp>

int main() {
    fs::path path("example/example.txt");
    fs::path path2("example/example2.txt");
    fs::path path3("example/example3.txt");
    fs::path backupPath("example/backups/");
    std::vector<std::string> lines;
    std::string line;

    FileHandler::CreateFile(path);

    FileHandler::WriteToFile(path, "hew how are you?\nToday is a nice day!\nNice to meet you!", std::ios::out);

    FileHandler::SimpleEncryptFile(path, path2, "this is my key");
    FileHandler::SimpleDecryptFile(path2, path3, "this is not my key");

    FileHandler::CreateBackupFromFile(path);
    FileHandler::CreateBackupFromFile(path, backupPath);

    FileHandler::GetLinesFromFile(path, lines);
    FileHandler::GetLineFromFile(path, line, 3);

    FileHandler::RenameFile(path3, "test3.txt");
    FileHandler::CopyFile(path, "example/backups");

    return 0;
}
```
