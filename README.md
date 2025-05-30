<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Kalkulator Gaji Kuli</title>
<style>
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    max-width: 600px;
    margin: 1rem auto;
    background: #f9fafb;
    color: #333;
    padding: 1rem;
  }
  h1 {
    text-align: center;
    color: #2c3e50;
  }
  table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 1rem;
    background: #fff;
    box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    border-radius: 8px;
    overflow: hidden;
  }
  thead th {
    background-color: #2980b9;
    color: white;
    padding: 0.75rem;
    text-align: left;
  }
  tbody tr:nth-child(even){
    background-color: #ecf0f1;
  }
  tbody tr:hover {
    background-color: #d0e7ff;
  }
  td {
    padding: 0.5rem 0.75rem;
    vertical-align: middle;
  }
  td.label-cell {
    font-weight: 600;
  }
  input[type="text"] {
    width: 100%;
    padding: 0.25rem 0.5rem;
    font-size: 1rem;
    border: 1px solid #bbb;
    border-radius: 4px;
    box-sizing: border-box;
    font-family: monospace;
    text-align: right;
  }
  tfoot td {
    font-weight: 700;
    font-size: 1.1rem;
    background-color: #2980b9;
    color: white;
    padding: 0.75rem;
  }
  #copyBtn {
    margin-top: 1rem;
    padding: 0.5rem 1rem;
    font-size: 1rem;
    background-color: #2980b9;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  #copyBtn:hover {
    background-color: #1f6391;
  }
</style>
</head>
<body>
<h1>Kalkulator Gaji Kuli</h1>
<form id="salaryForm" oninput="calculateTotal()">
  <table>
    <thead>
      <tr><th>Komponen</th><th>Nominal (Rp)</th></tr>
    </thead>
    <tbody>
      <tr><td class="label-cell">A. Gaji Pokok</td><td><input type="text" id="gajiPokok" value="3.390.000" /></td></tr>
      <tr><td class="label-cell">B. Tunjangan Tidak Tetap</td><td></td></tr>
      <tr><td style="padding-left: 1.5rem;">Kehadiran</td><td><input type="text" id="kehadiran" value="260.000" /></td></tr>
      <tr><td style="padding-left: 1.5rem;">Shift Malam</td><td><input type="text" id="shiftMalam" value="0" /></td></tr>
      <tr><td class="label-cell">C. Pendapatan Lain-lain</td><td></td></tr>
      <tr><td style="padding-left: 1.5rem;">Bonus Kinerja</td><td><input type="text" id="bonusKinerja" value="300.000" /></td></tr>
      <tr><td style="padding-left: 1.5rem;">Lembur</td><td><input type="text" id="lembur" value="0" /></td></tr>
      <tr><td style="padding-left: 1.5rem;">Tunjangan 3S3R</td><td><input type="text" id="tunjangan3s3r" value="33.000" /></td></tr>
      <tr><td style="padding-left: 1.5rem;">Bonus Masa Kerja</td><td><input type="text" id="bonusMasaKerja" value="120.000" /></td></tr>
      <tr><td style="padding-left: 1.5rem;">Bonus Produksi</td><td><input type="text" id="bonusProduksi" value="300.000" /></td></tr>
      <tr><td style="padding-left: 1.5rem;">Bonus Lain-lain</td><td><input type="text" id="bonusLainLain" value="1.200.000" /></td></tr>
      <tr><td class="label-cell">D. Tunjangan Tetap</td><td></td></tr>
      <tr><td style="padding-left: 1.5rem;">Lokasi</td><td><input type="text" id="tunjanganLokasi" value="100.000" /></td></tr>
      <tr><td style="padding-left: 1.5rem;">Perumahan</td><td><input type="text" id="tunjanganPerumahan" value="600.000" /></td></tr>
      <tr><td class="label-cell">E. Potongan</td><td></td></tr>
      <tr><td style="padding-left: 1.5rem;">Jaminan Pensiun</td><td><input type="text" id="jaminanPensiun" value="40.900" /></td></tr>
      <tr><td style="padding-left: 1.5rem;">Jaminan Hari Tua</td><td><input type="text" id="jaminanHariTua" value="81.800" /></td></tr>
      <tr><td style="padding-left: 1.5rem;">BPJS (potongan)</td><td><input type="text" id="bpjs" value="-122.700" /></td></tr>
    </tbody>
    <tfoot>
      <tr>
        <td>Total Gaji (A + B + C + D - E - BPJS)</td>
        <td><output id="totalGaji">0</output></td>
      </tr>
    </tfoot>
  </table>
</form>
<button id="copyBtn" onclick="copyTotalGaji()">Salin Total Gaji</button>
<script>
function parseRupiah(value) {
  if (!value) return 0;
  let num = value.replace(/[^0-9\-]/g, '');
  return Number(num) || 0;
}

function formatRupiah(num) {
  if (isNaN(num)) return '0';
  return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, '.');
}

function calculateTotal() {
  const getInputValue = id => parseRupiah(document.getElementById(id).value);
  let gajiPokok = getInputValue('gajiPokok');
  let kehadiran = getInputValue('kehadiran');
  let shiftMalam = getInputValue('shiftMalam');
  let bonusKinerja = getInputValue('bonusKinerja');
  let lembur = getInputValue('lembur');
  let tunjangan3s3r = getInputValue('tunjangan3s3r');
  let bonusMasaKerja = getInputValue('bonusMasaKerja');
  let bonusProduksi = getInputValue('bonusProduksi');
  let bonusLainLain = getInputValue('bonusLainLain');
  let tunjanganLokasi = getInputValue('tunjanganLokasi');
  let tunjanganPerumahan = getInputValue('tunjanganPerumahan');
  let jaminanPensiun = getInputValue('jaminanPensiun');
  let jaminanHariTua = getInputValue('jaminanHariTua');
  let bpjs = getInputValue('bpjs');

  let totalB = kehadiran + shiftMalam;
  let totalC = bonusKinerja + lembur + tunjangan3s3r + bonusMasaKerja + bonusProduksi + bonusLainLain;
  let totalD = tunjanganLokasi + tunjanganPerumahan;
  let totalE = jaminanPensiun + jaminanHariTua;

  let totalGaji = gajiPokok + totalB + totalC + totalD - totalE - bpjs;

  ['gajiPokok','kehadiran','shiftMalam','bonusKinerja','lembur','tunjangan3s3r','bonusMasaKerja','bonusProduksi','bonusLainLain','tunjanganLokasi','tunjanganPerumahan','jaminanPensiun','jaminanHariTua','bpjs'].forEach(id => {
    let el = document.getElementById(id);
    let val = parseRupiah(el.value);
    if(id === 'bpjs' && val > 0) val = -val;
    el.value = formatRupiah(val);
  });

  document.getElementById('totalGaji').value = formatRupiah(totalGaji);
}

function copyTotalGaji() {
  const total = document.getElementById('totalGaji').value;
  navigator.clipboard.writeText(total).then(() => {
    alert('Total Gaji berhasil disalin: ' + total);
  }, () => {
    alert('Gagal menyalin total gaji.');
  });
}

document.addEventListener('DOMContentLoaded', calculateTotal);
document.getElementById('salaryForm').addEventListener('input', calculateTotal);
</script>
</body>
</html>
