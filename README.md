<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>KSU BAHAGIA: Terus Berinovasi Untuk Bahagia Bersama</title>
  <style>
    body { font-family: Arial, sans-serif; background: #e6f0fa; padding: 20px; max-width: 800px; margin: auto; }
    h1, h2 { text-align: center; color: #2c3e50; }
    .branding { background-color: #3498db; color: white; padding: 10px; border-radius: 8px; margin-bottom: 20px; text-align: center; }
    input, button, select { padding: 8px; margin: 5px 0; width: 100%; border-radius: 5px; border: 1px solid #ccc; }
    button { background-color: #2980b9; color: white; cursor: pointer; }
    button:hover { background-color: #1f6391; }
    table { width: 100%; border-collapse: collapse; margin-top: 20px; }
    th, td { border: 1px solid #ccc; padding: 8px; text-align: center; }
    th { background-color: #2980b9; color: white; }
    .section { margin-bottom: 40px; }
  </style>
</head>
<body>

<div class="branding">
  <h1>KSU BAHAGIA: Terus Berinovasi Untuk Bahagia Bersama</h1>
  <p>Simulasi & Manajemen Pinjaman Anggota</p>
</div>

<div class="section">
  <h2>📋 Tambah Anggota</h2>
  <input type="text" id="nama" placeholder="Nama Anggota">
  <input type="text" id="nomang" placeholder="NOMANG">
  <button onclick="tambahAnggota()">Simpan Anggota</button>
</div>

<div class="section">
  <h2>💰 Tambah Pinjaman</h2>
  <select id="anggotaSelect"></select>
  <input type="number" id="pinjaman" placeholder="Jumlah Pinjaman (Rp)">
  <input type="number" id="bulan" placeholder="Lama Pinjaman (bulan)">
  <button onclick="hitungPinjaman()">Hitung & Simpan</button>
</div>

<div id="hasil"></div>

<div class="section">
  <h2>👥 Daftar Anggota</h2>
  <table id="tabelAnggota">
    <tr><th>Nama</th><th>NOMANG</th></tr>
  </table>
</div>

<script>
let anggota = JSON.parse(localStorage.getItem("anggota")) || [];

function simpanData() {
  localStorage.setItem("anggota", JSON.stringify(anggota));
  tampilkanAnggota();
  isiDropdown();
}

function tambahAnggota() {
  const nama = document.getElementById("nama").value.trim();
  const nomang = document.getElementById("nomang").value.trim();
  if (!nama || !nomang) return alert("Isi nama dan NOMANG.");
  anggota.push({ nama, nomang });
  simpanData();
  document.getElementById("nama").value = '';
  document.getElementById("nomang").value = '';
}

function tampilkanAnggota() {
  const tabel = document.getElementById("tabelAnggota");
  tabel.innerHTML = "<tr><th>Nama</th><th>NOMANG</th></tr>";
  anggota.forEach(a => {
    const row = tabel.insertRow();
    row.insertCell(0).innerText = a.nama;
    row.insertCell(1).innerText = a.nomang;
  });
}

function isiDropdown() {
  const select = document.getElementById("anggotaSelect");
  select.innerHTML = "";
  anggota.forEach((a, i) => {
    const option = document.createElement("option");
    option.value = i;
    option.text = `${a.nama} (${a.nomang})`;
    select.add(option);
  });
}

function hitungPinjaman() {
  const pinjaman = parseFloat(document.getElementById("pinjaman").value);
  const bulan = parseInt(document.getElementById("bulan").value);
  const bungaPerBulan = 0.02;
  const index = document.getElementById("anggotaSelect").value;
  if (isNaN(pinjaman) || isNaN(bulan) || index === "") {
    alert("Isi semua data pinjaman.");
    return;
  }

  const pokokPerBulan = pinjaman / bulan;
  let sisaPinjaman = pinjaman;
  let hasil = `
    <h3>📝 Detail Angsuran: ${anggota[index].nama}</h3>
    <table>
      <tr>
        <th>Bulan</th>
        <th>Pokok</th>
        <th>Bunga</th>
        <th>Total Angsuran</th>
        <th>Sisa Pinjaman</th>
      </tr>
  `;

  for (let i = 1; i <= bulan; i++) {
    const bunga = sisaPinjaman * bungaPerBulan;
    const total = pokokPerBulan + bunga;
    sisaPinjaman -= pokokPerBulan;

    hasil += `
      <tr>
        <td>${i}</td>
        <td>Rp ${pokokPerBulan.toFixed(2)}</td>
        <td>Rp ${bunga.toFixed(2)}</td>
        <td>Rp ${total.toFixed(2)}</td>
        <td>Rp ${sisaPinjaman > 0 ? sisaPinjaman.toFixed(2) : "0.00"}</td>
      </tr>
    `;
  }

  hasil += `</table>`;
  document.getElementById("hasil").innerHTML = hasil;
}

window.onload = () => {
  tampilkanAnggota();
  isiDropdown();
}
</script>

</body>
</html>
