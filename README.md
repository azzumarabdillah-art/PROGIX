<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Kalkulator Kalori Harian</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:wght@500;700;800&family=Figtree:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#eef3ea; --surface:#ffffff; --ink:#18261c; --muted:#5b6b5f; --line:#cfdac8;
  --accent:#1f6b45; --accent-ink:#ffffff; --bulk:#c2570c; --cut:#1f6b45; --focus:#0a58ca;
  box-sizing:border-box; padding-top:env(safe-area-inset-top,0px); padding-bottom:env(safe-area-inset-bottom,0px);
}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
@media (prefers-color-scheme: dark){
  :root:not([data-theme="light"]){--bg:#121a15;--surface:#1b261f;--ink:#e8f0e9;--muted:#9bb0a0;--line:#2e3f33;--accent:#5fcf93;--accent-ink:#0b1a11;--bulk:#f0954f;--cut:#5fcf93;--focus:#8ab4ff}
}
:root[data-theme="dark"]{--bg:#121a15;--surface:#1b261f;--ink:#e8f0e9;--muted:#9bb0a0;--line:#2e3f33;--accent:#5fcf93;--accent-ink:#0b1a11;--bulk:#f0954f;--cut:#5fcf93;--focus:#8ab4ff}
*,*::before,*::after{box-sizing:inherit}
body{margin:0;background:var(--bg);color:var(--ink);font-family:Figtree,system-ui,-apple-system,"Segoe UI",sans-serif;line-height:1.5}
main{max-width:560px;margin:0 auto;padding:28px 18px 48px}
h1{font-family:"Bricolage Grotesque",Georgia,sans-serif;font-weight:800;font-size:clamp(1.9rem,7vw,2.6rem);line-height:1.1;margin:0 0 8px;letter-spacing:-.02em}
.lead{color:var(--muted);margin:0 0 24px}
form{background:var(--surface);border:1px solid var(--line);border-radius:14px;padding:20px;display:grid;gap:16px}
.row{display:grid;grid-template-columns:1fr 1fr;gap:14px}
label,.legend{font-weight:600;font-size:.95rem;display:block;margin-bottom:6px}
input,select{width:100%;font:inherit;color:var(--ink);background:var(--bg);border:1.5px solid var(--line);border-radius:10px;padding:11px 12px}
input:focus-visible,select:focus-visible,button:focus-visible,.goal input:focus-visible+span{outline:3px solid var(--focus);outline-offset:2px}
.hint{font-size:.82rem;color:var(--muted);margin-top:4px}
fieldset{border:0;padding:0;margin:0}
.goal{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.goal label{margin:0;position:relative;cursor:pointer}
.goal input{position:absolute;opacity:0;inset:0}
.goal span{display:block;border:1.5px solid var(--line);border-radius:10px;padding:12px;text-align:center;background:var(--bg);font-weight:600}
.goal small{display:block;font-weight:400;color:var(--muted);font-size:.8rem}
.goal input:checked+span{border-color:var(--accent);box-shadow:inset 0 0 0 1.5px var(--accent)}
button{font:inherit;font-weight:700;background:var(--accent);color:var(--accent-ink);border:0;border-radius:10px;padding:14px;cursor:pointer}
button:hover{filter:brightness(1.08)}
.err{color:#b3261e;font-weight:600;min-height:1.2em;margin:0}
@media (prefers-color-scheme: dark){:root:not([data-theme="light"]) .err{color:#ff9d94}}
#result{margin-top:22px;background:var(--surface);border:1px solid var(--line);border-radius:14px;padding:22px}
#result[hidden]{display:none}
.big{font-family:"Bricolage Grotesque",Georgia,sans-serif;font-weight:800;font-size:clamp(3rem,16vw,4.6rem);line-height:1;letter-spacing:-.03em;margin:4px 0 0}
.big small{font-size:1.1rem;font-weight:600;color:var(--muted);letter-spacing:0;margin-left:6px}
.tag{display:inline-block;font-weight:700;font-size:.9rem;padding:3px 10px;border-radius:99px;border:1.5px solid currentColor}
.tag.bulk{color:var(--bulk)} .tag.cut{color:var(--cut)}
dl{display:grid;grid-template-columns:1fr auto;gap:8px 12px;margin:18px 0 0;padding-top:16px;border-top:1px solid var(--line)}
dt{color:var(--muted)} dd{margin:0;font-weight:600;text-align:right}
.note{font-size:.82rem;color:var(--muted);margin:16px 0 0}
</style>
</head>
<body>
<main>
  <h1>Kalkulator kalori harian</h1>
  <p class="lead">Masukkan data tubuhmu dan pilih tujuan. Hasilnya adalah perkiraan kalori yang perlu kamu makan per hari.</p>

  <form id="f" novalidate>
    <div class="row">
      <div><label for="usia">Usia (tahun)</label><input id="usia" type="number" inputmode="numeric" min="15" max="80" placeholder="25"></div>
      <div><label for="bb">Berat badan (kg)</label><input id="bb" type="number" inputmode="decimal" min="30" max="250" step="0.1" placeholder="65"></div>
    </div>
    <div class="row">
      <div><label for="tb">Tinggi badan (cm)</label><input id="tb" type="number" inputmode="numeric" min="120" max="230" placeholder="170"></div>
      <div><label for="jk">Jenis kelamin</label><select id="jk"><option value="p">Laki-laki</option><option value="w">Perempuan</option></select></div>
    </div>
    <div>
      <label for="akt">Aktivitas harian</label>
      <select id="akt">
        <option value="1.2">Jarang olahraga (kerja duduk)</option>
        <option value="1.375" selected>Olahraga ringan 1–3x/minggu</option>
        <option value="1.55">Olahraga sedang 3–5x/minggu</option>
        <option value="1.725">Olahraga berat 6–7x/minggu</option>
      </select>
      <div class="hint">Tinggi, jenis kelamin, dan aktivitas ikut dihitung agar hasilnya lebih akurat.</div>
    </div>
    <fieldset>
      <div class="legend">Tujuan</div>
      <div class="goal">
        <label><input type="radio" name="goal" value="bulking" checked><span>Bulking<small>Naik berat badan</small></span></label>
        <label><input type="radio" name="goal" value="cutting"><span>Cutting / diet<small>Turun berat badan</small></span></label>
      </div>
    </fieldset>
    <p class="err" id="err" role="alert"></p>
    <button type="submit">Hitung kalori</button>
  </form>

  <section id="result" hidden aria-live="polite">
    <span class="tag" id="tag"></span>
    <p class="big"><span id="target"></span><small>kkal / hari</small></p>
    <dl>
      <dt>Kalori dasar (BMR)</dt><dd id="bmr"></dd>
      <dt>Kalori maintenance (TDEE)</dt><dd id="tdee"></dd>
      <dt id="adjLabel"></dt><dd id="adj"></dd>
      <dt>Protein (±2 g/kg)</dt><dd id="prot"></dd>
    </dl>
    <p class="note">Ini perkiraan berdasarkan rumus Mifflin-St Jeor. Pantau berat badan 2–3 minggu lalu sesuaikan 100–200 kkal bila perlu. Untuk kondisi medis, konsultasikan dengan dokter atau ahli gizi.</p>
  </section>
</main>
<script>
(function(){
  var f=document.getElementById('f'),err=document.getElementById('err'),res=document.getElementById('result');
  var fmt=function(n){return Math.round(n).toLocaleString('id-ID')};
  f.addEventListener('submit',function(e){
    e.preventDefault();
    var u=parseFloat(document.getElementById('usia').value),
        bb=parseFloat(document.getElementById('bb').value),
        tb=parseFloat(document.getElementById('tb').value),
        jk=document.getElementById('jk').value,
        akt=parseFloat(document.getElementById('akt').value),
        goal=f.elements['goal'].value;
    if(!(u>=15&&u<=80)){err.textContent='Isi usia antara 15 dan 80 tahun.';return}
    if(!(bb>=30&&bb<=250)){err.textContent='Isi berat badan antara 30 dan 250 kg.';return}
    if(!(tb>=120&&tb<=230)){err.textContent='Isi tinggi badan antara 120 dan 230 cm.';return}
    err.textContent='';
    var bmr=10*bb+6.25*tb-5*u+(jk==='p'?5:-161);
    var tdee=bmr*akt;
    var adj=goal==='bulking'?300:-500;
    var target=tdee+adj;
    var min=jk==='p'?1500:1200;
    var note=false;
    if(target<min){target=min;adj=target-tdee;note=true}
    var tag=document.getElementById('tag');
    tag.textContent=goal==='bulking'?'Bulking':'Cutting / diet';
    tag.className='tag '+(goal==='bulking'?'bulk':'cut');
    document.getElementById('target').textContent=fmt(target);
    document.getElementById('bmr').textContent=fmt(bmr)+' kkal';
    document.getElementById('tdee').textContent=fmt(tdee)+' kkal';
    document.getElementById('adjLabel').textContent=goal==='bulking'?'Surplus kalori':(note?'Defisit (dibatasi batas aman)':'Defisit kalori');
    document.getElementById('adj').textContent=(adj>0?'+':'−')+fmt(Math.abs(adj))+' kkal';
    document.getElementById('prot').textContent=fmt(bb*2)+' g / hari';
    res.hidden=false;
    res.scrollIntoView({behavior:window.matchMedia('(prefers-reduced-motion: reduce)').matches?'auto':'smooth',block:'nearest'});
  });
})();
</script>
</body>
</html>
