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

### 3️⃣ Open Developer Console or Inspect element
- Press:
  - **Windows / Linux:** `Ctrl + Shift + I`
  - **Mac:** `Cmd + Option + I`
  - **Or:** `F12`
- Go to the **Console** tab

> ⚠️ **In Console:**  
> Type <p>allow pasting</p> and press ENTER, if you are using Chrome and cannot paste any code directly in the console tab,:
> Copy and paste script below on console tab,
```js

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

      // specific check for Google Drive blob images
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

      // Determine orientation and dimensions for THIS specific image
      let orientation = img.naturalWidth > img.naturalHeight ? "l" : "p";
      let pageWidth = img.naturalWidth;
      let pageHeight = img.naturalHeight;

      if (i === 0) {
        // Initialize PDF with the dimensions of the FIRST image
        pdf = new jsPDF({
          orientation: orientation,
          unit: "px",
          format: [pageWidth, pageHeight],
        });
      } else {
        // For subsequent images, add a new page with THAT image's specific dimensions
        pdf.addPage([pageWidth, pageHeight], orientation);
      }

      // Add the image to the current page
      pdf.addImage(imgData, "PNG", 0, 0, pageWidth, pageHeight, "", "SLOW");

      const percentages = Math.floor(((i + 1) / validImgs.length) * 100);
      console.log(`Processing content ${percentages}%`);
    }

    // Check if title contains .pdf in end of the title
    // Use optional chaining to avoid errors if the meta tag isn't present.
    // Fall back to document.title when necessary. Note: if the PDF is inside a cross-origin iframe,
    // parent scripts cannot access the iframe document due to same-origin policy.
    let title = document.querySelector('meta[itemprop="name"]')?.content || document.title || 'download.pdf';
    if ((title.split(".").pop() || "").toLowerCase() !== "pdf") {
      title = title + ".pdf";
    }

    // Download the generated PDF.
    console.log("Downloading PDF file ...");
    pdf.save(title, { returnPromise: true }).then(() => {
      document.body.removeChild(script);
      console.log("PDF downloaded!");
    });
  };

  // Load the jsPDF library using the trusted URL.
  let scriptURL = "https://unpkg.com/jspdf@latest/dist/jspdf.umd.min.js";
  let trustedURL;
  if (window.trustedTypes && trustedTypes.createPolicy) {
    const policy = trustedTypes.createPolicy("myPolicy", {
      createScriptURL: (input) => {
        return input;
      },
    });
    trustedURL = policy.createScriptURL(scriptURL);
  } else {
    trustedURL = scriptURL;
  }

  script.src = trustedURL;
  document.body.appendChild(script);
})();
```

### 4️⃣ Paste the Script

* Copy the script
* Paste it into the console
* Press **ENTER**

### 5️⃣ Wait & Download

* The console will show progress
* PDF will download automatically when finished 🎉

---

## 🧠 Script Behavior Notes

* ⏳ Processing speed depends on:

  * Number of pages
  * Page resolution
  * Device performance
* 📐 Each page keeps its **original width, height, and orientation**
* 📝 Output filename is taken from the PDF title

---

## ⚠️ Important Notice (Read This)

> **This project is for educational and personal use only.**

* Respect **copyright laws**
* Do **not** use this tool to:

  * Violate content ownership
  * Redistribute paid or restricted material
* The author is **not responsible** for misuse

✅ **Use wisely. Use ethically.**

---

## 🛠️ Technologies Used

* **JavaScript (ES6)**
* **Canvas API**
* **jsPDF**
* **Browser Developer Tools**

---

## 📌 Disclaimer

This repository does **not** bypass Google security systems.
It only processes **content already rendered in your browser session**.

---

<p align="center">
  Made with ☕ & JavaScript  
</p>


