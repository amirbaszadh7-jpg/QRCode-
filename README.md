<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>تولیدکنندهٔ کد QR</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Vazirmatn:wght@400;600;800&display=swap" rel="stylesheet" />
<script src="https://cdn.jsdelivr.net/npm/qrcode-generator@1.4.4/qrcode.min.js"></script>
<style>
  :root{
    --bg1:#e0e7ff; --bg2:#f8fafc; --card:#fff; --text:#1f2937; --muted:#64748b;
    --primary:#4f46e5; --primary-dark:#4338ca; --border:#d3dae4; --ring:#c7d2fe; --chip:#eef2ff;
  }
  *{box-sizing:border-box}
  body{
    margin:0; min-height:100vh; padding:24px;
    display:flex; align-items:center; justify-content:center;
    background:linear-gradient(135deg,var(--bg1),var(--bg2) 55%);
    font-family:"Vazirmatn",Tahoma,"Segoe UI",sans-serif; color:var(--text);
  }
  .card{
    width:min(920px,100%); background:var(--card); border-radius:18px;
    box-shadow:0 14px 44px rgba(30,41,59,.14); padding:28px;
    display:grid; grid-template-columns:1fr 300px; gap:30px;
  }
  @media (max-width:760px){ .card{grid-template-columns:1fr} }
  h1{margin:0 0 6px; font-size:1.35rem; font-weight:800}
  .subtitle{margin:0 0 22px; color:var(--muted); font-size:.88rem}
  .field{margin-bottom:18px}
  label{display:block; font-weight:700; font-size:.9rem; margin-bottom:8px}
  legend{font-weight:700; font-size:.9rem; padding:0 8px}
  textarea{
    width:100%; min-height:76px; resize:vertical; padding:10px 12px;
    font:inherit; font-size:.98rem; border:1px solid var(--border); border-radius:10px; background:#fff;
  }
  .color-row{display:flex; gap:12px; flex-wrap:wrap}
  .color-field{flex:1; min-width:120px}
  input[type="color"]{
    width:100%; height:44px; padding:4px; background:#fff;
    border:1px solid var(--border); border-radius:10px; cursor:pointer;
  }
  .ghost-btn{
    align-self:flex-end; height:44px; padding:0 14px; background:#fff;
    border:1px solid var(--border); border-radius:10px; font:inherit; font-size:.85rem; cursor:pointer;
  }
  .ghost-btn:hover{background:var(--chip)}
  .range-row{display:flex; align-items:center; gap:12px}
  input[type="range"]{flex:1; accent-color:var(--primary); height:28px}
  output{
    font-weight:800; color:var(--primary-dark); background:var(--chip);
    border-radius:8px; padding:4px 10px; font-size:.85rem; white-space:nowrap;
  }
  fieldset{border:1px solid var(--border); border-radius:12px; padding:10px 14px 14px; margin:0 0 18px}
  .radio-row{display:flex; gap:10px; flex-wrap:wrap}
  .radio-row label{
    display:flex; align-items:center; gap:6px; margin:0; cursor:pointer;
    font-size:.85rem; font-weight:400; padding:7px 13px;
    border:1px solid var(--border); border-radius:999px; background:#fff;
  }
  .radio-row label:has(input:checked){
    background:var(--chip); border-color:var(--primary); color:var(--primary-dark); font-weight:700;
  }
  .radio-row input{accent-color:var(--primary); margin:0}
  .primary-btn{
    width:100%; padding:13px; border:none; border-radius:12px; cursor:pointer;
    background:var(--primary); color:#fff; font:inherit; font-weight:800; font-size:1rem;
  }
  .primary-btn:hover:not(:disabled){background:var(--primary-dark)}
  .primary-btn:disabled{background:#cbd5e1; cursor:not-allowed}
  .preview{display:flex; flex-direction:column; align-items:center; gap:14px}
  #qr-canvas{
    width:min(300px,100%); height:auto; border-radius:14px;
    border:1px solid #e5e7eb; box-shadow:0 6px 18px rgba(30,41,59,.10);
    image-rendering:pixelated;
  }
  #status{margin:0; min-height:3em; font-size:.8rem; line-height:1.7; color:var(--muted); text-align:center}
  #status.warn{color:#b45309; font-weight:600}
  :focus-visible{outline:3px solid var(--ring); outline-offset:2px}
</style>
</head>
<body>
<main class="card">
  <section>
    <h1>تولیدکنندهٔ کد QR</h1>
    <p class="subtitle">همزمان با تایپ، پیش‌نمایش به‌صورت زنده به‌روزرسانی می‌شود.</p>

    <div class="field">
      <label for="qr-text">متن یا آدرس (URL)</label>
      <textarea id="qr-text" dir="auto" rows="3"
        placeholder="مثلاً: https://example.com یا هر متن دلخواه"></textarea>
    </div>

    <div class="field">
      <div class="color-row">
        <div class="color-field">
          <label for="fg-color">رنگ کد (پیش‌زمینه)</label>
          <input type="color" id="fg-color" value="#111827" />
        </div>
        <div class="color-field">
          <label for="bg-color">رنگ پس‌زمینه</label>
          <input type="color" id="bg-color" value="#ffffff" />
        </div>
        <button type="button" class="ghost-btn" id="swap-colors"
          aria-label="تعویض رنگ پیش‌زمینه و پس‌زمینه">⇄ تعویض</button>
      </div>
    </div>

    <div class="field">
      <label for="qr-size">اندازهٔ خروجی</label>
      <div class="range-row">
        <input type="range" id="qr-size" min="128" max="1024" step="32" value="320" />
        <output id="size-value" for="qr-size">۳۲۰ پیکسل</output>
      </div>
    </div>

    <fieldset>
      <legend>سطح تصحیح خطا</legend>
      <div class="radio-row">
        <label><input type="radio" name="ec" value="L" /> L — کم (۷٪)</label>
        <label><input type="radio" name="ec" value="M" checked /> M — متوسط (۱۵٪)</label>
        <label><input type="radio" name="ec" value="Q" /> Q — خوب (۲۵٪)</label>
        <label><input type="radio" name="ec" value="H" /> H — زیاد (۳۰٪)</label>
      </div>
    </fieldset>

    <button type="button" class="primary-btn" id="download-btn" disabled>⬇ دانلود PNG</button>
  </section>

  <section class="preview" aria-label="پیش‌نمایش کد QR">
    <canvas id="qr-canvas" width="320" height="320" role="img" aria-label="پیش‌نمایش کد QR"></canvas>
    <p id="status" role="status" aria-live="polite"></p>
  </section>
</main>

<script>
(function () {
  'use strict';

  var textEl = document.getElementById('qr-text');
  var fgEl = document.getElementById('fg-color');
  var bgEl = document.getElementById('bg-color');
  var sizeEl = document.getElementById('qr-size');
  var sizeOut = document.getElementById('size-value');
  var swapBtn = document.getElementById('swap-colors');
  var downloadBtn = document.getElementById('download-btn');
  var canvas = document.getElementById('qr-canvas');
  var ctx = canvas.getContext('2d');
  var statusEl = document.getElementById('status');

  var libReady = typeof window.qrcode === 'function';

  // تبدیل متن به بایت‌های UTF-8 برای پشتیبانی صحیح از فارسی و سایر زبان‌ها
  if (libReady) {
    window.qrcode.stringToBytes = function (s) {
      return Array.from(new TextEncoder().encode(s));
    };
  }

  function faNum(n) { return Number(n).toLocaleString('fa-IR'); }

  function luminance(hex) {
    var n = parseInt(hex.slice(1), 16);
    return 0.2126 * ((n >> 16) & 255) + 0.7152 * ((n >> 8) & 255) + 0.0722 * (n & 255);
  }

  function setStatus(msg, warn) {
    statusEl.textContent = msg;
    statusEl.classList.toggle('warn', !!warn);
  }

  function paintEmpty(size, label) {
    ctx.fillStyle = '#f8fafc';
    ctx.fillRect(0, 0, size, size);
    ctx.strokeStyle = '#cbd5e1';
    ctx.lineWidth = Math.max(2, size / 160);
    ctx.setLineDash([8, 8]);
    ctx.strokeRect(size * 0.18, size * 0.18, size * 0.64, size * 0.64);
    ctx.setLineDash([]);
    ctx.fillStyle = '#94a3b8';
    ctx.font = Math.max(13, size * 0.045) + 'px Vazirmatn, Tahoma, sans-serif';
    ctx.textAlign = 'center';
    ctx.textBaseline = 'middle';
    ctx.fillText(label, size / 2, size / 2);
  }

  function render() {
    var size = Number(sizeEl.value);
    sizeOut.textContent = faNum(size) + ' پیکسل';
    canvas.width = size;
    canvas.height = size;

    var text = textEl.value.trim();

    if (!libReady) {
      paintEmpty(size, '⚠');
      setStatus('کتابخانهٔ QR بارگذاری نشد؛ اتصال اینترنت را بررسی و صفحه را دوباره باز کنید.', true);
      downloadBtn.disabled = true;
      return;
    }

    if (!text) {
      paintEmpty(size, 'منتظر ورودی…');
      setStatus('برای ساخت کد QR، متن یا آدرسی وارد کنید.');
      downloadBtn.disabled = true;
      canvas.setAttribute('aria-label', 'پیش‌نمایش کد QR (خالی)');
      return;
    }

    var ec = document.querySelector('input[name="ec"]:checked').value;

    try {
      var qr = window.qrcode(0, ec); // عدد ۰ یعنی تشخیص خودکار نسخه
      qr.addData(text);
      qr.make();

      var count = qr.getModuleCount();
      var quiet = 4; // حاشیهٔ سکوت استاندارد
      var cell = size / (count + quiet * 2);

      ctx.fillStyle = bgEl.value;
      ctx.fillRect(0, 0, size, size);
      ctx.fillStyle = fgEl.value;
      for (var r = 0; r < count; r++) {
        for (var c = 0; c < count; c++) {
          if (!qr.isDark(r, c)) continue;
          var x0 = Math.round((c + quiet) * cell);
          var y0 = Math.round((r + quiet) * cell);
          var x1 = Math.round((c + quiet + 1) * cell);
          var y1 = Math.round((r + quiet + 1) * cell);
          ctx.fillRect(x0, y0, x1 - x0, y1 - y0);
        }
      }

      var version = (count - 17) / 4;
      var lowContrast = Math.abs(luminance(fgEl.value) - luminance(bgEl.value)) < 60;
      var msg = 'کد QR آماده شد — نسخهٔ ' + faNum(version) + '، ماتریس ' +
                faNum(count) + '×' + faNum(count);
      if (lowContrast) msg += ' ⚠️ کنتراست رنگ‌ها کم است؛ ممکن است اسکن دشوار شود.';
      setStatus(msg, lowContrast);
      downloadBtn.disabled = false;
      canvas.setAttribute('aria-label', 'پیش‌نمایش کد QR برای: ' + text);
    } catch (err) {
      paintEmpty(size, 'خطا');
      setStatus('حجم داده بیشتر از ظرفیت کد است؛ متن را کوتاه‌تر یا سطح تصحیح خطا را کمتر کنید.', true);
      downloadBtn.disabled = true;
    }
  }

  // به‌روزرسانی زنده با کمی تأخیر برای روان بودن تایپ
  var timer;
  function scheduleRender() {
    clearTimeout(timer);
    timer = setTimeout(render, 120);
  }

  textEl.addEventListener('input', scheduleRender);
  fgEl.addEventListener('input', scheduleRender);
  bgEl.addEventListener('input', scheduleRender);
  sizeEl.addEventListener('input', scheduleRender);
  document.querySelectorAll('input[name="ec"]').forEach(function (r) {
    r.addEventListener('change', render);
  });

  swapBtn.addEventListener('click', function () {
    var t = fgEl.value;
    fgEl.value = bgEl.value;
    bgEl.value = t;
    render();
  });

  downloadBtn.addEventListener('click', function () {
    canvas.toBlob(function (blob) {
      if (!blob) return;
      var url = URL.createObjectURL(blob);
      var a = document.createElement('a');
      a.href = url;
      a.download = 'qr-code.png';
      document.body.appendChild(a);
      a.click();
      a.remove();
      setTimeout(function () { URL.revokeObjectURL(url); }, 1000);
    }, 'image/png');
  });

  render();
})();
</script>
</body>
</html>