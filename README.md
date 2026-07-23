<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>Skanepa Bakery</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,600;9..144,700;9..144,900&family=Work+Sans:wght@400;500;600;700&family=IBM+Plex+Mono:wght@500;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.8.2/jspdf.plugin.autotable.min.js"></script>
<style>
  :root{
    --bg:#FBF3E3;
    --surface:#FFFFFF;
    --ink:#2B1B12;
    --ink-soft:#6B5A4C;
    --crust:#A8481D;
    --crust-dark:#7F3413;
    --wheat:#D9A441;
    --wheat-soft:#F1DDA8;
    --sage:#55694A;
    --sage-soft:#E1E9D6;
    --line:#EADFC8;
    --danger:#B33A3A;
    --radius:16px;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:var(--bg);
    color:var(--ink);
    font-family:'Work Sans',sans-serif;
    -webkit-font-smoothing:antialiased;
    min-height:100vh;
  }
  #app{
    max-width:480px;
    margin:0 auto;
    min-height:100vh;
    background:var(--bg);
    display:flex;
    flex-direction:column;
    position:relative;
    padding-bottom:78px;
  }
  .topbar{
    padding:22px 20px 16px;
    background:linear-gradient(180deg,#3A2214 0%,#2B1B12 100%);
    color:#F6EAD4;
    position:sticky;
    top:0;
    z-index:20;
  }
  .topbar .brand{
    display:flex;
    align-items:baseline;
    gap:8px;
  }
  .topbar .brand-mark{
    font-family:'Fraunces',serif;
    font-weight:900;
    font-size:23px;
    letter-spacing:0.3px;
  }
  .topbar .brand-sub{
    font-family:'IBM Plex Mono',monospace;
    font-size:10.5px;
    letter-spacing:2px;
    text-transform:uppercase;
    color:var(--wheat);
    margin-top:2px;
  }
  main{flex:1;padding:18px 16px 12px;}
  .view{display:none;}
  .view.active{display:block;animation:fadeIn .25s ease;}
  @keyframes fadeIn{from{opacity:0;transform:translateY(6px);}to{opacity:1;transform:translateY(0);}}

  .card{
    background:var(--surface);
    border:1px solid var(--line);
    border-radius:var(--radius);
    padding:16px;
    margin-bottom:14px;
  }
  .card h3{
    font-family:'Fraunces',serif;
    font-size:14.5px;
    font-weight:700;
    margin:0 0 12px;
    color:var(--crust-dark);
    text-transform:uppercase;
    letter-spacing:0.6px;
  }
  label{
    display:block;
    font-size:12.5px;
    font-weight:600;
    color:var(--ink-soft);
    margin-bottom:5px;
  }
  input[type=text],input[type=number],input[type=date],select{
    width:100%;
    padding:11px 12px;
    border-radius:10px;
    border:1.5px solid var(--line);
    background:#FFFEFB;
    font-family:'Work Sans',sans-serif;
    font-size:15px;
    color:var(--ink);
    margin-bottom:12px;
  }
  input:focus,select:focus{outline:2px solid var(--wheat);border-color:var(--wheat);}
  .row2{display:grid;grid-template-columns:1fr 1fr;gap:10px;}
  .row2 > div > label{white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}

  .admin-name-display{
    font-family:'Fraunces',serif;
    font-weight:900;
    font-size:26px;
    color:var(--crust-dark);
    text-align:center;
    padding:6px 0 2px;
  }
  .admin-name-display.placeholder{
    font-family:'Work Sans',sans-serif;
    font-weight:500;
    font-size:14px;
    color:var(--ink-soft);
  }

  .receipt{
    background:var(--crust);
    color:#FFF7EA;
    border-radius:var(--radius);
    padding:18px 18px 16px;
    margin-bottom:16px;
    position:relative;
    clip-path:polygon(0% 0%,100% 0%,100% 92%,96% 100%,92% 92%,88% 100%,84% 92%,80% 100%,76% 92%,72% 100%,68% 92%,64% 100%,60% 92%,56% 100%,52% 92%,48% 100%,44% 92%,40% 100%,36% 92%,32% 100%,28% 92%,24% 100%,20% 92%,16% 100%,12% 92%,8% 100%,4% 92%,0% 100%);
    padding-bottom:26px;
  }
  .receipt .rlabel{
    font-family:'IBM Plex Mono',monospace;
    font-size:11px;
    letter-spacing:1.5px;
    text-transform:uppercase;
    opacity:0.85;
  }
  .receipt .ramount{
    font-family:'IBM Plex Mono',monospace;
    font-weight:600;
    font-size:30px;
    margin-top:4px;
  }

  .breakdown-line{
    display:flex;
    justify-content:space-between;
    align-items:center;
    padding:8px 0;
    border-bottom:1px dashed var(--line);
    font-size:13.5px;
  }
  .breakdown-line:last-child{border-bottom:none;}
  .breakdown-line .bl-label{color:var(--ink-soft);}
  .breakdown-line .bl-label small{display:block;font-size:10.5px;margin-top:1px;}
  .breakdown-line .bl-val{
    font-family:'IBM Plex Mono',monospace;
    font-weight:600;
  }
  .bl-minus .bl-val{color:var(--danger);}
  .bl-minus .bl-val::before{content:"- ";}

  .laba-box{
    background:var(--sage-soft);
    border:1.5px solid var(--sage);
    border-radius:var(--radius);
    padding:18px;
    text-align:center;
    margin-top:14px;
  }
  .laba-box .ll{
    font-family:'Fraunces',serif;
    font-weight:900;
    font-size:15px;
    letter-spacing:2px;
    color:var(--sage);
  }
  .laba-box .lv{
    font-family:'IBM Plex Mono',monospace;
    font-weight:700;
    font-size:28px;
    color:var(--ink);
    margin-top:4px;
  }

  button{
    font-family:'Work Sans',sans-serif;
    font-weight:600;
    cursor:pointer;
    border:none;
  }
  .btn-primary{
    width:100%;
    background:var(--crust);
    color:#FFF7EA;
    padding:14px;
    border-radius:12px;
    font-size:15px;
    margin-top:6px;
  }
  .btn-primary:active{background:var(--crust-dark);}
  .btn-secondary{
    width:100%;
    background:var(--surface);
    color:var(--crust-dark);
    border:1.5px solid var(--crust);
    padding:12px;
    border-radius:12px;
    font-size:14px;
  }
  .btn-small{
    background:var(--wheat-soft);
    color:var(--crust-dark);
    padding:8px 12px;
    border-radius:8px;
    font-size:13px;
    font-weight:600;
  }
  .btn-del{
    background:none;
    color:var(--danger);
    font-size:18px;
    padding:2px 8px;
  }

  .list-item{
    display:flex;
    justify-content:space-between;
    align-items:center;
    padding:10px 0;
    border-bottom:1px solid var(--line);
    font-size:14px;
  }
  .list-item:last-child{border-bottom:none;}
  .tag{
    display:inline-block;
    font-size:10.5px;
    font-weight:700;
    padding:2px 8px;
    border-radius:20px;
    margin-left:6px;
    text-transform:uppercase;
    letter-spacing:0.5px;
  }
  .tag.inti{background:var(--wheat-soft);color:var(--crust-dark);}
  .tag.helper{background:var(--sage-soft);color:var(--sage);}

  .add-row{display:flex;gap:8px;}
  .add-row input{margin-bottom:0;flex:1;}
  .add-row select{margin-bottom:0;width:110px;flex:none;}

  .empty-state{
    text-align:center;
    padding:36px 10px;
    color:var(--ink-soft);
    font-size:13.5px;
  }
  .empty-state .em-icon{font-size:34px;display:block;margin-bottom:8px;}

  .riwayat-card{
    background:var(--surface);
    border:1px solid var(--line);
    border-left:4px solid var(--wheat);
    border-radius:12px;
    padding:12px 14px;
    margin-bottom:10px;
  }
  .riwayat-card .rw-top{display:flex;justify-content:space-between;align-items:center;}
  .riwayat-card .rw-name{font-weight:700;font-size:14.5px;}
  .riwayat-card .rw-date{font-family:'IBM Plex Mono',monospace;font-size:11.5px;color:var(--ink-soft);}
  .riwayat-card .rw-laba{
    font-family:'IBM Plex Mono',monospace;
    font-weight:700;
    color:var(--sage);
    font-size:15px;
    margin-top:4px;
  }
  .riwayat-card .rw-actions{display:flex;gap:8px;margin-top:8px;}
  .riwayat-card .rw-actions button{flex:1;padding:7px;font-size:12px;border-radius:8px;}

  .notice{
    background:var(--wheat-soft);
    border:1px solid var(--wheat);
    border-radius:10px;
    padding:10px 12px;
    font-size:11.5px;
    color:var(--crust-dark);
    margin-bottom:14px;
    line-height:1.5;
  }

  .bottom-nav{
    position:fixed;
    bottom:0;left:50%;transform:translateX(-50%);
    width:100%;
    max-width:480px;
    background:#FFFDF8;
    border-top:1px solid var(--line);
    display:flex;
    z-index:30;
    box-shadow:0 -4px 14px rgba(43,27,18,0.06);
  }
  .bottom-nav button{
    flex:1;
    background:none;
    padding:11px 4px 9px;
    display:flex;
    flex-direction:column;
    align-items:center;
    gap:3px;
    color:var(--ink-soft);
    font-size:11px;
  }
  .bottom-nav button .nav-ic{font-size:19px;}
  .bottom-nav button.active{color:var(--crust);}

  h2.section-title{
    font-family:'Fraunces',serif;
    font-size:19px;
    font-weight:700;
    margin:4px 0 14px;
    color:var(--ink);
  }
  .divider-label{
    font-family:'IBM Plex Mono',monospace;
    font-size:10.5px;
    letter-spacing:1.5px;
    text-transform:uppercase;
    color:var(--ink-soft);
    margin:16px 0 8px;
  }
  .toast{
    position:fixed;
    bottom:90px;left:50%;
    transform:translateX(-50%);
    background:var(--ink);
    color:#FFF7EA;
    padding:10px 18px;
    border-radius:30px;
    font-size:13px;
    z-index:50;
    opacity:0;
    transition:opacity .25s;
    pointer-events:none;
    max-width:85%;
    text-align:center;
  }
  .toast.show{opacity:1;}
</style>
</head>
<body>
<div id="app">

  <div class="topbar">
    <div class="brand">
      <span class="brand-mark">🥖 Skanepa Bakery</span>
    </div>
    <div class="brand-sub">catatan penjualan roti sekolah</div>
  </div>

  <main>

    <!-- ================= INPUT PENJUALAN ================= -->
    <section id="view-input" class="view active">
      <h2 class="section-title">Input Penjualan</h2>

      <div class="card">
        <label for="selAdmin">Nama Admin</label>
        <select id="selAdmin"></select>
        <label for="inpTanggal">Tanggal</label>
        <input type="date" id="inpTanggal">
        <div id="adminNameDisplay" class="admin-name-display placeholder">Pilih nama admin di atas</div>
      </div>

      <div class="receipt">
        <div class="rlabel">Uang Masuk · Hasil Penjualan Hari Ini</div>
        <div class="ramount" id="uangMasukDisplay">Rp 0</div>
      </div>

      <div class="card">
        <h3>Produksi Roti</h3>
        <div class="row2">
          <div>
            <label id="lblBahan">Jumlah Bahan (kg)</label>
            <input type="number" min="0" step="0.01" id="jumlahBahan" placeholder="0">
          </div>
          <div>
            <label>Jumlah Roti Dihasilkan</label>
            <input type="number" min="0" step="1" id="jumlahDihasilkan" placeholder="0">
          </div>
        </div>
      </div>

      <div class="card">
        <h3>Harga &amp; Penjualan</h3>
        <div class="row2">
          <div>
            <label>Harga Satuan Roti (Rp)</label>
            <input type="number" min="0" step="50" id="hargaSatuan" placeholder="0">
          </div>
          <div>
            <label>Harga Total Roti (Rp)</label>
            <input type="text" id="hargaTotalDisplay" readonly style="background:#F3EEE3;font-weight:600;">
          </div>
        </div>
        <label>Jumlah Roti Terjual</label>
        <input type="number" min="0" step="1" id="jumlahTerjual" placeholder="0">
        <label>Uang Keluar · Belanja Bahan Hari Ini (Rp)</label>
        <input type="number" min="0" step="500" id="uangKeluarBahan" placeholder="0">
      </div>

      <div class="card">
        <h3>Rincian Potongan</h3>
        <div class="breakdown-line bl-minus">
          <div class="bl-label">Belanja Bahan<small>uang keluar hari ini</small></div>
          <div class="bl-val" id="calcBelanja">Rp 0</div>
        </div>
        <div class="breakdown-line bl-minus">
          <div class="bl-label" id="lblPeralatan">Biaya Peralatan<small>persen dari uang masuk</small></div>
          <div class="bl-val" id="calcPeralatan">Rp 0</div>
        </div>
        <div class="breakdown-line bl-minus">
          <div class="bl-label">Gaji Pegawai<small id="lblGajiDetail">-</small></div>
          <div class="bl-val" id="calcGaji">Rp 0</div>
        </div>
        <div class="breakdown-line bl-minus">
          <div class="bl-label" id="lblBonus">Bonus Pegawai<small>per roti terjual</small></div>
          <div class="bl-val" id="calcBonus">Rp 0</div>
        </div>
        <div class="breakdown-line bl-minus">
          <div class="bl-label" id="lblSkabic">Biaya Skabic<small>persen dari uang masuk</small></div>
          <div class="bl-val" id="calcSkabic">Rp 0</div>
        </div>
      </div>

      <div class="laba-box">
        <div class="ll">LABA</div>
        <div class="lv" id="calcLaba">Rp 0</div>
      </div>

      <button class="btn-primary" id="btnSimpan" style="margin-top:14px;">Simpan Transaksi Hari Ini</button>
    </section>

    <!-- ================= RIWAYAT ================= -->
    <section id="view-riwayat" class="view">
      <h2 class="section-title">Riwayat</h2>

      <div class="card">
        <h3>Unduh Laporan PDF</h3>
        <label>Nama Admin</label>
        <select id="pdfAdmin"></select>
        <label>Tanggal</label>
        <input type="date" id="pdfTanggal">
        <button class="btn-primary" id="btnDownloadPdf">⬇ Unduh PDF Laporan</button>
      </div>

      <div class="divider-label">Daftar Riwayat (terbaru dahulu)</div>
      <div id="riwayatList"></div>
    </section>

    <!-- ================= PENGATURAN ================= -->
    <section id="view-pengaturan" class="view">
      <h2 class="section-title">Pengaturan</h2>

      <div class="notice">ℹ️ Data pengaturan &amp; riwayat pada aplikasi ini tersimpan bersama (shared) — semua orang yang membuka aplikasi ini melihat &amp; memakai data yang sama.</div>

      <div class="card">
        <h3>Daftar Nama Admin</h3>
        <div id="adminSettingsList"></div>
        <div class="add-row" style="margin-top:10px;">
          <input type="text" id="newAdminName" placeholder="Nama admin baru">
          <button class="btn-small" id="btnAddAdmin">Tambah</button>
        </div>
      </div>

      <div class="card">
        <h3>Label Bahan Baku</h3>
        <label>Nama bahan yang ditampilkan di form input (satuan tetap kg)</label>
        <input type="text" id="bahanLabelInput" placeholder="cth: Tepung Terigu">
      </div>

      <div class="card">
        <h3>Daftar Pegawai</h3>
        <div id="pegawaiSettingsList"></div>
        <div class="add-row" style="margin-top:10px;">
          <input type="text" id="newPegawaiName" placeholder="Nama pegawai baru">
          <select id="newPegawaiKategori">
            <option value="inti">Inti</option>
            <option value="helper">Helper</option>
          </select>
          <button class="btn-small" id="btnAddPegawai">+</button>
        </div>
        <div class="row2" style="margin-top:12px;">
          <div>
            <label>Gaji Pegawai Inti (Rp/hari)</label>
            <input type="number" min="0" id="gajiIntiInput">
          </div>
          <div>
            <label>Gaji Pegawai Helper (Rp/hari)</label>
            <input type="number" min="0" id="gajiHelperInput">
          </div>
        </div>
      </div>

      <div class="card">
        <h3>Bonus &amp; Persentase Biaya</h3>
        <label>Nilai Bonus per Roti Terjual (Rp)</label>
        <input type="number" min="0" id="bonusPerRotiInput">
        <div class="row2">
          <div>
            <label>Biaya Peralatan (%)</label>
            <input type="number" min="0" max="100" step="0.1" id="pctPeralatanInput">
          </div>
          <div>
            <label>Biaya Skabic (%)</label>
            <input type="number" min="0" max="100" step="0.1" id="pctSkabicInput">
          </div>
        </div>
      </div>

      <div class="card">
        <h3>Guru Pembina (untuk tanda tangan laporan)</h3>
        <label>Nama Guru Pembina</label>
        <input type="text" id="guruNamaInput" placeholder="Nama lengkap">
        <label>NIP</label>
        <input type="text" id="guruNipInput" placeholder="NIP">
      </div>

      <button class="btn-primary" id="btnSaveSettings">Simpan Semua Pengaturan</button>
    </section>

  </main>

  <nav class="bottom-nav">
    <button data-view="input" class="active"><span class="nav-ic">🥐</span>Input</button>
    <button data-view="riwayat"><span class="nav-ic">📜</span>Riwayat</button>
    <button data-view="pengaturan"><span class="nav-ic">⚙️</span>Pengaturan</button>
  </nav>

  <div class="toast" id="toast"></div>
</div>

<script>
(function(){

const DEFAULT_SETTINGS = {
  admins: ["Admin 1"],
  bahanLabel: "Tepung Terigu",
  pegawai: [{name:"Pegawai Inti 1", kategori:"inti"}],
  gajiInti: 50000,
  gajiHelper: 30000,
  bonusPerRoti: 100,
  pctPeralatan: 30,
  pctSkabic: 2,
  guruNama: "",
  guruNip: ""
};

let settings = JSON.parse(JSON.stringify(DEFAULT_SETTINGS));
let riwayat = [];

function rp(n){
  n = Math.round(Number(n)||0);
  return "Rp " + n.toLocaleString('id-ID');
}
function todayStr(){
  const d = new Date();
  return d.getFullYear()+"-"+String(d.getMonth()+1).padStart(2,'0')+"-"+String(d.getDate()).padStart(2,'0');
}
function fmtDateID(iso){
  if(!iso) return "-";
  const [y,m,d] = iso.split("-");
  const bulan = ["Januari","Februari","Maret","April","Mei","Juni","Juli","Agustus","September","Oktober","November","Desember"];
  return `${parseInt(d)} ${bulan[parseInt(m)-1]} ${y}`;
}
function showToast(msg){
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'), 2200);
}

// ---------- STORAGE ----------
async function loadAll(){
  try{
    const s = await window.storage.get('settings', true);
    if(s && s.value){
      settings = Object.assign({}, DEFAULT_SETTINGS, JSON.parse(s.value));
    }else{
      await window.storage.set('settings', JSON.stringify(settings), true);
    }
  }catch(e){ console.error('load settings error', e); }

  try{
    const r = await window.storage.get('riwayat-penjualan', true);
    riwayat = (r && r.value) ? JSON.parse(r.value) : [];
  }catch(e){ riwayat = []; }
}
async function persistSettings(){
  try{ await window.storage.set('settings', JSON.stringify(settings), true); }
  catch(e){ console.error(e); showToast('Gagal menyimpan pengaturan'); }
}
async function persistRiwayat(){
  try{ await window.storage.set('riwayat-penjualan', JSON.stringify(riwayat), true); }
  catch(e){ console.error(e); showToast('Gagal menyimpan riwayat'); }
}

// ---------- NAV ----------
document.querySelectorAll('.bottom-nav button').forEach(btn=>{
  btn.addEventListener('click', ()=>{
    document.querySelectorAll('.bottom-nav button').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    document.querySelectorAll('.view').forEach(v=>v.classList.remove('active'));
    document.getElementById('view-'+btn.dataset.view).classList.add('active');
    if(btn.dataset.view === 'riwayat') renderRiwayatList();
    if(btn.dataset.view === 'pengaturan') renderSettingsForm();
  });
});

// ---------- INPUT PENJUALAN ----------
const elAdmin = document.getElementById('selAdmin');
const elTanggal = document.getElementById('inpTanggal');
const elAdminDisplay = document.getElementById('adminNameDisplay');
const elJumlahBahan = document.getElementById('jumlahBahan');
const elJumlahDihasilkan = document.getElementById('jumlahDihasilkan');
const elHargaSatuan = document.getElementById('hargaSatuan');
const elHargaTotalDisplay = document.getElementById('hargaTotalDisplay');
const elJumlahTerjual = document.getElementById('jumlahTerjual');
const elUangKeluarBahan = document.getElementById('uangKeluarBahan');

function populateAdminSelects(){
  [elAdmin, document.getElementById('pdfAdmin')].forEach(sel=>{
    const cur = sel.value;
    sel.innerHTML = '<option value="">-- pilih --</option>' +
      settings.admins.map(a=>`<option value="${a}">${a}</option>`).join('');
    if(settings.admins.includes(cur)) sel.value = cur;
  });
}

function findExistingRecord(admin, tanggal){
  return riwayat.find(r=>r.admin===admin && r.tanggal===tanggal);
}

function loadFormFromRecord(rec){
  elJumlahBahan.value = rec ? rec.jumlahBahan : '';
  elJumlahDihasilkan.value = rec ? rec.jumlahDihasilkan : '';
  elHargaSatuan.value = rec ? rec.hargaSatuan : '';
  elJumlahTerjual.value = rec ? rec.jumlahTerjual : '';
  elUangKeluarBahan.value = rec ? rec.uangKeluarBahan : '';
}

function onAdminOrDateChange(){
  const admin = elAdmin.value;
  const tanggal = elTanggal.value;
  if(admin){
    elAdminDisplay.textContent = admin;
    elAdminDisplay.classList.remove('placeholder');
  }else{
    elAdminDisplay.textContent = 'Pilih nama admin di atas';
    elAdminDisplay.classList.add('placeholder');
  }
  const rec = (admin && tanggal) ? findExistingRecord(admin, tanggal) : null;
  loadFormFromRecord(rec);
  recalc();
}

function gajiPegawaiTotal(){
  let total = 0;
  settings.pegawai.forEach(p=>{
    total += (p.kategori === 'inti') ? Number(settings.gajiInti||0) : Number(settings.gajiHelper||0);
  });
  return total;
}

function recalc(){
  const hargaSatuan = Number(elHargaSatuan.value)||0;
  const jumlahDihasilkan = Number(elJumlahDihasilkan.value)||0;
  const jumlahTerjual = Number(elJumlahTerjual.value)||0;
  const uangKeluarBahan = Number(elUangKeluarBahan.value)||0;

  const hargaTotal = hargaSatuan * jumlahDihasilkan;
  elHargaTotalDisplay.value = rp(hargaTotal);

  const uangMasuk = hargaSatuan * jumlahTerjual;
  document.getElementById('uangMasukDisplay').textContent = rp(uangMasuk);

  const biayaPeralatan = uangMasuk * (Number(settings.pctPeralatan||0)/100);
  const gajiPegawai = gajiPegawaiTotal();
  const bonus = jumlahTerjual * Number(settings.bonusPerRoti||0);
  const biayaSkabic = uangMasuk * (Number(settings.pctSkabic||0)/100);

  const laba = uangMasuk - uangKeluarBahan - biayaPeralatan - gajiPegawai - bonus - biayaSkabic;

  document.getElementById('calcBelanja').textContent = rp(uangKeluarBahan);
  document.getElementById('lblPeralatan').innerHTML = `Biaya Peralatan<small>${settings.pctPeralatan}% dari uang masuk</small>`;
  document.getElementById('calcPeralatan').textContent = rp(biayaPeralatan);
  document.getElementById('lblGajiDetail').textContent = settings.pegawai.length
    ? settings.pegawai.map(p=>`${p.name} (${p.kategori})`).join(', ')
    : 'belum ada pegawai';
  document.getElementById('calcGaji').textContent = rp(gajiPegawai);
  document.getElementById('lblBonus').innerHTML = `Bonus Pegawai<small>${jumlahTerjual} roti × ${rp(settings.bonusPerRoti)}</small>`;
  document.getElementById('calcBonus').textContent = rp(bonus);
  document.getElementById('lblSkabic').innerHTML = `Biaya Skabic<small>${settings.pctSkabic}% dari uang masuk</small>`;
  document.getElementById('calcSkabic').textContent = rp(biayaSkabic);
  document.getElementById('calcLaba').textContent = rp(laba);

  return {hargaSatuan, jumlahDihasilkan, hargaTotal, jumlahTerjual, uangMasuk, uangKeluarBahan,
    biayaPeralatan, gajiPegawai, bonus, biayaSkabic, laba};
}

[elJumlahBahan, elJumlahDihasilkan, elHargaSatuan, elJumlahTerjual, elUangKeluarBahan].forEach(el=>{
  el.addEventListener('input', recalc);
});
elAdmin.addEventListener('change', onAdminOrDateChange);
elTanggal.addEventListener('change', onAdminOrDateChange);

document.getElementById('btnSimpan').addEventListener('click', async ()=>{
  const admin = elAdmin.value;
  const tanggal = elTanggal.value;
  if(!admin){ showToast('Pilih nama admin terlebih dahulu'); return; }
  if(!tanggal){ showToast('Pilih tanggal terlebih dahulu'); return; }

  const c = recalc();
  const record = {
    id: admin+'|'+tanggal,
    admin, tanggal,
    jumlahBahan: Number(elJumlahBahan.value)||0,
    bahanLabel: settings.bahanLabel,
    jumlahDihasilkan: c.jumlahDihasilkan,
    hargaSatuan: c.hargaSatuan,
    hargaTotal: c.hargaTotal,
    jumlahTerjual: c.jumlahTerjual,
    uangMasuk: c.uangMasuk,
    uangKeluarBahan: c.uangKeluarBahan,
    biayaPeralatan: c.biayaPeralatan,
    pctPeralatan: settings.pctPeralatan,
    gajiPegawai: c.gajiPegawai,
    pegawaiSnapshot: JSON.parse(JSON.stringify(settings.pegawai)),
    bonus: c.bonus,
    bonusPerRoti: settings.bonusPerRoti,
    biayaSkabic: c.biayaSkabic,
    pctSkabic: settings.pctSkabic,
    laba: c.laba,
    savedAt: new Date().toISOString()
  };

  const idx = riwayat.findIndex(r=>r.id===record.id);
  if(idx>=0) riwayat[idx] = record; else riwayat.push(record);
  await persistRiwayat();
  showToast('Transaksi tersimpan ✓');
});

// ---------- RIWAYAT ----------
function renderRiwayatList(){
  const wrap = document.getElementById('riwayatList');
  const sorted = [...riwayat].sort((a,b)=> b.tanggal.localeCompare(a.tanggal) || a.admin.localeCompare(b.admin));
  if(sorted.length===0){
    wrap.innerHTML = `<div class="empty-state"><span class="em-icon">🧺</span>Belum ada riwayat penjualan</div>`;
    return;
  }
  wrap.innerHTML = sorted.map(r=>`
    <div class="riwayat-card">
      <div class="rw-top">
        <div class="rw-name">${r.admin}</div>
        <div class="rw-date">${fmtDateID(r.tanggal)}</div>
      </div>
      <div class="rw-laba">LABA ${rp(r.laba)}</div>
      <div class="rw-actions">
        <button class="btn-small" data-act="edit" data-id="${r.id}">Buka di Input</button>
        <button class="btn-small" style="background:#F3D9D9;color:#8A2E2E;" data-act="del" data-id="${r.id}">Hapus</button>
      </div>
    </div>
  `).join('');

  wrap.querySelectorAll('[data-act="edit"]').forEach(b=>{
    b.addEventListener('click', ()=>{
      const rec = riwayat.find(x=>x.id===b.dataset.id);
      if(!rec) return;
      elAdmin.value = rec.admin;
      elTanggal.value = rec.tanggal;
      onAdminOrDateChange();
      document.querySelector('.bottom-nav button[data-view="input"]').click();
    });
  });
  wrap.querySelectorAll('[data-act="del"]').forEach(b=>{
    b.addEventListener('click', async ()=>{
      if(!confirm('Hapus riwayat ini?')) return;
      riwayat = riwayat.filter(x=>x.id!==b.dataset.id);
      await persistRiwayat();
      renderRiwayatList();
      showToast('Riwayat dihapus');
    });
  });
}

document.getElementById('btnDownloadPdf').addEventListener('click', ()=>{
  const admin = document.getElementById('pdfAdmin').value;
  const tanggal = document.getElementById('pdfTanggal').value;
  if(!admin || !tanggal){ showToast('Pilih nama admin & tanggal dulu'); return; }
  const rec = findExistingRecord(admin, tanggal);
  if(!rec){ showToast('Data tidak ditemukan untuk admin & tanggal tsb'); return; }
  generatePDF(rec);
});

function generatePDF(rec){
  const { jsPDF } = window.jspdf;
  const doc = new jsPDF({orientation:'portrait', unit:'mm', format:'a4'});
  const pageW = doc.internal.pageSize.getWidth();
  const marginX = 18;

  doc.setFont('helvetica','bold');
  doc.setFontSize(18);
  doc.text('SKANEPA BAKERY', pageW/2, 20, {align:'center'});
  doc.setFont('helvetica','normal');
  doc.setFontSize(10.5);
  doc.text('Laporan Penjualan Roti Harian', pageW/2, 26, {align:'center'});

  doc.setDrawColor(168,72,29);
  doc.setLineWidth(0.6);
  doc.line(marginX, 30, pageW-marginX, 30);

  doc.setFontSize(10);
  doc.setFont('helvetica','bold');
  doc.text('Diinput oleh', marginX, 37);
  doc.setFont('helvetica','normal');
  doc.text(rec.admin, marginX, 42.5);
  doc.setLineWidth(0.2);
  doc.line(marginX, 44, marginX+55, 44);

  doc.setFont('helvetica','bold');
  doc.text('Tanggal', pageW-marginX-45, 37);
  doc.setFont('helvetica','normal');
  doc.text(fmtDateID(rec.tanggal), pageW-marginX-45, 42.5);
  doc.line(pageW-marginX-45, 44, pageW-marginX, 44);

  const rows = [
    ['Jumlah Bahan (' + (rec.bahanLabel||'Bahan') + ')', rec.jumlahBahan + ' kg'],
    ['Jumlah Roti Dihasilkan', rec.jumlahDihasilkan + ' pcs'],
    ['Harga Satuan Roti', rp(rec.hargaSatuan)],
    ['Harga Total Roti (produksi)', rp(rec.hargaTotal)],
    ['Jumlah Roti Terjual', rec.jumlahTerjual + ' pcs'],
    ['Uang Masuk (Hasil Penjualan)', rp(rec.uangMasuk)],
    ['Uang Keluar (Belanja Bahan)', '- ' + rp(rec.uangKeluarBahan)],
    ['Biaya Peralatan (' + rec.pctPeralatan + '%)', '- ' + rp(rec.biayaPeralatan)],
    ['Gaji Pegawai (' + (rec.pegawaiSnapshot||[]).map(p=>p.name+' - '+p.kategori).join(', ') + ')', '- ' + rp(rec.gajiPegawai)],
    ['Bonus Pegawai (' + rec.jumlahTerjual + ' x ' + rp(rec.bonusPerRoti) + ')', '- ' + rp(rec.bonus)],
    ['Biaya Skabic (' + rec.pctSkabic + '%)', '- ' + rp(rec.biayaSkabic)],
  ];

  doc.autoTable({
    startY: 50,
    head: [['Rincian', 'Nilai']],
    body: rows,
    theme: 'grid',
    styles: { font:'helvetica', fontSize:9.5, cellPadding:2.6 },
    headStyles: { fillColor:[168,72,29], textColor:255, fontStyle:'bold' },
    columnStyles: { 1: {halign:'right', cellWidth:55} },
    margin: { left: marginX, right: marginX }
  });

  let y = doc.lastAutoTable.finalY + 4;
  doc.setFillColor(225,233,214);
  doc.rect(marginX, y, pageW-marginX*2, 12, 'F');
  doc.setFont('helvetica','bold');
  doc.setFontSize(12);
  doc.setTextColor(43,27,18);
  doc.text('LABA', marginX+4, y+8);
  doc.text(rp(rec.laba), pageW-marginX-4, y+8, {align:'right'});
  doc.setTextColor(0,0,0);

  y += 30;
  const sigX = pageW - marginX - 55;
  doc.setFont('helvetica','normal');
  doc.setFontSize(10);
  doc.text('Mengetahui,', sigX, y);
  doc.text('Guru Pembina', sigX, y+5);
  y += 28;
  doc.line(sigX, y, sigX+55, y);
  doc.setFont('helvetica','bold');
  doc.text(rec.__guruNama || '', sigX, y+5);
  doc.setFont('helvetica','normal');
  doc.text('NIP. ' + (rec.__guruNip || '-'), sigX, y+10);

  doc.save(`Laporan-Skanepa-${rec.admin}-${rec.tanggal}.pdf`);
}

// ---------- PENGATURAN ----------
function renderAdminSettingsList(){
  const wrap = document.getElementById('adminSettingsList');
  if(settings.admins.length===0){
    wrap.innerHTML = `<div class="empty-state" style="padding:14px 0;">Belum ada nama admin</div>`;
    return;
  }
  wrap.innerHTML = settings.admins.map((a,i)=>`
    <div class="list-item">
      <span>${a}</span>
      <button class="btn-del" data-i="${i}">✕</button>
    </div>`).join('');
  wrap.querySelectorAll('.btn-del').forEach(b=>{
    b.addEventListener('click', ()=>{
      settings.admins.splice(Number(b.dataset.i),1);
      renderAdminSettingsList();
    });
  });
}
function renderPegawaiSettingsList(){
  const wrap = document.getElementById('pegawaiSettingsList');
  if(settings.pegawai.length===0){
    wrap.innerHTML = `<div class="empty-state" style="padding:14px 0;">Belum ada pegawai</div>`;
    return;
  }
  wrap.innerHTML = settings.pegawai.map((p,i)=>`
    <div class="list-item">
      <span>${p.name} <span class="tag ${p.kategori}">${p.kategori}</span></span>
      <button class="btn-del" data-i="${i}">✕</button>
    </div>`).join('');
  wrap.querySelectorAll('.btn-del').forEach(b=>{
    b.addEventListener('click', ()=>{
      settings.pegawai.splice(Number(b.dataset.i),1);
      renderPegawaiSettingsList();
    });
  });
}

function renderSettingsForm(){
  renderAdminSettingsList();
  renderPegawaiSettingsList();
  document.getElementById('bahanLabelInput').value = settings.bahanLabel;
  document.getElementById('gajiIntiInput').value = settings.gajiInti;
  document.getElementById('gajiHelperInput').value = settings.gajiHelper;
  document.getElementById('bonusPerRotiInput').value = settings.bonusPerRoti;
  document.getElementById('pctPeralatanInput').value = settings.pctPeralatan;
  document.getElementById('pctSkabicInput').value = settings.pctSkabic;
  document.getElementById('guruNamaInput').value = settings.guruNama;
  document.getElementById('guruNipInput').value = settings.guruNip;
}

document.getElementById('btnAddAdmin').addEventListener('click', ()=>{
  const inp = document.getElementById('newAdminName');
  const v = inp.value.trim();
  if(!v){ showToast('Isi nama admin dulu'); return; }
  if(settings.admins.includes(v)){ showToast('Nama sudah ada'); return; }
  settings.admins.push(v);
  inp.value='';
  renderAdminSettingsList();
});
document.getElementById('btnAddPegawai').addEventListener('click', ()=>{
  const inp = document.getElementById('newPegawaiName');
  const kat = document.getElementById('newPegawaiKategori').value;
  const v = inp.value.trim();
  if(!v){ showToast('Isi nama pegawai dulu'); return; }
  settings.pegawai.push({name:v, kategori:kat});
  inp.value='';
  renderPegawaiSettingsList();
});

document.getElementById('btnSaveSettings').addEventListener('click', async ()=>{
  settings.bahanLabel = document.getElementById('bahanLabelInput').value.trim() || 'Bahan';
  settings.gajiInti = Number(document.getElementById('gajiIntiInput').value)||0;
  settings.gajiHelper = Number(document.getElementById('gajiHelperInput').value)||0;
  settings.bonusPerRoti = Number(document.getElementById('bonusPerRotiInput').value)||0;
  settings.pctPeralatan = Number(document.getElementById('pctPeralatanInput').value)||0;
  settings.pctSkabic = Number(document.getElementById('pctSkabicInput').value)||0;
  settings.guruNama = document.getElementById('guruNamaInput').value.trim();
  settings.guruNip = document.getElementById('guruNipInput').value.trim();

  await persistSettings();
  populateAdminSelects();
  document.getElementById('lblBahan').textContent = settings.bahanLabel + ' (kg)';
  recalc();
  showToast('Pengaturan tersimpan ✓');
});

// ---------- INIT ----------
async function init(){
  await loadAll();
  populateAdminSelects();
  document.getElementById('lblBahan').textContent = settings.bahanLabel + ' (kg)';
  elTanggal.value = todayStr();
  document.getElementById('pdfTanggal').value = todayStr();
  renderSettingsForm();
  renderRiwayatList();
  recalc();

  // attach guru info onto record right before pdf generation (in case updated after saving)
  const origGen = generatePDF;
  window.__origGenPDF = origGen;
}
init();

// patch generatePDF to inject latest guru info at call-time
const _generatePDF = generatePDF;
generatePDF = function(rec){
  rec = Object.assign({}, rec, { __guruNama: settings.guruNama, __guruNip: settings.guruNip });
  _generatePDF(rec);
};

})();
</script>
</body>
</html>
