# negscript

A lightweight, open-source command-line utility for analog film archivers. `negscript` orchestrates batch-tagging for raw film negative scans by generating a simple, editable CSV spreadsheet scaffold via `ExifTool`, allowing you to safely log historical dates, captions, and keywords before importing frames into non-destructive raw editors like Darktable.

---

## 📥 Installation

`negscript` runs globally as a native system utility on macOS and Linux.

### 1. Prerequisites
This tool requires **ExifTool** to read and write metadata blocks. If you don't have it installed, run:

```bash
# On macOS via Homebrew
brew install exiftool

```

*(Alternatively, download the static package installer from [exiftool.org](https://exiftool.org/))*

### 2. One-Line Installer

Run the following command in your terminal to download the latest version, install it globally to your local binary path, and grant execution permissions:

```bash
sudo curl -fsSL https://raw.githubusercontent.com/lunajuan/negscript/main/negscript -o /usr/local/bin/negscript && sudo chmod +x /usr/local/bin/negscript

```

---

## 🚀 How to Use

`negscript` splits your tagging workflow into two simple steps: creating a spreadsheet and baking it into your files.

### Step 1: Scaffold your Metadata Sheet

Navigate to the directory containing your fresh 16-bit TIFF film scans and run:

```bash
negscript --scaffold

```

This scans the folder and generates a `metadata.csv` file matching every image's relative path.

### Step 2: Fill Out Your Roll Details

Open `metadata.csv` in VS Code, Excel, or any text editor.

Because metadata relies on universal imaging standards (EXIF, IPTC, and XMP), you cannot invent custom column headers (like `random`). `negscript` expects specific, standard fields so that applications like **Apple Photos**, and **Darktable** can index them automatically.

Add your metadata to the following standardized columns:

| Column Heading | Expected Format | Real-World Example | Destination / System Mapping |
| --- | --- | --- | --- |
| **`AllDates`** | `YYYY:MM:DD HH:MM:SS` | `2026:03:08 11:00:00` | Chronological Timeline sorting |
| **`Description`** | Clear-text sentence | `Family dinner on the back porch.` | Image Caption / Notes block |
| **`Keywords`** | Comma-separated strings | `Mexico, vacation, landscape` | Searchable tags & smart albums |
| **`Make`** | Text string | `Olympus` | Camera Manufacturer field |
| **`Model`** | Text string | `OM-1` | Camera Hardware model field |

#### Example of a completed CSV row:

```text
SourceFile,FileName,FileSize,ImageSize,AllDates,Description,Keywords,Make,Model
./img01.tif,img01.tif,228 MB,5115x7421,2026:03:08 11:00:00,Testing the vintage glass,dog, yard, summer,Olympus,OM-1

```
### Step 3: Bake Metadata to Files

Save the CSV file, return to your terminal, and execute:

```bash
negscript --tag

```

`negscript` processes the spreadsheet rows, bakes the EXIF/IPTC information permanently into the files, and automatically cleans up temporary files. These tags will now be indexed perfectly by platforms like Synology Photos or Apple Photos.

---

## 🔄 Updating

You do not need to repeat the manual download steps to update the tool. `negscript` features a built-in self-updater. Simply run:

```bash
sudo negscript --update

```

This forces the tool to pull down your latest codebase straight from GitHub and overwrite your global binary with the new script logic.

---

## 💻 Developer Guide & Local Setup

If you want to contribute to the code, fix bugs, or add features, follow this workflow to develop cleanly outside of your system paths.

### 1. Set Up Local Workspace

Clone your repository into a local coding project folder:

```bash
cd ~/Documents/Projects
git clone https://github.com/lunajuan/negscript.git
cd negscript

```

### 2. The Symlink Trick

Do **not** copy files directly into `/usr/local/bin/` during active development. Instead, create a symbolic link pointing to your local Git directory:

```bash
# Make your local repository script executable
chmod +x negscript

# Link it globally
sudo ln -s "$(pwd)/negscript" /usr/local/bin/negscript

```

Now, when you run `negscript` anywhere on your computer, your terminal executes your live project file. You can test your changes inside VS Code instantly without resetting permissions or manually copying files.

### 3. Deploying Code Changes

When your feature additions or code refinements are fully tested:

1. Commit your changes locally.
2. Push them to the `main` branch on GitHub.
3. Users (and your own separate production environments) can immediately access the update via `negscript --update`.

