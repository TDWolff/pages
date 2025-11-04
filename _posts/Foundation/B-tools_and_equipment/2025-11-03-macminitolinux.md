---
title: Mac Mini to Ubuntu Setup Guide
permalink: /foundation/b-tools-and-equipment/macminitolinux/
---

<style>
    .tabs {
        display: flex;
        gap: 10px;
        margin: 20px 0;
        border-bottom: 2px solid #ddd;
    }

    .tab-button {
        padding: 10px 20px;
        background: #79baffff;
        border: none;
        cursor: pointer;
        font-size: 16px;
        border-radius: 5px 5px 0 0;
    }

    .tab-button.active {
        background: #007bff;
        color: white;
    }

    .tab-content {
        display: none;
        padding: 20px 0;
    }

    .tab-content.active {
        display: block;
    }

    .note {
        background: #fff3cd;
        border-left: 4px solid #ffc107;
        padding: 15px;
        margin: 20px 0;
    }

    .credential-box {
        background: #f5f5f5;
        border: 1px solid #ddd;
        padding: 15px;
        margin: 15px 0;
        font-family: monospace;
    }

    pre {
        background: #f7f7f7;
        border: 1px solid #ddd;
        padding: 10px;
        border-radius: 5px;
        overflow-x: auto;
    }
</style>

<div class="container">
    <header>
        <h1>🖥️ Mac Mini OS Setup Guide</h1>
        <p>Setting up 2010–2013 Mac Minis with alternate operating systems</p>
    </header>

    <div class="tabs">
        <button class="tab-button active" onclick="switchTab('ubuntu', event)">Ubuntu Setup</button>
        <button class="tab-button" onclick="switchTab('kali', event)">Kali Linux Setup</button>
        <button class="tab-button" onclick="switchTab('script', event)">apps_ubuntu.sh</button>
    </div>

    <!-- Ubuntu Setup -->
    <div id="ubuntu" class="tab-content active">
        <h2>Ubuntu Setup</h2>
        <p>This guide walks through setting up a Mac Mini (2010–2013) to run Ubuntu instead of macOS.</p>

        <div class="note">
            <strong>Note:</strong> Mac Minis from 2009 or earlier cannot boot Ubuntu 24 Noble natively due to 64-bit EFI requirements.
        </div>

        <p><strong>OS:</strong> Ubuntu 24.04.3 Noble — <a href="https://releases.ubuntu.com/noble/" target="_blank">Download Here</a></p>

        <h3>Steps:</h3>
        <ol>
            <li>Burn the Ubuntu ISO to a flash drive (6GB or larger).</li>
            <li>Insert the USB drive into your Mac Mini.</li>
            <li>Hold the power button and <strong>Option</strong> key to access the boot menu.</li>
            <li>Select <strong>EFI Boot</strong>.</li>
            <li>Choose <strong>Try and install Ubuntu</strong> and proceed with installation.</li>
            <li>Check <strong>Install third-party software</strong> for drivers during setup.</li>
            <li>Use these credentials:</li>
        </ol>

        <div class="credential-box">
            Name: ubuntu<br>
            Username: ubuntu<br>
            Password: Ubuntu14*&*41
        </div>

        <h3>Wi-Fi Fix</h3>
        <ol>
            <li>Connect via ethernet.</li>
            <li>Run:
                <pre><code>sudo add-apt-repository restricted
sudo apt update
sudo apt install bcmwl-kernel-source
sudo reboot</code></pre>
            </li>
        </ol>

        <h3>Install Apps</h3>
        <p>Run the following script:  
            <a href="https://gist.githubusercontent.com/TDWolff/b77194acea8c2d8bb476c136250e497c/raw/b90b87fcfb224c1fbb9d55b057c79e398210d6ab/apps_ubuntu.sh" target="_blank">apps_ubuntu.sh</a>
        </p>
    </div>

    <!-- Kali Linux Setup -->
    <div id="kali" class="tab-content">
        <h2>Kali Linux Setup</h2>
        <p>For advanced users or security professionals, Kali Linux is a great choice for testing and system auditing with advanced customizability.</p>

        <p><strong>OS:</strong> Kali Linux — <a href="https://www.kali.org/get-kali/#kali-installer-images" target="_blank">Download Installer Image</a></p>

        <h3>Steps:</h3>
        <ol>
            <li>Download the ISO from Kali’s official website.</li>
            <li>Use <code>Rufus</code> to burn the image to a flash drive (6GB+).</li>
            <li>Insert the USB into your Mac Mini.</li>
            <li>Boot while holding the <strong>Option</strong> key and choose <strong>EFI Boot</strong>.</li>
            <li>Select <strong>Graphical Install</strong> and follow the prompts.</li>
            <li>During setup, select the default desktop environment (XFCE recommended for stability).</li>
            <li>Once installation is done, reboot and log in using your created credentials.</li>
        </ol>

        <div class="note">
            <strong>Tip:</strong> After setup, update your system using:
            <pre><code>sudo apt update && sudo apt full-upgrade -y</code></pre>
        </div>
    </div>

    <!-- Script Tab -->
    <div id="script" class="tab-content">
        <h2>apps_ubuntu.sh</h2>
        {% raw %}
        <pre><code>echo Adding App Installations for Ubuntu
# Install VS Code
sudo snap install --classic code
# Install Git
sudo apt update
sudo apt install -y git

# Install Google Chrome
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome-stable_current_amd64.deb

echo "Creating desktop shortcuts"

USER_HOME=$(eval echo "~$USER")
DESKTOP_PATH="$USER_HOME/Desktop"
mkdir -p "$DESKTOP_PATH"

# VS Code
cat <<EOF > "$DESKTOP_PATH/Visual Studio Code.desktop"
[Desktop Entry]
Name=Visual Studio Code
Comment=Code Editing. Redefined.
Exec=code --no-sandbox --unity-launch %F
Icon=/snap/code/current/meta/gui/com.visualstudio.code.png
Terminal=false
Type=Application
Categories=Development;IDE;
EOF

# Google Chrome
cat <<EOF > "$DESKTOP_PATH/Google Chrome.desktop"
[Desktop Entry]
Name=Google Chrome
Comment=Access the Internet
Exec=/usr/bin/google-chrome-stable %U
Icon=/usr/share/icons/hicolor/128x128/apps/google-chrome.png
Terminal=false
Type=Application
Categories=Network;WebBrowser;
EOF

# Git (optional shortcut)
cat <<EOF > "$DESKTOP_PATH/Git Bash.desktop"
[Desktop Entry]
Name=Git Terminal
Comment=Open terminal for Git
Exec=gnome-terminal
Icon=utilities-terminal
Terminal=false
Type=Application
Categories=Development;Utility;
EOF

chmod +x "$DESKTOP_PATH"/*.desktop</code></pre>
        {% endraw %}
<script>
    function switchTab(tabName, event) {
        const contents = document.querySelectorAll('.tab-content');
        const buttons = document.querySelectorAll('.tab-button');

        contents.forEach(c => c.classList.remove('active'));
        buttons.forEach(b => b.classList.remove('active'));

        const selected = document.getElementById(tabName);
        if (selected) selected.classList.add('active');

        if (event && event.target) event.target.classList.add('active');
    }
</script>
