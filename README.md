<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Universal ID Card PVC & A4 Auto Arranger</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- PDF.js CDN -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.min.js"></script>
    <style>
        @media print {
            body * {
                visibility: hidden;
            }
            #printableA4, #printableA4 * {
                visibility: visible;
            }
            #printableA4 {
                position: absolute;
                left: 0;
                top: 0;
                width: 210mm;
                height: 297mm;
                margin: 0;
                padding: 10mm;
                background: white;
            }
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen p-4 md:p-8">

    <div class="max-w-4xl mx-auto bg-white shadow-xl rounded-2xl p-6 md:p-8">
        <h1 class="text-2xl md:text-3xl font-extrabold text-indigo-600 mb-2 text-center">Universal ID Card Auto Cutter & A4 Arranger</h1>
        <p class="text-gray-500 text-center mb-6 text-sm">ओरिजिनल आईडी पीडीएफ अपलोड करें, यह ऑटोमैटिक पीवीसी कार्ड फॉर्मेट में क्रॉप होकर A4 शीट पर सेट हो जाएगा।</p>
        
        <!-- File Upload Section -->
        <div class="mb-6 border-2 border-dashed border-indigo-300 rounded-xl p-6 text-center bg-indigo-50 hover:bg-indigo-100 transition">
            <input type="file" id="pdfFile" accept="application/pdf" class="block w-full text-sm text-gray-500 file:mr-4 file:py-2.5 file:px-6 file:rounded-xl file:border-0 file:text-sm file:font-semibold file:bg-indigo-600 file:text-white hover:file:bg-indigo-700 cursor-pointer">
            <p class="text-xs text-gray-500 mt-2">समर्थित फॉर्मेट: PDF (Ensure it is not password protected)</p>
        </div>

        <!-- Preview Section -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
            <div class="bg-gray-50 p-4 rounded-xl border text-center">
                <h3 class="font-semibold text-gray-700 mb-2">Original PDF Preview</h3>
                <div class="overflow-auto max-h-96 flex justify-center">
                    <canvas id="pdfCanvas" class="border rounded shadow-sm max-w-full"></canvas>
                </div>
            </div>
            <div class="bg-gray-50 p-4 rounded-xl border text-center">
                <h3 class="font-semibold text-gray-700 mb-2">Cropped PVC Card Preview</h3>
                <div class="overflow-auto max-h-96 flex justify-center items-center bg-white p-2 border rounded">
                    <canvas id="cropCanvas" class="shadow-sm max-w-full"></canvas>
                </div>
            </div>
        </div>

        <!-- Action Buttons -->
        <div class="flex flex-wrap justify-center gap-4">
            <button onclick="processAndArrange()" id="processBtn" disabled class="bg-indigo-600 text-white px-6 py-3 rounded-xl font-semibold hover:bg-indigo-700 transition disabled:opacity-50 disabled:cursor-not-allowed">
                कार्रवाई करें & A4 पर सेट करें
            </button>
            <button onclick="window.print()" class="bg-emerald-600 text-white px-6 py-3 rounded-xl font-semibold hover:bg-emerald-700 transition flex items-center gap-2">
                🖨️ A4 प्रिंट / PDF डाउनलोड करें
            </button>
        </div>

        <!-- Hidden A4 Print Layout Container -->
        <div id="printableA4" class="hidden mt-8 p-4 bg-white border border-gray-300 mx-auto" style="width: 210mm; min-height: 297mm; box-sizing: border-box;">
            <h2 class="text-center text-sm font-bold text-gray-400 mb-4 uppercase tracking-wider">--- PVC Card Print Layout (A4 Sheet) ---</h2>
            <div id="a4CardGrid" class="flex flex-col gap-4 items-center">
                <!-- Dynamically injected cropped cards for A4 grid -->
            </div>
        </div>
    </div>

    <script>
        pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.worker.min.js';

        const pdfFileInput = document.getElementById('pdfFile');
        const pdfCanvas = document.getElementById('pdfCanvas');
        const cropCanvas = document.getElementById('cropCanvas');
        const processBtn = document.getElementById('processBtn');
        const a4CardGrid = document.getElementById('a4CardGrid');

        let loadedPDFPage = null;
        let sourceViewport = null;

        pdfFileInput.addEventListener('change', async function(e) {
            const file = e.target.files[0];
            if (!file) return;

            const fileReader = new FileReader();
            fileReader.onload = async function() {
                try {
                    const typedarray = new Uint8Array(this.result);
                    const pdf = await pdfjsLib.getDocument(typedarray).promise;
                    loadedPDFPage = await pdf.getPage(1);

                    sourceViewport = loadedPDFPage.getViewport({ scale: 1.5 });
                    pdfCanvas.height = sourceViewport.height;
                    pdfCanvas.width = sourceViewport.width;

                    const ctxPdf = pdfCanvas.getContext('2d');
                    await loadedPDFPage.render({ canvasContext: ctxPdf, viewport: sourceViewport }).promise;

                    processBtn.disabled = false;
                } catch (error) {
                    alert('पीडीएफ लोड करने में त्रुटि: कृपया सही और अनलॉक्ड पीडीएफ अपलोड करें।');
                }
            };
            fileReader.readAsArrayBuffer(file);
        });

        async function processAndArrange() {
            if (!loadedPDFPage) return;

            // स्टैंडर्ड पीवीसी कार्ड आस्पेक्ट रेशियो (85.6mm x 54mm) के हिसाब से क्रॉप कोऑर्डिनेट्स सेट करें
            // आप अपने डॉक्यूमेंट लेआउट के अनुसार इन्हें एडजस्ट कर सकते हैं
            const startX = sourceViewport.width * 0.05;
            const startY = sourceViewport.height * 0.42; 
            const cropWidth = sourceViewport.width * 0.9;
            const cropHeight = sourceViewport.height * 0.38;

            cropCanvas.width = 1011; // Standard high-res width for PVC printing (~300 DPI)
            cropCanvas.height = 638;  // Standard high-res height for PVC printing

            const ctxCrop = cropCanvas.getContext('2d');
            ctxCrop.clearRect(0, 0, cropCanvas.width, cropCanvas.height);
            ctxCrop.drawImage(
                pdfCanvas, 
                startX, startY, cropWidth, cropHeight, 
                0, 0, cropCanvas.width, cropCanvas.height
            );

            // A4 लेआउट ग्रिड में क्रॉप की गई इमेज को डालें
            const dataUrl = cropCanvas.toDataURL('image/png');
            a4CardGrid.innerHTML = `
                <div style="width: 85.6mm; height: 54mm; border: 1px dashed #999; padding: 2mm; box-sizing: border-box; display: flex; justify-content: center; align-items: center; background: #fff;">
                    <img src="${dataUrl}" style="width: 100%; height: 100%; object-fit: contain;" />
                </div>
                <div style="width: 85.6mm; height: 54mm; border: 1px dashed #999; padding: 2mm; box-sizing: border-box; display: flex; justify-content: center; align-items: center; background: #fff; margin-top: 5mm;">
                    <img src="${dataUrl}" style="width: 100%; height: 100%; object-fit: contain;" />
                </div>
            `;
            
            alert('आईडी कार्ड सफलतापूर्वक क्रॉप होकर A4 लेआउट में सेट हो गया है! अब आप प्रिंट बटन दबा सकते हैं।');
        }
    </script>
</body>
</html>
