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

# Fork & Clone This Project

Let's fork this project. Which means to copy the repository into your own account:

1. Login to your GitHub account (create one if you don't already have one).
2. Go to the repository page: https://github.com/CODEDHQ/popotheworker-starter
3. Click the `Fork` button on the top right corner of the page:

   ![Fork](https://imgur.com/KmH4Fp4.png)

4. Give it a moment to fork the repository, creating a copy of it under your account and ownership.

Next, clone it:

1. `cd` into the location you'd like to keep this project at.
2. Clone this project:
   ```bash
   $ git clone https://github.com/<YOUR_GITHUB_USERNAME>/popotheworker-starter
   ```
   If your GitHub username is "mcCool107", then the command will be:
   ```bash
   $ git clone https://github.com/mcCool107/popotheworker-starter
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
