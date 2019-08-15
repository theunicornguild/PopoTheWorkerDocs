1. Install [Chocolatey](https://chocolatey.org/install).
2. Run the command line as administrator. You do that by right-clicking on the CMD application, and clicking on "Run As Administrator".
3. Paste the following command:
   ```bash
   $ @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
   ```
   If that command doesn't work, go [here](https://chocolatey.org/install#install-with-cmdexe) and copy the command under "`Install with cmd.exe`".
4. Wait as it should take some time to finish installing.
5. Close the command line and open it again as administrator.
6. Install NodeJS by running the command:
   ```bash
   $ choco install nodejs
   $ choco install yarn
   ```
7. Wait for it to finish installing.
8. Close the command line.
