# 📄 Google Drive View-Only PDF Downloader

<p align="center">
  <img src="https://img.shields.io/badge/JavaScript-ES6-yellow?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-Working-brightgreen?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Platform-Browser-blue?style=for-the-badge" />
</p>

<p align="center">
  <b>Download view-only PDF files from Google Drive by capturing loaded pages and rebuilding them into a high-quality PDF.</b>
</p>

---

## ✨ Features

✅ Works on **Google Drive View-Only PDFs**  
📸 Captures **each PDF page as a high-resolution image**  
📄 Automatically **combines all pages into a single PDF**  
⚡ Maintains **original page dimensions & orientation**  
🌐 Runs **entirely in the browser** (no installation needed)

---

## 🎬 How It Works (Concept)

> Google Drive loads view-only PDFs as image blobs.  
> This script:
1. Detects all loaded PDF page images
2. Converts them into canvas data
3. Rebuilds them into a downloadable PDF using `jsPDF`

---

## 🚀 Usage Instructions

### 1️⃣ Open the PDF
- Open the **Google Drive PDF**
- Click **Preview**
- Click the **⋮ (three dots)** → **Open in new window**

### 2️⃣ Load All Pages
- Scroll **all the way down**
- Make sure **every page is fully loaded**

### 3️⃣ Open Developer Console
- Press:
  - **Windows / Linux:** `Ctrl + Shift + I`
  - **Mac:** `Cmd + Option + I`
- Go to the **Console** tab

> ⚠️ **Chrome users:**  
> Type the following and press **ENTER**:
```js
allow pasting

(function () {
  console.log("Loading script ...");

  let script = document.createElement("script");
  script.onload = function () {
    const { jsPDF } = window.jspdf;

    // Generate a PDF from images with "blob:" sources.
    let pdf = null;
    let imgElements = document.getElementsByTagName("img");
    let validImgs = [];

    console.log("Scanning content ...");
    for (let i = 0; i < imgElements.length; i++) {
      let img = imgElements[i];

      // Specific check for Google Drive blob images
      let checkURLString = "blob:https://drive.google.com/";
      if (img.src.substring(0, checkURLString.length) !== checkURLString) {
        continue;
      }

      validImgs.push(img);
    }

    console.log(`${validImgs.length} content found!`);
    console.log("Generating PDF file ...");

    for (let i = 0; i < validImgs.length; i++) {
      let img = validImgs[i];

      // Convert image to DataURL via Canvas
      let canvasElement = document.createElement("canvas");
      let con = canvasElement.getContext("2d");
      canvasElement.width = img.naturalWidth;
      canvasElement.height = img.naturalHeight;
      con.drawImage(img, 0, 0, img.naturalWidth, img.naturalHeight);
      let imgData = canvasElement.toDataURL();

      // Determine orientation and dimensions
      let orientation = img.naturalWidth > img.naturalHeight ? "l" : "p";
      let pageWidth = img.naturalWidth;
      let pageHeight = img.naturalHeight;

      if (i === 0) {
        // Initialize PDF with first page size
        pdf = new jsPDF({
          orientation: orientation,
          unit: "px",
          format: [pageWidth, pageHeight],
        });

4️⃣ Paste the Script

Copy the script

Paste it into the console

Press ENTER

5️⃣ Wait & Download

The console will show progress

PDF will download automatically when finished 🎉

🧠 Script Behavior Notes

⏳ Processing speed depends on:

Number of pages

Page resolution

Device performance

📐 Each page keeps its original width, height, and orientation

📝 Output filename is taken from the PDF title

⚠️ Important Notice (Read This)

This project is for educational and personal use only.

Respect copyright laws

Do not use this tool to:

Violate content ownership

Redistribute paid or restricted material

The author is not responsible for misuse

✅ Use wisely. Use ethically.

🛠️ Technologies Used

JavaScript (ES6)

Canvas API

jsPDF

Browser Developer Tools
📌 Disclaimer

This repository does not bypass Google security systems.
It only processes content already rendered in your browser session.
<p align="center"> Made with ☕ & JavaScript </p> ```
