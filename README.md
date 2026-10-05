<!doctype html>
<html lang="hi">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <meta name="description" content="PDF से कार्ड के आगे और पीछे के पेज चुनें और उन्हें A4 प्रिंट शीट पर व्यवस्थित करें।">
  <title>PVC Card · A4 Print Layout</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
  <style>
    :root{font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",sans-serif;color:#15221e;background:#f4f7f5;font-synthesis:none;--green:#087f5b;--line:#dce5e0;--muted:#64736d}
    *{box-sizing:border-box}body{margin:0}button,select,input{font:inherit}button{cursor:pointer}.top{background:#fff;border-bottom:1px solid var(--line);padding:18px max(20px,calc((100vw - 1120px)/2));display:flex;align-items:center;justify-content:space-between;gap:15px}.brand{font-weight:800;font-size:18px}.brand span{color:var(--green)}.badge{font-size:12px;color:#567067;background:#edf7f2;padding:7px 10px;border-radius:20px}.wrap{max-width:1120px;margin:36px auto;padding:0 20px 60px}.intro h1{font-size:clamp(27px,4vw,40px);letter-spacing:-1.2px;margin:0 0 8px}.intro p{color:var(--muted);margin:0 0 24px;line-height:1.6}.grid{display:grid;grid-template-columns:minmax(290px,370px) 1fr;gap:22px;align-items:start}.panel{background:white;border:1px solid var(--line);border-radius:16px;padding:20px;box-shadow:0 6px 22px #183b2b08}.panel h2{font-size:16px;margin:0 0 14px}.drop{display:block;border:1.5px dashed #9eb8ac;border-radius:13px;padding:23px 16px;text-align:center;background:#f8fbf9;cursor:pointer}.drop:hover{border-color:var(--green);background:#f2faf6}.drop strong{display:block;font-size:15px;margin:8px}.drop small{color:var(--muted)}#file{display:none}.field{margin-top:17px}.field label{display:block;font-size:13px;font-weight:650;margin-bottom:7px}.field select,.field input[type=number]{width:100%;padding:10px 11px;border:1px solid var(--line);border-radius:9px;background:white}.pair{display:grid;grid-template-columns:1fr 1fr;gap:12px}.hint{font-size:12px;color:var(--muted);line-height:1.55;margin:8px 0 0}.actions{display:flex;gap:10px;margin-top:18px}.btn{border:0;border-radius:9px;padding:11px 14px;font-weight:700}.primary{background:var(--green);color:white;flex:1}.primary:disabled{opacity:.45;cursor:not-allowed}.secondary{background:#edf2ef;color:#30463d}.status{font-size:13px;color:var(--muted);min-height:19px;margin-top:12px}.pages{display:grid;gap:9px;max-height:310px;overflow:auto;margin-top:14px}.page-row{border:1px solid var(--line);border-radius:10px;padding:9px;display:grid;grid-template-columns:22px 54px 1fr;align-items:center;gap:10px;font-size:13px}.page-row img{width:54px;height:42px;object-fit:cover;background:#eee;border-radius:5px}.page-row select{border:1px solid var(--line);border-radius:6px;padding:5px;max-width:115px}.preview-head{display:flex;align-items:center;justify-content:space-between;gap:10px;margin-bottom:12px}.preview-head h2{margin:0}.paper-wrap{background:#eef2ef;border-radius:12px;padding:16px;overflow:auto}.paper{width:min(100%,595px);aspect-ratio:210/297;background:white;margin:auto;box-shadow:0 5px 20px #16261d20;position:relative;color:#9aa7a1}.paper-label{position:absolute;top:3%;left:0;right:0;text-align:center;font-size:9px;letter-spacing:1px;color:#92a49b}.slots{position:absolute;top:8%;left:5%;width:90%;display:grid;grid-template-columns:1fr 1fr;gap:2.5%;align-items:start}.slot{aspect-ratio:85.6/54;border:1px dashed #b8c7c0;border-radius:2%;display:flex;align-items:center;justify-content:center;overflow:hidden;background:#fafcfb;font-size:12px;text-align:center}.slot img{width:100%;height:100%;object-fit:fill}.marks{position:absolute;top:calc(8% + (90% * .025));left:5%;width:90%;display:grid;grid-template-columns:1fr 1fr;gap:2.5%;font-size:8px;color:#798a81;text-align:center}.paper-foot{position:absolute;bottom:3%;width:100%;text-align:center;font-size:8px;color:#a2ada7}.note{margin:15px 0 0;background:#fff8e8;border:1px solid #f1dfb0;color:#745b20;border-radius:10px;padding:11px 13px;font-size:12px;line-height:1.55}.empty{display:grid;place-items:center;min-height:220px;color:#82928a;text-align:center;line-height:1.6}.footer{margin-top:22px;text-align:center;color:#87938d;font-size:11px}
    @media(max-width:800px){.grid{grid-template-columns:1fr}.wrap{margin-top:24px}.preview-panel{order:-1}.paper{width:min(100%,500px)}}
    @page{size:A4 portrait;margin:0}
    @media print{body{background:#fff}.top,.intro,.controls,.preview-head,.note,.footer{display:none!important}.wrap{max-width:none;margin:0;padding:0}.grid{display:block}.preview-panel{border:0;padding:0;box-shadow:none;border-radius:0}.paper-wrap{padding:0;background:white;border-radius:0;overflow:visible}.paper{width:210mm;height:297mm;aspect-ratio:auto;margin:0;box-shadow:none}.slot{border:0;border-radius:0;background:transparent}.slot:not(:has(img)){outline:1px dashed #ddd}.paper-label,.marks,.paper-foot{color:#777}}
  </style>
</head>
<body>
  <header class="top"><div class="brand">PVC <span>Studio</span></div><div class="badge">PDF आपके ब्राउज़र में प्रोसेस होती है</div></header>
  <main class="wrap">
    <section class="intro"><h1>PDF से PVC कार्ड, A4 प्रिंट शीट पर</h1><p>PDF अपलोड करें, सामने और पीछे वाले पेज चुनें, फिर A4 पर अगल-बगल रखकर प्रिंट करें।</p></section>
    <div class="grid">
      <section class="panel controls">
        <h2>1 · PDF चुनें</h2>
        <label class="drop" for="file"><span style="font-size:26px">📄</span><strong>PDF यहाँ छोड़ें या चुनने के लिए क्लिक करें</strong><small>एक PDF फ़ाइल · अधिकतम 50 MB</small></label>
        <input id="file" type="file" accept="application/pdf,.pdf">
        <div class="field"><label for="front">सामने (Front) का पेज</label><select id="front" disabled><option>पहले PDF अपलोड करें</option></select></div>
        <div class="field"><label for="back">पीछे (Back) का पेज</label><select id="back" disabled><option>पहले PDF अपलोड करें</option></select><p class="hint">अगर PDF में कई पेज हैं तो नीचे की thumbnails देखकर सही पेज चुनें। कार्ड पहचानने का अनुमान automatic है; पेज का चुनाव आप जाँचकर करें।</p></div>
        <div class="pair">
          <div class="field"><label for="width">कार्ड चौड़ाई (mm)</label><input id="width" type="number" min="30" max="150" step="0.1" value="85.6"></div>
          <div class="field"><label for="height">कार्ड ऊँचाई (mm)</label><input id="height" type="number" min="20" max="100" step="0.1" value="54"></div>
        </div>
        <div class="field"><label for="flip">डुप्लेक्स प्रिंट का पलटना</label><select id="flip"><option value="none">सामान्य (preview जैसा)</option><option value="horizontal">पीछे वाला पेज क्षैतिज पलटें</option><option value="vertical">पीछे वाला पेज लंबवत पलटें</option></select><p class="hint">प्रिंटर के duplex और कार्ड काटने/मोड़ने के तरीके के अनुसार विकल्प जाँचें।</p></div>
        <div class="actions"><button id="print" class="btn primary" disabled>प्रिंट / PDF सेव करें</button><button id="reset" class="btn secondary">रीसेट</button></div>
        <div id="status" class="status" aria-live="polite">अपलोड की गई फ़ाइल सर्वर पर नहीं भेजी जाती।</div>
        <div id="pages" class="pages"></div>
      </section>
      <section class="panel preview-panel">
        <div class="preview-head"><h2>2 · A4 शीट का preview</h2><span class="badge">A4 · Portrait</span></div>
        <div class="paper-wrap"><div class="paper" id="paper"><div class="paper-label">PVC CARD · FRONT / BACK</div><div class="slots"><div class="slot" id="frontSlot">Front यहाँ दिखेगा</div><div class="slot" id="backSlot">Back यहाँ दिखेगा</div></div><div class="marks"><span>FRONT</span><span>BACK</span></div><div class="paper-foot">100% scale पर प्रिंट करें · Fit to page बंद रखें</div></div></div>
        <div class="note"><strong>ज़रूरी:</strong> हर PDF में कार्ड की पहचान संभव नहीं—कुछ PDF में scan, कई कार्ड, password, या अलग-अलग आकार हो सकते हैं। यह टूल पेज चुनने और A4 layout बनाने में मदद करता है; सही पेज और प्रिंट आकार preview में जाँचें। वास्तविक आकार के लिए print dialog में “Actual size / 100%” चुनें। प्रिंटर का non-printable margin नतीजे को प्रभावित कर सकता है।</div>
      </section>
    </div>
    <div class="footer">PDF.js से browser-side rendering · पहचान और आकार की अंतिम जाँच उपयोगकर्ता करें</div>
  </main>
<script>
(() => {
  'use strict';
  const fileInput=document.getElementById('file'), frontSelect=document.getElementById('front'), backSelect=document.getElementById('back');
  const statusEl=document.getElementById('status'), pagesEl=document.getElementById('pages'), printBtn=document.getElementById('print');
  let pdfDoc=null, images=[], sourceUrl=null, loadId=0;
  if(!window.pdfjsLib){statusEl.textContent='PDF लाइब्रेरी लोड नहीं हुई। इंटरनेट कनेक्शन जाँचकर पेज reload करें।';return;}
  pdfjsLib.GlobalWorkerOptions.workerSrc='https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';
  const label=(i)=>`पेज ${i+1}`;
  function options(){
    for(const sel of [frontSelect,backSelect]){const old=sel.value;sel.replaceChildren();images.forEach((_,i)=>{const op=document.createElement('option');op.value=String(i);op.textContent=label(i);sel.append(op);});if(images.length>1){const op=document.createElement('option');op.value='';op.textContent='चयन नहीं';sel.append(op);}if([...sel.options].some(o=>o.value===old))sel.value=old;}
    frontSelect.value='0';backSelect.value=images.length>1?'1':'';frontSelect.disabled=false;backSelect.disabled=false;updatePreview();
  }
  function updatePreview(){
    const f=Number(frontSelect.value),b=backSelect.value===''?-1:Number(backSelect.value);
    for(const [slot,index] of [[document.getElementById('frontSlot'),f],[document.getElementById('backSlot'),b]]){
      slot.replaceChildren();if(index>=0&&images[index]){const img=document.createElement('img');img.src=images[index];img.alt=index===f?'Front card page':'Back card page';slot.append(img);}else slot.textContent='पेज नहीं चुना';
    }
    const w=Math.min(150,Math.max(30,Number(document.getElementById('width').value)||85.6)),h=Math.min(100,Math.max(20,Number(document.getElementById('height').value)||54));
    document.querySelectorAll('.slot').forEach(s=>s.style.aspectRatio=`${w}/${h}`);
    const flip=document.getElementById('flip').value;const backImg=document.querySelector('#backSlot img');if(backImg)backImg.style.transform=flip==='horizontal'?'scaleX(-1)':flip==='vertical'?'scaleY(-1)':'';
    printBtn.disabled=!pdfDoc||f<0||!images[f];
  }
  async function loadFile(file){
    const id=++loadId;if(!file)return;
    if(file.type!=='application/pdf'&&!file.name.toLowerCase().endsWith('.pdf')){statusEl.textContent='कृपया PDF फ़ाइल चुनें।';return;}
    if(file.size>50*1024*1024){statusEl.textContent='फ़ाइल 50 MB से बड़ी है। छोटी PDF चुनें।';return;}
    statusEl.textContent='PDF पढ़ रहे हैं…';printBtn.disabled=true;pagesEl.replaceChildren();images=[];
    try{
      const data=await file.arrayBuffer();if(id!==loadId)return;
      pdfDoc=await pdfjsLib.getDocument({data}).promise;if(id!==loadId)return;
      if(pdfDoc.numPages>100){pdfDoc.destroy();pdfDoc=null;statusEl.textContent='PDF में 100 से अधिक पेज हैं। कृपया संबंधित पेज वाली छोटी PDF चुनें।';return;}
      const count=pdfDoc.numPages;
      for(let n=1;n<=count;n++){
        if(id!==loadId)return;
        statusEl.textContent=`पेज render हो रहे हैं… ${n}/${count}`;
        const page=await pdfDoc.getPage(n), viewport=page.getViewport({scale:Math.min(1.6,900/page.getViewport({scale:1}).width)});
        const canvas=document.createElement('canvas'),ctx=canvas.getContext('2d',{alpha:false});canvas.width=Math.ceil(viewport.width);canvas.height=Math.ceil(viewport.height);
        await page.render({canvasContext:ctx,viewport}).promise;images.push(canvas.toDataURL('image/jpeg',.9));
        const row=document.createElement('div');row.className='page-row';const check=document.createElement('input');check.type='checkbox';check.setAttribute('aria-label',`${label(n-1)} चुनें`);const thumb=document.createElement('img');thumb.src=images[n-1];thumb.alt=`${label(n-1)} thumbnail`;const text=document.createElement('span');text.textContent=label(n-1);row.append(check,thumb,text);pagesEl.append(row);
        check.addEventListener('change',()=>{if(check.checked){if(frontSelect.value==='')frontSelect.value=String(n-1);else if(backSelect.value==='')backSelect.value=String(n-1);updatePreview();}});
      }
      options();statusEl.textContent=`${count} पेज लोड हुए। thumbnails देखकर Front और Back चुनें।`;
    }catch(err){pdfDoc=null;statusEl.textContent=err?.name==='PasswordException'?'यह PDF पासवर्ड से सुरक्षित है; पहले बिना पासवर्ड वाली PDF export करें।':'PDF नहीं पढ़ी जा सकी। फ़ाइल सही/खुली हुई है या नहीं जाँचें।';console.error(err);}
  }
  fileInput.addEventListener('change',e=>loadFile(e.target.files?.[0]));
  for(const el of [frontSelect,backSelect,document.getElementById('width'),document.getElementById('height'),document.getElementById('flip')])el.addEventListener('input',updatePreview);
  document.querySelector('.drop').addEventListener('dragover',e=>{e.preventDefault();});
  document.querySelector('.drop').addEventListener('drop',e=>{e.preventDefault();const f=e.dataTransfer.files?.[0];if(f)loadFile(f);});
  printBtn.addEventListener('click',()=>window.print());
  document.getElementById('reset').addEventListener('click',()=>{loadId++;pdfDoc?.destroy();pdfDoc=null;images=[];fileInput.value='';pagesEl.replaceChildren();frontSelect.replaceChildren(new Option('पहले PDF अपलोड करें'));backSelect.replaceChildren(new Option('पहले PDF अपलोड करें'));frontSelect.disabled=backSelect.disabled=true;document.getElementById('frontSlot').textContent='Front यहाँ दिखेगा';document.getElementById('backSlot').textContent='Back यहाँ दिखेगा';printBtn.disabled=true;statusEl.textContent='अपलोड की गई फ़ाइल सर्वर पर नहीं भेजी जाती।';});
})();
</script>
</body>
</html>
