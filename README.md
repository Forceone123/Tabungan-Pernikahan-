<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tabungan Pernikahan👩‍❤️‍👨</title>
    <!-- Menambahkan SweetAlert2 -->
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f9f9f9;
            margin: 0;
            padding: 0;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            margin-top: 100px;
        }

        h1 {
            text-align: center;
            color: #333;
        }

        /* LED Box */
        .led-box {
            background-color: #add8e6; /* Biru muda */
            color: #fff; /* Teks putih */
            font-family: 'Courier New', Courier, monospace;
            padding: 10px;
            border-radius: 5px;
            box-shadow: 0 0 10px 2px #add8e6;
            width: fit-content;
            margin: 10px 0;
        }

        /* Running Text */
        #runningText {
            font-size: 18px;
            padding: 10px;
            background-color: #add8e6; /* Biru muda */
            color: #fff; /* Teks putih */
            width: 100%;
            text-align: center;
            overflow: hidden;
            white-space: nowrap;
            box-shadow: 0 0 10px 2px #add8e6;
            position: fixed;
            top: 0;
            left: 0;
            z-index: 1;
        }

        #runningText span {
            display: inline-block;
            padding-left: 100%;
            animation: scrolling 18s linear infinite;
        }

        @keyframes scrolling {
            from {
                transform: translateX(100%);
            }
            to {
                transform: translateX(-100%);
            }
        }

        /* Info Saldo */
        #saldoInfo {
            position: fixed;
            top: 208px;
            left: 130px;
            z-index: 100;
        }

        /* Jam di Pojok Kanan Atas */
        #jam {
            position: fixed;
            top: 5px;
            right: 2px;
            background-color: #ffcccb; /* Pink soft */
            color: #fff; /* Teks putih */
            z-index: 1;
            padding: 5px 10px; /* Dipendekkan lebih lanjut */
            font-size: 12px; /* Ukuran diperkecil lagi */
            font-weight: bold; /* Teks bold */
            border-radius: 5px;
        }

        /* Target Tabungan */
        #target {
            position: fixed;
            bottom: 1000px;
            right: 5px;
            z-index: 100;
            font-size: 10px; /* Ukuran lebih kecil */
            padding: 5px 5px; /* Padding lebih kecil */
            font-weight: bold; /* Teks bold */
            background-color: #add8e6; /* Biru muda */
            color: #fff; /* Teks putih */
            border-radius: 5px;
        }

        form, table {
            margin-top: 20px;
        }

        button {
            background-color: #00b300;
            color: white;
            cursor: pointer;
            padding: 5px;
            margin-top: 10px;
            border-radius: 5px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        th, td {
            border: 1px solid #ddd;
            padding: 10px;
            text-align: left;
        }

        th {
            background-color: #f2f2f2;
        }

        .balance {
            margin-top: 20px;
            padding: 10px;
            background-color: #e0ffe0;
            border-left: 5px solid #00b300;
            font-size: 18px;
            text-align: center;
        }

        input, select {
            width: 100%;
            padding: 8px;
            margin: 5px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        #inputTarget {
            margin-top: 10px;
            width: auto;
        }
    </style>
</head>
<body>

    <!-- Tampilan saat login -->
    <div id="loginPage" style="text-align: center; margin-top: 150px;">
        <h1>Login Dulu Sayang😘💞</h1>
        <label for="username">Username:</label>
        <input type="text" id="username" placeholder="Masukkan username">
        <button id="loginButton">Login</button>
    </div>

    <!-- Tampilan setelah login -->
    <div id="appPage" style="display: none;">
        <div id="runningText">
            <span>Halooooo sayangcuuuu😘💖🥰 kita nabung sama-sama yaaahhh🥳💖✨ buat kita nikah🥰💞💓👩‍❤️‍👨</span>
        </div>

        <div id="jam" class="led-box"></div>
        <div id="saldoInfo" class="led-box">Saldo: Rp <span id="saldo">0</span></div>
        <div id="target" class="led-box">Target Tabungan: Rp <span id="targetAmount">0</span> <br> 
            <input type="number" id="inputTarget" placeholder="Isi target">
            <button onclick="setTarget()">Set Target</button>
        </div>

        <div class="container">
            <h1>Tabungan Pernikahan👩‍❤️‍👨</h1>
            <!-- Form untuk transaksi -->
            <form id="transaction-form">
                <label for="keterangan">Keterangan</label>
                <input type="text" id="keterangan" placeholder="Contoh: Hadiah, Catering" required>

                <label for="jumlah">Jumlah (Rp)</label>
                <input type="number" id="jumlah" placeholder="Masukkan jumlah" required>

                <label for="tipe">Tipe Transaksi</label>
                <select id="tipe">
                    <option value="pemasukan">Pemasukan</option>
                    <option value="pengeluaran">Pengeluaran</option>
                </select>

                <button type="submit">Tambah Transaksi</button>
            </form>

            <!-- Tabel Transaksi -->
            <table>
                <thead>
                    <tr>
                        <th>Keterangan</th>
                        <th>Tipe</th>
                        <th>Jumlah (Rp)</th>
                    </tr>
                </thead>
                <tbody id="transaction-table">
                    <!-- Transaksi akan muncul di sini -->
                </tbody>
            </table>

            <!-- Tombol Reset -->
            <button id="resetButton">Reset Transaksi</button>
        </div>
    </div>

    <script>
        // Variabel untuk menyimpan data
        let saldo = 0;
        let target = 0;
        let transactions = [];
        const saldoEl = document.getElementById('saldo');
        const targetEl = document.getElementById('targetAmount');
        const form = document.getElementById('transaction-form');
        const transactionTable = document.getElementById('transaction-table');

        // Ambil data dari localStorage saat halaman dimuat
        window.onload = function() {
            saldo = localStorage.getItem('saldo') ? parseInt(localStorage.getItem('saldo')) : 0;
            target = localStorage.getItem('target') ? parseInt(localStorage.getItem('target')) : 0;
            transactions = localStorage.getItem('transactions') ? JSON.parse(localStorage.getItem('transactions')) : [];

            updateSaldo();
            updateTarget();
            loadTransactions();
        }

        // Login dan animasi loading menggunakan SweetAlert2
        const loginPage = document.getElementById('loginPage');
        const appPage = document.getElementById('appPage');
        document.getElementById('loginButton').addEventListener('click', function() {
            const username = document.getElementById('username').value;
            if (username === 'dicky' || username === 'silva') {
                // Animasi SweetAlert2
                Swal.fire({
                    title: 'Loading...',
                    text: 'Tunggu sebentar...',
                    allowOutsideClick: false,
                    showConfirmButton: false,
                    timer: 1500,
                    timerProgressBar: true,
                    didOpen: () => {
                        Swal.showLoading();
                    }
                }).then(() => {
                    loginPage.style.display = 'none';
                    appPage.style.display = 'block';
                });
            } else {
                Swal.fire({
                    icon: 'error',
                    title: 'Username salah!',
                    text: 'Coba lagi',
                });
            }
        });

        // Jam dengan emoticon
        function updateJam() {
            const jamEl = document.getElementById('jam');
            const now = new Date();
            const hours = now.getHours();
            let waktu = 'Pagi 🌞';

            if (hours >= 12 && hours < 15) {
                waktu = 'Siang ☀️';
            } else if (hours >= 15 && hours < 18) {
                waktu = 'Sore 🌅';
            } else if (hours >= 18) {
                waktu = 'Malam 🌙';
            }

            jamEl.textContent = `Jam: ${now.toLocaleTimeString()} - ${waktu}`;
        }

        setInterval(updateJam, 1000); // Update setiap detik

        // Fungsi untuk mengupdate saldo
        function updateSaldo() {
            saldoEl.textContent = saldo.toLocaleString('id-ID'); // Format rupiah
            localStorage.setItem('saldo', saldo); // Simpan saldo ke localStorage
        }

        // Fungsi untuk mengupdate target tabungan
        function updateTarget() {
            targetEl.textContent = target.toLocaleString('id-ID'); // Format rupiah
            localStorage.setItem('target', target); // Simpan target ke localStorage
        }

        // Fungsi untuk set target tabungan
        function setTarget() {
            const inputTarget = document.getElementById('inputTarget').value;
            target = parseInt(inputTarget);
            updateTarget();
        }

        // Fungsi untuk menambah transaksi
        form.addEventListener('submit', function(e) {
            e.preventDefault();
            const keterangan = document.getElementById('keterangan').value;
            const jumlah = document.getElementById('jumlah').value;
            const tipe = document.getElementById('tipe').value;

            if (tipe === 'pemasukan') {
                saldo += parseInt(jumlah);
            } else if (tipe === 'pengeluaran') {
                saldo -= parseInt(jumlah);
            }

            // Simpan transaksi ke array
            const transaction = { keterangan, jumlah: parseInt(jumlah), tipe };
            transactions.push(transaction);
            localStorage.setItem('transactions', JSON.stringify(transactions)); // Simpan transaksi ke localStorage

            // Tambahkan transaksi ke tabel
            const row = document.createElement('tr');
            row.innerHTML = `<td>${keterangan}</td><td>${tipe}</td><td>${parseInt(jumlah).toLocaleString('id-ID')}</td>`;
            transactionTable.appendChild(row);

            // Update saldo
            updateSaldo();

            // SweetAlert untuk notifikasi sukses
            Swal.fire({
                title: 'Transaksi Berhasil!',
                text: 'Transaksi Anda telah berhasil ditambahkan.',
                icon: 'success',
                confirmButtonText: 'OK'
            });

            // Reset form
            form.reset();
        });

        // Fungsi untuk memuat transaksi dari localStorage
        function loadTransactions() {
            transactions.forEach(transaction => {
                const row = document.createElement('tr');
                row.innerHTML = `<td>${transaction.keterangan}</td><td>${transaction.tipe}</td><td>${transaction.jumlah.toLocaleString('id-ID')}</td>`;
                transactionTable.appendChild(row);
            });
        }

        // Tombol reset untuk menghapus semua transaksi
        document.getElementById('resetButton').addEventListener('click', function() {
            transactionTable.innerHTML = ''; // Mengosongkan tabel transaksi
            transactions = []; // Kosongkan array transaksi
            localStorage.removeItem('transactions'); // Hapus transaksi dari localStorage
        });

    </script>

</body>
</html>
