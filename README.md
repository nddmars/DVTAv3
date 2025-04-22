# DVTA - Desktop Vulnerable Thick Client Application

**DVTA** (Desktop Vulnerable Thick Client Application) is a C# WinForms application built on .NET Framework 4.5. It is intentionally designed with common security flaws to serve as a hands‑on training tool for developers and security professionals to identify and mitigate vulnerabilities in desktop applications.

## Table of Contents

- [Features](#features)
- [Vulnerabilities Covered](#vulnerabilities-covered)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [Contribution](#contribution)
- [License](#license)
- [Acknowledgements](#acknowledgements)

## Features

- User registration and authentication
- Expense tracking: add, view, and export expenses
- CSV export functionality via `DataTableToCsv` extension
- Basic admin functionality for viewing all users' data

## Vulnerabilities Covered

1. **Insecure local data storage**: Sensitive credentials and encryption keys stored in plaintext (`App.config`).
2. **Insecure logging**: Use of `Console.WriteLine` to log decrypted passwords and connection strings.
3. **Weak cryptography**: Hardcoded AES key and IV with improper management.
4. **Exposed decryption logic**: Cryptographic routines visible in source (`DBAccessClass`).
5. **SQL Injection**: Concatenation of user input into SQL commands (`checkLogin`, `addExpenses`, etc.).
6. **CSV Injection**: Unsanitized export in `DataTableToCsv` allowing malicious formulas.
7. **Sensitive data in memory**: Use of immutable `String` for handling sensitive data.
8. **DLL Hijacking**: Inclusion of `ExcelLibrary.dll` without secure loading paths.
9. **Clear text data in transit**: Connection strings and credentials transmitted in clear text.
10. **Lack of code obfuscation**: All source code is available in plaintext.

## Project Structure

```
DVTA.sln
├── DVTA/              # WinForms UI project
├── DBAccess/          # Data access layer (encryption & SQL logic)
├── DB/                # Database layer (models & operations)
├── packages/          # NuGet packages (EntityFramework 5.0.0)
└── ExcelLibrary.dll   # Library for Excel export
```

## Prerequisites

- **Operating System**: Windows with .NET Framework 4.5
- **IDE**: Visual Studio 2015 or later
- **Database**: SQL Server instance (e.g., SQL Server Express)

## Installation & Setup

1. **Clone the repository**
   ```bash
   git clone <repo-url>
   cd dvta-updated-master/DVTA
   ```
2. **Configure the database**
   - Update `App.config` in the `DVTA` project with your environment:
     ```xml
     <add key="DBSERVER" value="YOUR_SQL_SERVER_INSTANCE" />
     <add key="DBNAME" value="DVTA" />
     <add key="DBUSERNAME" value="YOUR_DB_USERNAME" />
     <add key="DBPASSWORD" value="YOUR_ENCRYPTED_PASSWORD" />
     <add key="AESKEY" value="YOUR_AES_KEY" />
     <add key="IV" value="YOUR_INITIALIZATION_VECTOR" />
     ```
   - Ensure your SQL Server hosts a `DVTA` database with the following tables:
     - `users` (columns: `username`, `password`, `email`, `isadmin`)
     - `expenses` (columns: `item`, `price`, `date`, `time`, `email`)
3. **Build & Run**
   - Open `DVTA.sln` in Visual Studio.
   - Build the solution (`Build > Build Solution`).
   - Set `DVTA` as the startup project and run the application.

## Usage

1. **Register** a new user via the registration form.
2. **Login** with your credentials.
3. **Add Expenses**: Enter expense details to store them.
4. **View Expenses**: List personal or all users' expenses (admin only).
5. **Export CSV**: Export expense data to CSV files for analysis.

## Contribution

Contributions are welcome! Feel free to fork the repository and submit pull requests with security improvements, feature enhancements, or documentation updates.

## Acknowledgements

- [EntityFramework 5.0.0](https://www.nuget.org/packages/EntityFramework/)
- [ExcelLibrary](http://www.codeproject.com/KB/office/ExcelLibrary.aspx)

