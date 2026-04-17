<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kalkulator PPh by Adien</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --island-bg: #000000;
            --primary-accent: #00f2fe; 
            --secondary-accent: #4facfe; 
            --bg-body: #0f172a;
            --text-dark: #1e293b;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-body);
            background-image: radial-gradient(at 0% 0%, rgba(79, 172, 254, 0.15) 0, transparent 50%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
            color: white;
            overflow-x: hidden;
        }

        /* --- OVERLAY README --- */
        #readmeOverlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(15, 23, 42, 0.95);
            backdrop-filter: blur(10px);
            z-index: 9999;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.5s ease;
        }

        .readme-content {
            background: #ffffff;
            color: #1e293b;
            padding: 40px;
            border-radius: 32px;
            max-width: 500px;
            width: 90%;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5);
            animation: slideUp 0.6s cubic-bezier(0.23, 1, 0.32, 1);
        }

        @keyframes slideUp { from { opacity: 0; transform: translateY(50px); } to { opacity: 1; transform: translateY(0); } }

        .readme-content h2 { font-weight: 800; margin-bottom: 15px; color: #000; }
        .readme-content p { font-size: 14px; line-height: 1.6; color: #64748b; margin-bottom: 20px; }
        .readme-content ul { list-style: none; margin-bottom: 25px; }
        .readme-content li { font-size: 13px; margin-bottom: 8px; display: flex; align-items: center; }
        .readme-content li::before { content: "✓"; color: #10b981; font-weight: bold; margin-right: 10px; }

        .btn-close-readme {
            width: 100%;
            padding: 16px;
            background: #000;
            color: #fff;
            border: none;
            border-radius: 16px;
            font-weight: 700;
            cursor: pointer;
            transition: 0.3s;
        }

        .btn-close-readme:hover { transform: scale(1.02); background: #334155; }

        /* --- KALKULATOR UTAMA --- */
        .container {
            background: #ffffff;
            padding: 40px;
            border-radius: 32px;
            box-shadow: 0 40px 100px -20px rgba(0,0,0,0.4);
            width: 100%;
            max-width: 450px;
            color: var(--text-dark);
            position: relative;
        }

        .dynamic-island {
            width: 140px;
            height: 35px;
            background: var(--island-bg);
            border-radius: 20px;
            margin: -20px auto 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 11px;
            font-weight: 600;
            letter-spacing: 0.5px;
            transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .island-active { width: 260px; height: 45px; background: var(--primary-accent); color: #000; }

        h1 { text-align: center; font-size: 24px; font-weight: 800; margin-bottom: 25px; }

        .input-box { margin-bottom: 20px; }
        label { display: block; font-size: 11px; font-weight: 700; color: #94a3b8; text-transform: uppercase; margin-bottom: 8px; }
        
        input {
            width: 100%; padding: 16px; border: 2px solid #f1f5f9; border-radius: 18px;
            font-size: 16px; font-weight: 600; background: #f8fafc; transition: 0.3s;
        }

        input:focus { outline: none; border-color: var(--secondary-accent); background: #fff; box-shadow: 0 10px 20px -5px rgba(79, 172, 254, 0.2); }

        .btn-calc { width: 100%; padding: 18px; background: var(--island-bg); color: white; border: none; border-radius: 18px; font-size: 16px; font-weight: 700; cursor: pointer; transition: 0.3s; }

        .result-area { margin-top: 30px; display: none; animation: expand 0.5s ease forwards; }
        @keyframes expand { from { opacity: 0; transform: scale(0.98); } to { opacity: 1; transform: scale(1); } }

        .res-card { background: #f8fafc; padding: 25px; border-radius: 24px; border: 1px solid #f1f5f9; }
        .res-total-val { font-size: 30px; font-weight: 900; color: var(--text-dark); text-align: center; display: block; margin: 10px 0; }
        .monthly-label { text-align: center; font-size: 14px; color: var(--secondary-accent); font-weight: 700; }
    </style>
</head>
<body>
    
    <div id="readmeOverlay">
        <div class="readme-content">
            <h2>📖 Panduan Penggunaan</h2>
            <p>Selamat datang di <b>Kalkulator PPh by Adien</b>. Alat ini menghitung estimasi Pajak Penghasilan orang pribadi berdasarkan tarif progresif terbaru.</p>
            <ul>
                <li>Masukkan penghasilan bruto (kotor) setahun.</li>
                <li>Pilih jumlah tanggungan (untuk hitung PTKP).</li>
                <li>Tekan <b>Enter</b> atau tombol hitung.</li>
                <li>Perhitungan sesuai lapisan tarif 5% sampai 35%.</li>
            </ul>
            <button class="btn-close-readme" onclick="closeReadme()">Siap, Masuk ke Kalkulator</button>
        </div>
    </div>

    <div class="container">
        <div class="dynamic-island" id="island">ADIEN TAX PRO</div>
        <h1>Kalkulator PPh by Adien</h1>

        <div class="input-box">
            <label>Penghasilan Setahun (Bruto)</label>
            <input type="text" id="gajiDisplay" placeholder="Rp 0" oninput="formatInputRupiah(this)">
            <input type="hidden" id="gaji">
        </div>

        <div class="input-box">
            <label>Jumlah Tanggungan</label>
            <input type="number" id="tanggungan" min="0" value="0">
        </div>

        <button class="btn-calc" onclick="hitungPajak()">Hitung Pajak</button>

        <div id="hasil" class="result-area">
            <div class="res-card">
                <div style="display: flex; justify-content: space-between; font-size: 13px; margin-bottom: 8px;">
                    <span>Penghasilan Kena Pajak:</span>
                    <span id="resPkp" style="font-weight: 700;"></span>
                </div>
                <hr style="border: 0; border-top: 1px dashed #cbd5e1; margin: 15px 0;">
                <label style="text-align: center; display: block;">Total Pajak Tahunan</label>
                <span class="res-total-val" id="resPajakTahun"></span>
                <div class="monthly-label">± <span id="resPajakBulan"></span> / Bulan</div>
            </div>
        </div>
    </div>

    <script>
        // Tutup Readme
        function closeReadme() {
            document.getElementById('readmeOverlay').style.opacity = '0';
            setTimeout(() => {
                document.getElementById('readmeOverlay').style.display = 'none';
            }, 500);
        }

        // Enter untuk hitung
        document.addEventListener('keypress', function (e) {
            if (e.key === 'Enter' && document.getElementById('readmeOverlay').style.display === 'none') { 
                hitungPajak(); 
            }
        });

        function formatInputRupiah(input) {
            let value = input.value.replace(/[^,\d]/g, '');
            document.getElementById('gaji').value = value;
            if (value) {
                input.value = new Intl.NumberFormat('id-ID', {
                    style: 'currency', currency: 'IDR', minimumFractionDigits: 0
                }).format(parseInt(value));
            }
        }

        function hitungPajak() {
            const gaji = parseFloat(document.getElementById('gaji').value) || 0;
            const tanggungan = parseInt(document.getElementById('tanggungan').value) || 0;
            const island = document.getElementById('island');

            if (gaji <= 0) {
                island.innerText = "ISI GAJI DULU";
                island.style.background = "#ff4757";
                setTimeout(() => { island.innerText = "ADIEN TAX PRO"; island.style.background = "#000"; }, 2000);
                return;
            }

            island.classList.add('island-active');
            island.innerText = "CALCULATING...";

            setTimeout(() => {
                const ptkp = 54000000 + (tanggungan * 4500000);
                const pkp = Math.max(0, gaji - ptkp);

                // Logika sesuai data Excel Anda
                const D3 = 60000000, D4 = 190000000, D5 = 250000000, D6 = 500000000, C7 = 5000000000;
                let pajak = (Math.min(pkp, D3) * 0.05) +
                            (Math.max(Math.min(pkp - D3, D4), 0) * 0.15) +
                            (Math.max(Math.min(pkp - D5, D5), 0) * 0.25) +
                            (Math.max(Math.min(pkp - D6, D6), 0) * 0.30) +
                            (Math.max(pkp - C7, 0) * 0.35);

                const fmt = (n) => new Intl.NumberFormat('id-ID', { 
                    style: 'currency', currency: 'IDR', minimumFractionDigits: 0 
                }).format(n);

                document.getElementById('hasil').style.display = 'block';
                document.getElementById('resPkp').innerText = fmt(pkp);
                document.getElementById('resPajakTahun').innerText = fmt(pajak);
                document.getElementById('resPajakBulan').innerText = fmt(pajak/12);

                island.innerText = "SUCCESS ✨";
                setTimeout(() => {
                    island.classList.remove('island-active');
                    island.innerText = "ADIEN TAX PRO";
                }, 1500);
            }, 600);
        }
    </script>
</body>
</html>
