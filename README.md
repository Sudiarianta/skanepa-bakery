<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>Skanepa Bakery</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=IBM+Plex+Mono:wght@500;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.8.2/jspdf.plugin.autotable.min.js"></script>
<style>
  :root{
    --bg:#F3F5F8;
    --surface:#FFFFFF;
    --ink:#1E2A38;
    --ink-soft:#66748A;
    --primary:#2F5D8A;
    --primary-dark:#1F3F5E;
    --primary-tint:#EAF1F8;
    --positive:#1F7A54;
    --positive-bg:#E6F4EC;
    --danger:#B3261E;
    --danger-bg:#FBEAE9;
    --line:#E1E6EC;
    --radius:14px;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:var(--bg);
    color:var(--ink);
    font-family:'Inter',sans-serif;
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
    padding:20px 20px 16px;
    background:var(--primary-dark);
    color:#F4F7FA;
    position:sticky;
    top:0;
    z-index:20;
  }
  .topbar .brand-mark{
    font-weight:800;
    font-size:20px;
    letter-spacing:0.2px;
  }
  .topbar .brand-sub{
    font-family:'IBM Plex Mono',monospace;
    font-size:10px;
    letter-spacing:1.6px;
    text-transform:uppercase;
    color:#9FB6CC;
    margin-top:3px;
  }
  main{flex:1;padding:18px 16px 12px;}
  .view{display:none;}
  .view.active{display:block;animation:fadeIn .2s ease;}
  @keyframes fadeIn{from{opacity:0;transform:translateY(6px);}to{opacity:1;transform:translateY(0);}}

  .card{
    background:var(--surface);
    border:1px solid var(--line);
    border-radius:var(--radius);
    padding:16px;
    margin-bottom:14px;
  }
  .card h3{
    font-size:12.5px;
    font-weight:700;
    margin:0 0 12px;
    color:var(--primary-dark);
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
    border-radius:9px;
    border:1.5px solid var(--line);
    background:#FCFDFE;
    font-family:'Inter',sans-serif;
    font-size:15px;
    color:var(--ink);
    margin-bottom:12px;
  }
  input:focus,select:focus{outline:2px solid var(--primary);border-color:var(--primary);}
  .row2{display:grid;grid-template-columns:1fr 1fr;gap:10px;}

  .admin-name-display{
    font-weight:800;
    font-size:24px;
    color:var(--primary-dark);
    text-align:center;
    padding:8px 0 4px;
  }
  .admin-name-display.placeholder{
    font-weight:500;
    font-size:14px;
    color:var(--ink-soft);
  }

  .receipt{
    background:var(--primary);
    color:#FFFFFF;
    border-radius:var(--radius);
    padding:18px;
    margin-bottom:16px;
  }
  .receipt .rlabel{
    font-family:'IBM Plex Mono',monospace;
    font-size:10.5px;
    letter-spacing:1.4px;
    text-transform:uppercase;
    opacity:0.85;
  }
  .receipt .ramount{
    font-family:'IBM Plex Mono',monospace;
    font-weight:600;
    font-size:29px;
    margin-top:4px;
  }

  .breakdown-line{
    display:flex;
    justify-content:space-between;
    align-items:center;
    padding:8px 0;
    border-bottom:1px solid var(--line);
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

  .staff-pay-row{
    border:1px solid var(--line);
    border-radius:10px;
    padding:10px 12px;
    margin-bottom:8px;
  }
  .staff-pay-row:last-child{margin-bottom:0;}
  .staff-pay-row .sp-name{font-weight:700;font-size:14px;}
  .staff-pay-row .sp-role{
    font-size:10.5px;
    color:var(--primary);
    font-weight:700;
    text-transform:uppercase;
    letter-spacing:0.5px;
    margin-left:6px;
  }
  .staff-pay-row .sp-nums{
    display:flex;
    justify-content:space-between;
    margin-top:6px;
    font-family:'IBM Plex Mono',monospace;
    font-size:12.5px;
    color:var(--ink-soft);
  }

  .laba-box{
    background:var(--positive-bg);
    border:1.5px solid var(--positive);
    border-radius:var(--radius);
    padding:18px;
    text-align:center;
    margin-top:14px;
  }
  .laba-box .ll{
    font-weight:800;
    font-size:13px;
    letter-spacing:2px;
    color:var(--positive);
  }
  .laba-box .lv{
    font-family:'IBM Plex Mono',monospace;
    font-weight:700;
    font-size:27px;
    color:var(--ink);
    margin-top:4px;
  }

  button{
    font-family:'Inter',sans-serif;
    font-weight:600;
    cursor:pointer;
    border:none;
  }
  .btn-primary{
    width:100%;
    background:var(--primary);
    color:#FFFFFF;
    padding:14px;
    border-radius:11px;
    font-size:15px;
    margin-top:6px;
  }
  .btn-primary:active{background:var(--primary-dark);}
  .btn-small{
    background:var(--primary-tint);
    color:var(--primary-dark);
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

  .add-row{display:flex;gap:8px;}
  .add-row input{margin-bottom:0;flex:1;}

  .empty-state{
    text-align:center;
    padding:30px 10px;
    color:var(--ink-soft);
    font-size:13.5px;
  }

  .riwayat-card{
    background:var(--surface);
    border:1px solid var(--line);
    border-left:4px solid var(--primary);
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
    color:var(--positive);
    font-size:15px;
    margin-top:4px;
  }
  .riwayat-card .rw-actions{display:flex;gap:8px;margin-top:8px;}
  .riwayat-card .rw-actions button{flex:1;padding:7px;font-size:12px;border-radius:8px;}

  .notice{
    background:var(--primary-tint);
    border:1px solid #C7D9EA;
    border-radius:10px;
    padding:10px 12px;
    font-size:11.5px;
    color:var(--primary-dark);
    margin-bottom:14px;
    line-height:1.5;
  }

  .bottom-nav{
    position:fixed;
    bottom:0;left:50%;transform:translateX(-50%);
    width:100%;
    max-width:480px;
    background:#FFFFFF;
    border-top:1px solid var(--line);
    display:flex;
    z-index:30;
    box-shadow:0 -4px 14px rgba(30,42,56,0.06);
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
  .bottom-nav button .nav-ic{
    width:18px;height:18px;
    border-radius:5px;
    border:2px solid currentColor;
    display:inline-block;
  }
  .bottom-nav button.active{color:var(--primary);}

  h2.section-title{
    font-size:18px;
    font-weight:800;
    margin:4px 0 14px;
    color:var(--ink);
  }
  .divider-label{
    font-family:'IBM Plex Mono',monospace;
    font-size:10.5px;
    letter-spacing:1.4px;
    text-transform:uppercase;
    color:var(--ink-soft);
    margin:16px 0 8px;
  }
  .toast{
    position:fixed;
    bottom:90px;left:50%;
    transform:translateX(-50%);
    background:var(--ink);
    color:#FFFFFF;
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
    <div class="brand-mark">Skanepa Bakery</div>
    <div class="brand-sub">catatan penjualan roti sekolah</div>
  </div>

  <main>

    <!-- ================= INPUT PENJUALAN ================= -->
    <section id="view-input" class="view active">
      <h2 class="section-title">Input Penjualan</h2>

      <div class="card">
        <div id="adminFieldWrap">
          <label for="selAdmin">Nama Admin</label>
          <select id="selAdmin"></select>
        </div>
        <div id="adminNameDisplay" class="admin-name-display placeholder">Pilih nama admin di atas</div>

        <label for="inpTanggal">Tanggal</label>
        <input type="date" id="inpTanggal">
      </div>

      <div class="card">
        <h3>Pegawai Bertugas Hari Ini</h3>
        <label>Pegawai Inti 1</label>
        <select id="staffInti1"></select>
        <label>Pegawai Inti 2</label>
        <select id="staffInti2"></select>
        <label>Pegawai Helper</label>
        <select id="staffHelper"></select>
      </div>

      <div class="receipt">
        <div class="rlabel">Uang Masuk · Hasil Penjualan Hari Ini</div>
        <div class="ramount" id="uangMasukDisplay">Rp 0</div>
      </div>

      <div class="card">
        <h3>Produksi Roti</h3>
        <div class="row2">
          <div>
            <label>Bahan (kg)</label>
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
            <input type="text" id="hargaTotalDisplay" readonly style="background:#F1F3F6;font-weight:600;">
          </div>
        </div>
        <label>Jumlah Roti Terjual</label>
        <input type="number" min="0" step="1" id="jumlahTerjual" placeholder="0">
        <label>Uang Keluar · Belanja Bahan Hari Ini (Rp)</label>
        <input type="number" min="0" step="500" id="uangKeluarBahan" placeholder="0">
      </div>

      <div class="card">
        <h3>Pendapatan Pegawai Hari Ini</h3>
        <div id="staffPayList"></div>
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
          <div class="bl-label" id="lblGajiDetail">Gaji Pegawai<small>-</small></div>
          <div class="bl-val" id="calcGaji">Rp 0</div>
        </div>
        <div class="breakdown-line bl-minus">
          <div class="bl-label" id="lblBonus">Bonus Pegawai<small>-</small></div>
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
        <button class="btn-primary" id="btnDownloadPdf">Unduh PDF Laporan</button>
      </div>

      <div class="divider-label">Daftar Riwayat (terbaru dahulu)</div>
      <div id="riwayatList"></div>
    </section>

    <!-- ================= PENGATURAN ================= -->
    <section id="view-pengaturan" class="view">
      <h2 class="section-title">Pengaturan</h2>

      <div class="notice">Data pengaturan &amp; riwayat pada aplikasi ini tersimpan bersama (shared) — semua orang yang membuka aplikasi ini melihat &amp; memakai data yang sama.</div>

      <div class="card">
        <h3>Daftar Nama Admin</h3>
        <div id="adminSettingsList"></div>
        <div class="add-row" style="margin-top:10px;">
          <input type="text" id="newAdminName" placeholder="Nama admin baru">
          <button class="btn-small" id="btnAddAdmin">Tambah</button>
        </div>
      </div>

      <div class="card">
        <h3>Daftar Pegawai</h3>
        <div id="pegawaiSettingsList"></div>
        <div class="add-row" style="margin-top:10px;">
          <input type="text" id="newPegawaiName" placeholder="Nama pegawai baru">
          <button class="btn-small" id="btnAddPegawai">Tambah</button>
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
        <h3>Persentase Biaya</h3>
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
    <button data-view="input" class="active"><span class="nav-ic"></span>Input</button>
    <button data-view="riwayat"><span class="nav-ic"></span>Riwayat</button>
    <button data-view="pengaturan"><span class="nav-ic"></span>Pengaturan</button>
  </nav>

  <div class="toast" id="toast"></div>
</div>

<script>
(function(){

const DEFAULT_SETTINGS = {
  admins: ["Admin 1"],
  pegawai: ["Pegawai 1", "Pegawai 2", "Pegawai 3"],
  gajiInti: 50000,
  gajiHelper: 30000,
  pctPeralatan: 30,
  pctSkabic: 2,
  guruNama: "",
  guruNip: ""
};
const BONUS_MULTIPLIER = 1000; // Rp per roti terjual, dibagi rata ke pegawai bertugas

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
    if(btn.dataset.view === 'input') resetInputForm();
  });
});

// ---------- INPUT PENJUALAN ----------
const elAdminFieldWrap = document.getElementById('adminFieldWrap');
const elAdmin = document.getElementById('selAdmin');
const elTanggal = document.getElementById('inpTanggal');
const elAdminDisplay = document.getElementById('adminNameDisplay');
const elStaffInti1 = document.getElementById('staffInti1');
const elStaffInti2 = document.getElementById('staffInti2');
const elStaffHelper = document.getElementById('staffHelper');
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
function populateStaffSelects(){
  [elStaffInti1, elStaffInti2, elStaffHelper].forEach(sel=>{
    const cur = sel.value;
    sel.innerHTML = '<option value="">-- pilih --</option>' +
      settings.pegawai.map(p=>`<option value="${p}">${p}</option>`).join('');
    if(settings.pegawai.includes(cur)) sel.value = cur;
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
  elStaffInti1.value = rec ? (rec.staffInti1 || '') : '';
  elStaffInti2.value = rec ? (rec.staffInti2 || '') : '';
  elStaffHelper.value = rec ? (rec.staffHelper || '') : '';
}

function resetInputForm(){
  elAdmin.value = '';
  elAdminFieldWrap.style.display = '';
  elAdminDisplay.textContent = 'Pilih nama admin di atas';
  elAdminDisplay.classList.add('placeholder');
  elTanggal.value = todayStr();
  loadFormFromRecord(null);
  recalc();
}

function onAdminChange(){
  const admin = elAdmin.value;
  if(admin){
    elAdminDisplay.textContent = admin;
    elAdminDisplay.classList.remove('placeholder');
    elAdminFieldWrap.style.display = 'none';
  }else{
    elAdminDisplay.textContent = 'Pilih nama admin di atas';
    elAdminDisplay.classList.add('placeholder');
    elAdminFieldWrap.style.display = '';
  }
  loadRecordForCurrentSelection();
}
function onTanggalChange(){
  loadRecordForCurrentSelection();
}
function loadRecordForCurrentSelection(){
  const admin = elAdmin.value;
  const tanggal = elTanggal.value;
  const rec = (admin && tanggal) ? findExistingRecord(admin, tanggal) : null;
  loadFormFromRecord(rec);
  recalc();
}

function staffList(){
  const list = [];
  if(elStaffInti1.value) list.push({slot:'Inti', name: elStaffInti1.value});
  if(elStaffInti2.value) list.push({slot:'Inti', name: elStaffInti2.value});
  if(elStaffHelper.value) list.push({slot:'Helper', name: elStaffHelper.value});
  return list;
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

  const staff = staffList();
  const gajiPegawai = staff.reduce((sum,s)=> sum + (s.slot==='Inti' ? Number(settings.gajiInti||0) : Number(settings.gajiHelper||0)), 0);
  const bonusTotal = jumlahTerjual * BONUS_MULTIPLIER;
  const bonusPerOrang = staff.length ? bonusTotal / staff.length : 0;

  const biayaPeralatan = uangMasuk * (Number(settings.pctPeralatan||0)/100);
  const biayaSkabic = uangMasuk * (Number(settings.pctSkabic||0)/100);

  const laba = uangMasuk - uangKeluarBahan - biayaPeralatan - gajiPegawai - bonusTotal - biayaSkabic;

  // staff pay list card
  const payWrap = document.getElementById('staffPayList');
  if(staff.length===0){
    payWrap.innerHTML = `<div class="empty-state" style="padding:14px 0;">Pilih pegawai bertugas terlebih dahulu</div>`;
  }else{
    payWrap.innerHTML = staff.map(s=>{
      const gaji = s.slot==='Inti' ? Number(settings.gajiInti||0) : Number(settings.gajiHelper||0);
      return `
      <div class="staff-pay-row">
        <span class="sp-name">${s.name}<span class="sp-role">${s.slot}</span></span>
        <div class="sp-nums"><span>Gaji: ${rp(gaji)}</span><span>Bonus: ${rp(bonusPerOrang)}</span></div>
      </div>`;
    }).join('');
  }

  document.getElementById('calcBelanja').textContent = rp(uangKeluarBahan);
  document.getElementById('lblPeralatan').innerHTML = `Biaya Peralatan<small>${settings.pctPeralatan}% dari uang masuk</small>`;
  document.getElementById('calcPeralatan').textContent = rp(biayaPeralatan);
  document.getElementById('lblGajiDetail').innerHTML = `Gaji Pegawai<small>${staff.length ? staff.map(s=>s.name+' ('+s.slot+')').join(', ') : 'belum ada pegawai bertugas'}</small>`;
  document.getElementById('calcGaji').textContent = rp(gajiPegawai);
  document.getElementById('lblBonus').innerHTML = `Bonus Pegawai<small>${jumlahTerjual} roti × ${rp(BONUS_MULTIPLIER)}, dibagi ${staff.length||0} pegawai</small>`;
  document.getElementById('calcBonus').textContent = rp(bonusTotal);
  document.getElementById('lblSkabic').innerHTML = `Biaya Skabic<small>${settings.pctSkabic}% dari uang masuk</small>`;
  document.getElementById('calcSkabic').textContent = rp(biayaSkabic);
  document.getElementById('calcLaba').textContent = rp(laba);

  return {hargaSatuan, jumlahDihasilkan, hargaTotal, jumlahTerjual, uangMasuk, uangKeluarBahan,
    biayaPeralatan, gajiPegawai, bonusTotal, bonusPerOrang, biayaSkabic, laba, staff};
}

[elJumlahBahan, elJumlahDihasilkan, elHargaSatuan, elJumlahTerjual, elUangKeluarBahan, elStaffInti1, elStaffInti2, elStaffHelper].forEach(el=>{
  el.addEventListener('input', recalc);
  el.addEventListener('change', recalc);
});
elAdmin.addEventListener('change', onAdminChange);
elTanggal.addEventListener('change', onTanggalChange);

document.getElementById('btnSimpan').addEventListener('click', async ()=>{
  const admin = elAdmin.value || elAdminDisplay.textContent;
  const tanggal = elTanggal.value;
  if(!admin || elAdminDisplay.classList.contains('placeholder')){ showToast('Pilih nama admin terlebih dahulu'); return; }
  if(!tanggal){ showToast('Pilih tanggal terlebih dahulu'); return; }

  const c = recalc();
  const record = {
    id: admin+'|'+tanggal,
    admin, tanggal,
    jumlahBahan: Number(elJumlahBahan.value)||0,
    jumlahDihasilkan: c.jumlahDihasilkan,
    hargaSatuan: c.hargaSatuan,
    hargaTotal: c.hargaTotal,
    jumlahTerjual: c.jumlahTerjual,
    uangMasuk: c.uangMasuk,
    uangKeluarBahan: c.uangKeluarBahan,
    biayaPeralatan: c.biayaPeralatan,
    pctPeralatan: settings.pctPeralatan,
    staffInti1: elStaffInti1.value,
    staffInti2: elStaffInti2.value,
    staffHelper: elStaffHelper.value,
    staffSnapshot: c.staff,
    gajiPegawai: c.gajiPegawai,
    gajiIntiRate: settings.gajiInti,
    gajiHelperRate: settings.gajiHelper,
    bonusTotal: c.bonusTotal,
    bonusPerOrang: c.bonusPerOrang,
    biayaSkabic: c.biayaSkabic,
    pctSkabic: settings.pctSkabic,
    laba: c.laba,
    savedAt: new Date().toISOString()
  };

  const idx = riwayat.findIndex(r=>r.id===record.id);
  if(idx>=0) riwayat[idx] = record; else riwayat.push(record);
  await persistRiwayat();
  showToast('Transaksi tersimpan');
});

// ---------- RIWAYAT ----------
function renderRiwayatList(){
  const wrap = document.getElementById('riwayatList');
  const sorted = [...riwayat].sort((a,b)=> b.tanggal.localeCompare(a.tanggal) || a.admin.localeCompare(b.admin));
  if(sorted.length===0){
    wrap.innerHTML = `<div class="empty-state">Belum ada riwayat penjualan</div>`;
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
        <button class="btn-small" style="background:var(--danger-bg);color:var(--danger);" data-act="del" data-id="${r.id}">Hapus</button>
      </div>
    </div>
  `).join('');

  wrap.querySelectorAll('[data-act="edit"]').forEach(b=>{
    b.addEventListener('click', ()=>{
      const rec = riwayat.find(x=>x.id===b.dataset.id);
      if(!rec) return;
      document.querySelector('.bottom-nav button[data-view="input"]').click();
      elAdmin.value = rec.admin;
      onAdminChange();
      elTanggal.value = rec.tanggal;
      loadRecordForCurrentSelection();
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

  doc.setDrawColor(47,93,138);
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

  const staffRows = (rec.staffSnapshot||[]).map(s=>{
    const gaji = s.slot==='Inti' ? rec.gajiIntiRate : rec.gajiHelperRate;
    return [`${s.name} (${s.slot})`, rp(gaji) + ' / ' + rp(rec.bonusPerOrang)];
  });

  const rows = [
    ['Jumlah Bahan (kg)', rec.jumlahBahan + ' kg'],
    ['Jumlah Roti Dihasilkan', rec.jumlahDihasilkan + ' pcs'],
    ['Harga Satuan Roti', rp(rec.hargaSatuan)],
    ['Harga Total Roti (produksi)', rp(rec.hargaTotal)],
    ['Jumlah Roti Terjual', rec.jumlahTerjual + ' pcs'],
    ['Uang Masuk (Hasil Penjualan)', rp(rec.uangMasuk)],
    ['Uang Keluar (Belanja Bahan)', '- ' + rp(rec.uangKeluarBahan)],
    ['Biaya Peralatan (' + rec.pctPeralatan + '%)', '- ' + rp(rec.biayaPeralatan)],
    ['Gaji Pegawai (total)', '- ' + rp(rec.gajiPegawai)],
    ['Bonus Pegawai (total)', '- ' + rp(rec.bonusTotal)],
    ['Biaya Skabic (' + rec.pctSkabic + '%)', '- ' + rp(rec.biayaSkabic)],
  ];

  doc.autoTable({
    startY: 50,
    head: [['Rincian', 'Nilai']],
    body: rows,
    theme: 'grid',
    styles: { font:'helvetica', fontSize:9.5, cellPadding:2.6 },
    headStyles: { fillColor:[47,93,138], textColor:255, fontStyle:'bold' },
    columnStyles: { 1: {halign:'right', cellWidth:55} },
    margin: { left: marginX, right: marginX }
  });

  let y = doc.lastAutoTable.finalY + 4;

  if(staffRows.length){
    doc.autoTable({
      startY: y,
      head: [['Pegawai Bertugas', 'Gaji / Bonus']],
      body: staffRows,
      theme: 'grid',
      styles: { font:'helvetica', fontSize:9, cellPadding:2.4 },
      headStyles: { fillColor:[102,116,138], textColor:255, fontStyle:'bold' },
      columnStyles: { 1: {halign:'right', cellWidth:55} },
      margin: { left: marginX, right: marginX }
    });
    y = doc.lastAutoTable.finalY + 4;
  }

  doc.setFillColor(230,244,236);
  doc.rect(marginX, y, pageW-marginX*2, 12, 'F');
  doc.setFont('helvetica','bold');
  doc.setFontSize(12);
  doc.setTextColor(31,42,56);
  doc.text('LABA', marginX+4, y+8);
  doc.text(rp(rec.laba), pageW-marginX-4, y+8, {align:'right'});
  doc.setTextColor(0,0,0);

  y += 26;
  const sigX = pageW - marginX - 55;
  doc.setFont('helvetica','normal');
  doc.setFontSize(10);
  doc.text('Mengetahui,', sigX, y);
  doc.text('Guru Pembina', sigX, y+5);
  y += 26;
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
      <span>${p}</span>
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
  document.getElementById('gajiIntiInput').value = settings.gajiInti;
  document.getElementById('gajiHelperInput').value = settings.gajiHelper;
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
  const v = inp.value.trim();
  if(!v){ showToast('Isi nama pegawai dulu'); return; }
  if(settings.pegawai.includes(v)){ showToast('Nama sudah ada'); return; }
  settings.pegawai.push(v);
  inp.value='';
  renderPegawaiSettingsList();
});

document.getElementById('btnSaveSettings').addEventListener('click', async ()=>{
  settings.gajiInti = Number(document.getElementById('gajiIntiInput').value)||0;
  settings.gajiHelper = Number(document.getElementById('gajiHelperInput').value)||0;
  settings.pctPeralatan = Number(document.getElementById('pctPeralatanInput').value)||0;
  settings.pctSkabic = Number(document.getElementById('pctSkabicInput').value)||0;
  settings.guruNama = document.getElementById('guruNamaInput').value.trim();
  settings.guruNip = document.getElementById('guruNipInput').value.trim();

  await persistSettings();
  populateAdminSelects();
  populateStaffSelects();
  recalc();
  showToast('Pengaturan tersimpan');
});

// ---------- INIT ----------
async function init(){
  await loadAll();
  populateAdminSelects();
  populateStaffSelects();
  document.getElementById('pdfTanggal').value = todayStr();
  renderSettingsForm();
  renderRiwayatList();
  resetInputForm();
}
init();

// inject latest guru info at pdf generation time
const _generatePDF = generatePDF;
generatePDF = function(rec){
  rec = Object.assign({}, rec, { __guruNama: settings.guruNama, __guruNip: settings.guruNip });
  _generatePDF(rec);
};

})();
</script>
</body>
</html>
