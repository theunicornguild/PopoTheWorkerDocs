We need to install a few things for our computer so we're ready to develop this site. We need `yarn` to build and run a React app.

## MacOS

Here are the steps to install it on macOS:

1. Install [Homebrew](https://brew.sh/)
   ```bash
   $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```
2. Install yarn
   ```bash
   $ brew install yarn
   ```

## Windows

Download the Windows Installer from [here](https://yarnpkg.com/en/docs/install#windows-stable).

If you don't already have git installed, install it with Git Bash:

1. Go to [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Download the Git windows setup (64-bit)
3. Install the application

   - When it Prompts you to adjust your PATH environment you should pick the 3rd option `Use Git and optional Unix tools from the Windows Command Prompt`
   - You can choose the default options the installer already picked for you for other things

# Clone This Project

1. `cd` into the location you'd like to keep this project at.
2. Clone this project:
   ```bash
   $ git clone https://github.com/CODEDHQ/popotheworker-starter
   ```
3. Install required packages:
   ```bash
   $ cd popotheworker-starter
   $ yarn install
   ```
4. Run project:
   ```bash
   $ yarn start
   ```
