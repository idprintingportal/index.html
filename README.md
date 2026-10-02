<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>e-PAN Real-Time PDF Editor</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
    body { display: flex; height: 100vh; background-color: #1e1e2e; color: #cdd6f4; overflow: hidden; }
    
    /* LEFT PANEL: LIVE PDF PREVIEW */
    #left-panel {
      flex: 1;
      padding: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-start;
      overflow: auto;
      background: #181825;
      border-right: 2px solid #313244;
    }
    #left-panel h2 { margin-bottom: 15px; font-size: 18px; color: #89b4fa; }
    #canvas-container {
      position: relative;
      background: #ffffff;
      box-shadow: 0 8px 24px rgba(0,0,0,0.5);
      border-radius: 8px;
      overflow: hidden;
    }
    canvas { display: block; }

    /* RIGHT PANEL: 7 INPUT BOXES */
    #right-panel {
      width: 400px;
      background: #1e1e2e;
      padding: 20px;
      overflow-y: auto;
    }
    #right-panel h2 { margin-bottom: 15px; font-size: 18px; color: #a6e3a1; border-bottom: 1px solid #45475a; padding-bottom: 8px; }
    
    .section-title { font-size: 14px; font-weight: bold; color: #f9e2af; margin: 15px 0 10px 0; }
    
    .input-group { margin-bottom: 12px; }
    .input-group label { display: block; font-weight: 600; margin-bottom: 5px; font-size: 13px; color: #bac2de; }
    .input-group input, .input-group select {
      width: 100%;
      padding: 10px;
      background: #313244;
      border: 1px solid #45475a;
      border-radius: 6px;
      color: #cdd6f4;
      font-size: 13px;
      outline: none;
      transition: 0.2s;
    }
    .input-group input:focus, .input-group select:focus {
      border-color: #89b4fa;
    }
    
    .file-input-wrapper input[type="file"] {
      padding: 6px;
      background: #181825;
      cursor: pointer;
    }

    button {
      width: 100%;
      padding: 14px;
      background-color: #a6e3a1;
      color: #11111b;
      border: none;
      border-radius: 6px;
      font-size: 15px;
      cursor: pointer;
      font-weight: bold;
      margin-top: 15px;
      transition: 0.2s;
    }
    button:hover { background-color: #94e2d5; }
  </style>

  <!-- PDF.js & pdf-lib Libraries -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.min.js"></script>
  <script src="https://unpkg.com/pdf-lib@1.17.1/dist/pdf-lib.min.js"></script>
</head>
<body>

  <!-- LEFT PANEL: REAL-TIME PDF PREVIEW -->
  <div id="left-panel">
    <h2>Real-Time PDF Preview</h2>
    <div id="canvas-container">
      <canvas id="pdf-canvas"></canvas>
    </div>
  </div>

  <!-- RIGHT PANEL: 7 EDITING INPUT BOXES -->
  <div id="right-panel">
    <h2>Controls & Input Fields</h2>

    <div class="input-group file-input-wrapper">
      <label for="pdf-upload">📄 PDF File Select Karein:</label>
      <input type="file" id="pdf-upload" accept="application/pdf">
    </div>

    <div class="section-title">7 Editing Fields:</div>
    
    <!-- 1. PAN Number -->
    <div class="input-group">
      <label for="input-pan">1. PAN Number Box:</label>
      <input type="text" id="input-pan" placeholder="ABCDE1234G">
    </div>

    <!-- 2. Name -->
    <div class="input-group">
      <label for="input-name">2. Name Box:</label>
      <input type="text" id="input-name" placeholder="FIRST MIDDLE LAST">
    </div>

    <!-- 3. Father's Name -->
    <div class="input-group">
      <label for="input-fname">3. Father's Name Box:</label>
      <input type="text" id="input-fname" placeholder="FIRST MIDDLE LAST">
    </div>

    <!-- 4. DOB -->
    <div class="input-group">
      <label for="input-dob">4. Date of Birth Box:</label>
      <input type="text" id="input-dob" placeholder="DD/MM/YYYY">
    </div>

    <!-- 5. Gender -->
    <div class="input-group">
      <label for="input-gender">5. Gender Box:</label>
      <select id="input-gender">
        <option value="">Select Gender</option>
        <option value="Male">Male</option>
        <option value="Female">Female</option>
      </select>
    </div>

    <!-- 6. Photo Upload -->
    <div class="input-group file-input-wrapper">
      <label for="input-photo">6. Photo Box (Upload Image):</label>
      <input type="file" id="input-photo" accept="image/*">
    </div>

    <!-- 7. Signature Upload -->
    <div class="input-group file-input-wrapper">
      <label for="input-signature">7. Signature Box (Upload Image):</label>
      <input type="file" id="input-signature" accept="image/*">
    </div>

    <button id="download-btn">⬇️ Final Edited PDF Download</button>
  </div>

  <script>
    pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.worker.min.js';

    let pdfBytesOriginal = null;
    let pdfDocJs = null;
    let pageCanvas = document.getElementById('pdf-canvas');
    let ctx = pageCanvas.getContext('2d');

    let photoImg = null;
    let sigImg = null;

    // Layout Coordinates
    const coords = {
      pan: { x: 75, y: 155 },
      name: { x: 75, y: 185 },
      fname: { x: 75, y: 215 },
      dob: { x: 75, y: 245 },
      gender: { x: 175, y: 245 },
      photo: { x: 375, y: 145, width: 90, height: 110 },
      signature: { x: 75, y: 268, width: 120, height: 35 }
    };

    // PDF Upload
    document.getElementById('pdf-upload').addEventListener('change', async function(e) {
      const file = e.target.files[0];
      if (!file) return;

      pdfBytesOriginal = await file.arrayBuffer();
      pdfDocJs = await pdfjsLib.getDocument({ data: pdfBytesOriginal }).promise;
      renderPage();
    });

    // Photo Upload Reader
    document.getElementById('input-photo').addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function(evt) {
          photoImg = new Image();
          photoImg.src = evt.target.result;
          photoImg.onload = () => renderPage();
        };
        reader.readAsDataURL(file);
      }
    });

    // Signature Upload Reader
    document.getElementById('input-signature').addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function(evt) {
          sigImg = new Image();
          sigImg.src = evt.target.result;
          sigImg.onload = () => renderPage();
        };
        reader.readAsDataURL(file);
      }
    });

    // Render Canvas with PDF Page & Overlays
    async function renderPage() {
      if (!pdfDocJs) return;

      const page = await pdfDocJs.getPage(1);
      const viewport = page.getViewport({ scale: 1.5 });

      pageCanvas.width = viewport.width;
      pageCanvas.height = viewport.height;

      await page.render({ canvasContext: ctx, viewport: viewport }).promise;
      drawLiveOverlays();
    }

    // Real-Time Drawing on Left Canvas
    function drawLiveOverlays() {
      ctx.font = 'bold 15px Arial';
      ctx.fillStyle = '#000000';

      const pan = document.getElementById('input-pan').value;
      const name = document.getElementById('input-name').value;
      const fname = document.getElementById('input-fname').value;
      const dob = document.getElementById('input-dob').value;
      const gender = document.getElementById('input-gender').value;

      if (pan) ctx.fillText(pan, coords.pan.x * 1.5, coords.pan.y * 1.5);
      if (name) ctx.fillText(name, coords.name.x * 1.5, coords.name.y * 1.5);
      if (fname) ctx.fillText(fname, coords.fname.x * 1.5, coords.fname.y * 1.5);
      if (dob) ctx.fillText(dob, coords.dob.x * 1.5, coords.dob.y * 1.5);
      if (gender) ctx.fillText(gender, coords.gender.x * 1.5, coords.gender.y * 1.5);

      if (photoImg) {
        ctx.drawImage(photoImg, coords.photo.x * 1.5, coords.photo.y * 1.5, coords.photo.width * 1.5, coords.photo.height * 1.5);
      }
      if (sigImg) {
        ctx.drawImage(sigImg, coords.signature.x * 1.5, coords.signature.y * 1.5, coords.signature.width * 1.5, coords.signature.height * 1.5);
      }
    }

    // Input Listeners for Instant Live Update
    document.querySelectorAll('#right-panel input, #right-panel select').forEach(elem => {
      elem.addEventListener('input', () => renderPage());
    });

    // Final PDF Generation
    document.getElementById('download-btn').addEventListener('click', async () => {
      if (!pdfBytesOriginal) {
        alert("Pehle PDF Upload Karein!");
        return;
      }

      const { PDFDocument, rgb } = PDFLib;
      const pdfDoc = await PDFDocument.load(pdfBytesOriginal);
      const pages = pdfDoc.getPages();
      const firstPage = pages[0];
      const { height } = firstPage.getSize();

      const pan = document.getElementById('input-pan').value;
      const name = document.getElementById('input-name').value;
      const fname = document.getElementById('input-fname').value;
      const dob = document.getElementById('input-dob').value;
      const gender = document.getElementById('input-gender').value;

      if (pan) firstPage.drawText(pan, { x: coords.pan.x, y: height - coords.pan.y, size: 11, color: rgb(0, 0, 0) });
      if (name) firstPage.drawText(name, { x: coords.name.x, y: height - coords.name.y, size: 11, color: rgb(0, 0, 0) });
      if (fname) firstPage.drawText(fname, { x: coords.fname.x, y: height - coords.fname.y, size: 11, color: rgb(0, 0, 0) });
      if (dob) firstPage.drawText(dob, { x: coords.dob.x, y: height - coords.dob.y, size: 11, color: rgb(0, 0, 0) });
      if (gender) firstPage.drawText(gender, { x: coords.gender.x, y: height - coords.gender.y, size: 11, color: rgb(0, 0, 0) });

      const photoFileInput = document.getElementById('input-photo').files[0];
      if (photoFileInput) {
        const photoArrayBuffer = await photoFileInput.arrayBuffer();
        let photoImagePdf = photoFileInput.type.includes('png') ? await pdfDoc.embedPng(photoArrayBuffer) : await pdfDoc.embedJpg(photoArrayBuffer);
        firstPage.drawImage(photoImagePdf, {
          x: coords.photo.x,
          y: height - coords.photo.y - coords.photo.height,
          width: coords.photo.width,
          height: coords.photo.height,
        });
      }

      const sigFileInput = document.getElementById('input-signature').files[0];
      if (sigFileInput) {
        const sigArrayBuffer = await sigFileInput.arrayBuffer();
        let sigImagePdf = sigFileInput.type.includes('png') ? await pdfDoc.embedPng(sigArrayBuffer) : await pdfDoc.embedJpg(sigArrayBuffer);
        firstPage.drawImage(sigImagePdf, {
          x: coords.signature.x,
          y: height - coords.signature.y - coords.signature.height,
          width: coords.signature.width,
          height: coords.signature.height,
        });
      }

      const pdfBytesEdited = await pdfDoc.save();
      const blob = new Blob([pdfBytesEdited], { type: 'application/pdf' });
      const link = document.createElement('a');
      link.href = URL.createObjectURL(blob);
      link.download = 'Edited_ePAN.pdf';
      link.click();
    });
  </script>
</body>
</html>
