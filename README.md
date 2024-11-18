![Thumbnail](/ci/thumbnail.png)

# godot-game-client
Godot Game Client allows users to download and play multiple Godot applications using a single application. The client works on Desktop, Mobile, and Web using the OpenGL compatability mode.

## Working with Packs
This "Client" can import Packs (`.pck`) from remote resources. They are cataloged in [scenes/main-menu/recommendations.json](scenes/main-menu/recommendations.json). To avoid file name conflicts with the Client and other packs, the Pack's scenes should be in a sub-folder with the Pack's name. E.g., `res://scenes/my_pack`.

### Setup Remote Repo
1. An new, empty file named `ci/.gdignore`
	1. Prevents Godot from importing files contained in this folder
1. A new file named `ci/export-pack.ps1`
	1. Paste in the contents of [export-pack.ps1](ci/export-pack.ps1)
	1. Use this to pack games using PowerShell
1. A new file named `ci/export-pack.sh`
	1. Paste in the contents of [export-pack.sh](ci/export-pack.sh)
	1. Use this to pack games using Bash
1. A new file named `ci/thumbnail.png`
	1. Use the [placeholder](assets/textures/placeholder-co/286x160.png)
1. Run the script and commit
	The pack and thumbnail should be publically available

### Add Remote Repo to Catalog
1. Open/Edit [scenes/main-menu/recommendations.json](scenes/main-menu/recommendations.json)
1. Add a new JSON Object with the following:
	- "title": Displayed under the thumbnail
	- "short": A brief description, displayed under the title
	- "long": A longer description, displayed on the details page
	- "thumb": The displayed image for the project in the catalog
	- "pack": The name of the pack, when it's imported
		- E.g., `res://scenes/my-pack` -> `my-pack`
	- "scene": The main scene (entry-point) of the pack
	- "url": The pack download URL
1. Once the Client loads, it will attempt to download the Thumbnail
	- If the user clicks the thumbnail, the "pack" will download
	- Once the download completes, the "scene" will be loaded


<details>
<summary>Using Addons</summary>

### Installing the Addon
1. Download [install-3d-player-controller.sh](ci/install-3d-player-controller.sh)
1. Move the file to a folder named `ci` in your project
1. Open your project in VS Code
1. Open the "Git Bash" terminal
1. Run `bash ci/install-3d-player-controller.sh`
	- This script will download the [3d_player_controller](/addons/3d_player_controller) folder from _this_ repo and then cleanup the `.git` files/folders.

</details>

<details>
<summary>Export Game as Pack</summary>

## Game Pack
This game can be [exported](https://docs.godotengine.org/en/stable/tutorials/export/exporting_pcks.html#generating-pck-files) as a `.pck` and [imported](https://docs.godotengine.org/en/stable/tutorials/export/exporting_pcks.html#opening-pck-files-at-runtime) into another Godot game client, like the [Godot Game Client](https://github.com/kirbycope/godot-game-client).

### Export Game as Pack Using Godot
1. Select "Project" > "Export.."
	1. Download the Presets, if prompted
1. Select "Add..."
1. Select "Web"
1. Select "Export PCK/ZIP..."
1. Change the type to "Godot Project Pack (*.pck)"
1. Select "Save"

### Export Game as Pack Using Bash
1. Open the root folder using [VS Code](https://code.visualstudio.com/)
	- If you use GitHub Desktop, select the "Open in Visual Studio" button
1. Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) using the "Git Bash" profile
1. Run the following command, `bash ci/export-pack.sh`

</details>

<details>
<summary>Web Export</summary>

### Export Game as Web App
1. Select "Project" > "Export..."
1. Select the preset "Web (Runnable)"
1. Select "Export Project..."
1. Select the "docs" folder
	- The GitHub Pages config points to the `main` branch and `/docs` directory
1. Enter `index.html`
1. Select "Save"
1. Commit the code to trigger a GitHub Pages deployment (above)

### Export Game as Web App Using Bash
1. Open the root folder using [VS Code](https://code.visualstudio.com/)
	- If you use GitHub Desktop, select the "Open in Visual Studio" button
1. Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) using the "Git Bash" profile
1. Run the following command, `bash ci/export-web.sh`

</details>

<details>
<summary>Web Hosting with localhost</summary>

### Install and Enable Live Server
[Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) allows you to host web pages, locally, from VSCode.

### Running/Hosting the App Locally
1. In VSCode's Explorer right-click on [docs/index.html](docs/index.html) and select "Open with Live Server"
1. Then you visit [https://127.0.0.1:5500/docs/index.html](https://127.0.0.1:5500/docs/index.html)
1. To get your "Host Local IP Address", use terminal to run:
	- [Windows] `ipconfig`
	- [MacOS] `ipconfig getifaddr en0`
1. On a device connected to the same wifi as the host, navigate to `https://{host.local.ip.address}:5500/docs/index.html`
	- Replace `{host.local.ip.address}` with your "Host Local IP Address" from earlier

</details>

<details>
<summary>Access localhost from Devices on Same Wifi Network</summary>

### Generate HTTPS Certificate
"Secure Context - Check web server configuration (use HTTPS)" The following features required to run Godot projects on the Web. Do the following to setup
1. Download and install the [ssl binary](https://wiki.openssl.org/index.php/Binaries)
	- [Windows] Use [OpenSSL for Windows](https://slproweb.com/products/Win32OpenSSL.html)
	- [MacOS] Use [Homebrew](https://brew.sh/) by running, `brew install openssl@3`
1. Confirm installation by running `openssl -v` in cmd/terminal
1. Open the root folder using [VS Code](https://code.visualstudio.com/)
	- If you use GitHub Desktop, select the "Open in Visual Studio" button
1. Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)
1. Run `openssl genrsa -aes256 -out localhost.key 2048`
	- You will be prompted for a "PEM pass phrase", remember this for the next step
	- `godot`
1. Run `openssl req -days 3650 -new -newkey rsa:2048 -key localhost.key -x509 -out localhost.pem`
	- You will be prompted for the "PEM pass phrase"
	- Fill out the rest of the information as the prompts request
		- "Country Name (2 letter code) [AU]:"`US`
		- "State or Province Name (full name) [Some-State]:"`WA`
		- "Locality Name (eg, city) []:"`Seattle`
		- "Organization Name (eg, company) [Internet Widgits Pty Ltd]:"`Timothy Cope`
		- "Organizational Unit Name (eg, section) []:"`Development`
		- "Common Name (e.g. server FQDN or YOUR name) []:"`localhost`
		- "Email Address []:"`kirbycope@gmail.com`
1. Open/Create `.vscode/settings.json` in the root of your project
1. Copy+paste the following:
	```
	{
		"liveServer.settings.root": "/",
		"liveServer.settings.https": {
			"enable": true,
			"cert": "{path/to/your/}localhost.pem",
			"key": "{path/to/your/}localhost.key",
			"passphrase": "{PEM pass phrase}"
		}
	}
	```
	- Replace `{PEM pass phrase}` with your "PEM pass phrase" from earlier
1. Restart VSCode (or the terminal, at least)

</details>

<details>
<summary>Web Hosting with GitHub Pages</summary>

### Set Up GitHub Pages
Note: This only needs to be done once.
1. Go to the "Settings" tab of the repo
1. Select "Pages" from left-nav
1. Select `main` branch and `/docs` directory, then select "Save"
	- A GitHub Action will deploy your website
1. On the main page of the GitHub repo, click the gear icon next to "About"
1. Select "Use your GitHub Pages website", then select "Save changes"

### Set Up Godot
Note: This only needs to be done once.</br>
The following is needed to work with GitHub Pages.
1. Select "Project" > "Export..."
	- If you see errors, click the link for "Manage Export Templates" and then click "Download and Install"
1. Select the preset "Web (Runnable)"
1. For "Head Include", enter `<script src="coi-serviceworker.js"></script>`
1. Download [coi.js](https://github.com/gzuidhof/coi-serviceworker/raw/master/coi-serviceworker.js) and add it to the `/docs` directory

</details>
