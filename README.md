# 🐧 ChatGPT for Linux (Nativefier Client)

This guide shows you how to create a desktop application for ChatGPT on Linux (Debian/Ubuntu-based distributions) using [Nativefier](https://github.com/nativefier/nativefier).

Nativefier wraps any website (in this case, `chat.openai.com`) into an Electron application, giving you a simple desktop client that is separate from your main browser.

## 1. Prepare Your Environment (Node.js with NVM)

Instead of using `apt`, we will use `nvm` (Node Version Manager) to install `node` and `npm`. This avoids permission issues and allows us to install the most compatible recent version.

```bash
# 1. Download and install nvm (Node Version Manager)
curl -o- [https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh](https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh) | bash

# 2. Activate nvm in your current session
# (You may need to close and reopen your terminal after this step)
export NVM_DIR="$HOME/.nvm"
source "$NVM_DIR/nvm.sh"

# 3. Install, use, and set Node.js v18 (a stable LTS version) as default
nvm install 18
nvm use 18
nvm alias default 18

# 4. Confirm the versions
echo "Node version:"
node -v  # Should be v18.x.x
echo "NPM version:"
npm -v   # Should be 9.x.x or 10.x.x
````

## 2\. Install Nativefier

Now that you have `npm`, you can install `nativefier` globally on your system:

```bash
npm install -g nativefier
```

## 3\. Create the ChatGPT Application

1.  Decide where you want the application to be "installed". A good place is an `Apps` folder in your `home` directory.
2.  Navigate to that directory and run the `nativefier` command.

<!-- end list -->

```bash
# Create the folder if it doesn't exist and enter it
mkdir -p ~/Apps
cd ~/Apps

# Run nativefier with recommended options
nativefier --name "ChatGPT" \
           --single-instance \
           --internal-urls ".*chat.openai.com.*" \
           "[https://chat.openai.com](https://chat.openai.com)"
```

**Option Explanations:**

  * `--name "ChatGPT"`: The name of the application.
  * `--single-instance`: Prevents you from accidentally opening multiple windows of the app.
  * `--internal-urls ".*chat.openai.com.*"`: Forces any links *outside* of ChatGPT (e.g., a Google link) to open in your regular web browser, keeping the app clean.

Nativefier will work for a few seconds and create a new folder named `ChatGPT-linux-x64` in your `~/Apps` directory.

## 4\. (Optional) Create a Menu Shortcut

To make "ChatGPT" appear in your applications menu (alongside Firefox, VSCode, etc.), you need to create a `.desktop` file.

1.  Open a text editor (like `nano`) to create the launcher file:

    ```bash
    nano ~/.local/share/applications/chatgpt.desktop
    ```

2.  **Paste the following content** into `nano`.

    **IMPORTANT\!** You must replace `YOUR_USER` with your actual username (the one you see in your terminal prompt, e.g., `/home/yourusername`).

    ```ini
    [Desktop Entry]
    Version=1.0
    Type=Application
    Name=ChatGPT
    Comment=Desktop client for ChatGPT (created with Nativefier)

    # CHANGE THIS PATH!!
    # Replace 'YOUR_USER' with your username
    Exec=/home/YOUR_USER/Apps/ChatGPT-linux-x64/ChatGPT

    # This icon path should be correct
    # Replace 'YOUR_USER' with your username
    Icon=/home/YOUR_USER/Apps/ChatGPT-linux-x64/resources/app/icon.png

    Terminal=false
    Categories=Utility;Network;Application;
    ```

3.  Save the file and exit `nano` (Ctrl+O, Enter, Ctrl+X).

4.  Finally, make the launcher executable:

    ```bash
    chmod +x ~/.local/share/applications/chatgpt.desktop
    ```

-----

That's it\! You should now be able to search for "ChatGPT" in your applications menu and launch it just like any other program.

```
```
