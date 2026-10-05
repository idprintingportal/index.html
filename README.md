<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aadhaar PVC Card Auto Cutter & A4 Layout</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- PDF.js CDN -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.min.js"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">

    <div class="max-w-4xl mx-auto bg-white shadow-lg rounded-xl p-6">
        <h1 class="text-2xl font-bold text-blue-600 mb-4 text-center">Aadhaar PVC Auto Cutter Portal</h1>
        
        <!-- File Upload Section -->
        <div class="mb-6 border-2 border-dashed border-blue-300 rounded-lg p-6 text-center bg-blue-50">
            <input type="file" id="pdfFile" accept="application/pdf" class="block w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-blue-600 file:text-white hover:file:bg-blue-700 cursor-pointer">
            <p class="text-xs text-gray-500 mt-2">अपना ओरिजिनल आधार पीडीएफ (Password Protected नहीं होना चाहिए) यहाँ अपलोड करें।</p>
        </div>

        <!-- Preview & Action Section -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 items-center">
            <div class="text-center">
                <h3 class="font-semibold text-gray-700 mb-2">Original PDF Preview</h3>
                <canvas id="pdfCanvas" class="border rounded shadow max-w-full mx-auto"></canvas>
            </div>
            <div class="text-center">
                <h3 class="font-semibold text-gray-700 mb-2">Cropped PVC Card (A4 Layout)</h3>
                <canvas id="cropCanvas" class="border rounded shadow max-w-full mx-auto bg-white"></canvas>
                <button onclick="printA4Sheet()" class="mt-4 bg-green-600 text-white px-6 py-2 rounded-lg font-semibold hover:bg-green-700 transition">A4 प्रिंट / डाउनलोड करें</button>
            </div>
        </div>
    </div>

    <script>
        pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.worker.min.js';

        const pdfFileInput = document.getElementById('pdfFile');
        const pdfCanvas = document.getElementById('pdfCanvas');
        const cropCanvas = document.getElementById('cropCanvas');
        const ctxPdf = pdfCanvas.getContext('2d');
        const ctxCrop = cropCanvas.getContext('2d');

        pdfFileInput.addEventListener('change', async function(e) {
            const file = e.target.files[0];
            if (!file) return;

            const fileReader = new FileReader();
            fileReader.onload = async function() {
                const typedarray = new Uint8Array(this.result);
                const pdf = await pdfjsLib.getDocument(typedarray).promise;
                const page = await pdf.getPage(1);

                const viewport = page.getViewport({ scale: 1.5 });
                pdfCanvas.height = viewport.height;
                pdfCanvas.width = viewport.width;

                await page.render({ canvasContext: ctxPdf, viewport: viewport }).promise;

                // Auto Crop Logic (Standard Aadhaar Letter PVC section coordinates)
                // Note: आप अपने आधार पीडीएफ लेआउट के अनुसार इन कोऑर्डिनेट्स (X, Y, Width, Height) को एडजस्ट कर सकते हैं।
                setTimeout(() => {
                    extractPVCCard(pdfCanvas);
                }, 200);
            };
            fileReader.readAsArrayBuffer(file);
        });

        function extractPVCCard(sourceCanvas) {
            // उदाहरण के लिए आधार के निचले हिस्से या तय कट-आउट एरिया को क्रॉप करना
            const startX = sourceCanvas.width * 0.05;
            const startY = sourceCanvas.height * 0.45; // आधार लेटर में कार्ड आमतौर पर बीच/नीचे होता है
            const cropWidth = sourceCanvas.width * 0.9;
            const cropHeight = sourceCanvas.height * 0.35;

            cropCanvas.width = 600;  // High resolution for print
            cropCanvas.height = 380;

            ctxCrop.clearRect(0, 0, cropCanvas.width, cropCanvas.height);
            ctxCrop.drawImage(
                sourceCanvas, 
                startX, startY, cropWidth, cropHeight, 
                0, 0, cropCanvas.width, cropCanvas.height
            );
        }

        function printA4Sheet() {
            const dataUrl = cropCanvas.toDataURL();
            const printWindow = window.open('', '_blank');
            printWindow.document.write(`
                <html>
                <head>
                    <title>A4 Aadhaar Print</title>
                    <style>
                        body { margin: 0; display: flex; justify-content: center; align-items: center; height: 100vh; }
                        .card-container { width: 85.6mm; height: 54mm; border: 1px dashed #ccc; padding: 2mm; }
                        img { width: 100%; height: 100% object-fit: contain; }
                        @media print { body { border: none; } }
                    </style>
                </head>
                <body>
                    <div class="card-container">
                        <img src="${dataUrl}" />
                    </div>
                    <script>
                        window.onload = function() { window.print(); window.close(); }
                    </script>
                </body>
                </html>
            `);
            printWindow.document.close();
        }
    </script>
</body>
</html>
