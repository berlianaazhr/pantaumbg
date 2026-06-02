# pantaumbg
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PantauMBG - Sistem Pengawasan Makan Bergizi Gratis</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js CDN -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Lucide Icons (via unpkg for modern look) -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap');
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
        }
        .sidebar-active {
            background-color: rgba(16, 185, 129, 0.1);
            border-left: 4px solid #10b981;
            color: #047857;
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 antialiased">

    <!-- Container Utama App -->
    <div class="flex min-h-screen">
        
        <!-- SIDEBAR NAVIGASI -->
        <aside class="w-64 bg-white border-r border-slate-200 flex flex-col justify-between hidden md:flex">
            <div>
                <!-- Logo & Brand -->
                <div class="p-6 border-b border-slate-100 flex items-center gap-3">
                    <div class="bg-emerald-600 text-white p-2 rounded-xl shadow-md shadow-emerald-200">
                        <i data-lucide="shield-check" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <h1 class="font-bold text-lg text-slate-900 leading-tight">PantauMBG</h1>
                        <p class="text-xs text-emerald-600 font-medium tracking-wider uppercase">Sistem Pengawasan</p>
                    </div>
                </div>

                <!-- Profile Singkat User Aktif -->
                <div class="p-4 mx-4 my-3 bg-slate-50 rounded-xl border border-slate-100 flex items-center gap-3">
                    <div class="w-10 h-10 bg-emerald-100 text-emerald-700 rounded-full flex items-center justify-center font-bold text-sm">
                        SD1
                    </div>
                    <div class="overflow-hidden">
                        <h4 class="font-semibold text-sm text-slate-900 truncate" id="user-display-name">SDN 01 Menteng</h4>
                        <p class="text-xs text-slate-500 truncate" id="user-role-display">User Sekolah (Petugas)</p>
                    </div>
                </div>

                <!-- Menu Navigasi -->
                <nav class="px-3 space-y-1 mt-4">
                    <button onclick="switchTab('dashboard')" id="btn-dashboard" class="sidebar-item w-full flex items-center gap-3 px-4 py-3 text-sm font-medium rounded-lg text-slate-600 hover:bg-slate-50 transition-all sidebar-active">
                        <i data-lucide="layout-dashboard" class="w-4 h-4"></i>
                        <span>Dashboard Utama</span>
                    </button>
                    
                    <button onclick="switchTab('verifikasi')" id="btn-verifikasi" class="sidebar-item w-full flex items-center gap-3 px-4 py-3 text-sm font-medium rounded-lg text-slate-600 hover:bg-slate-50 transition-all">
                        <i data-lucide="clipboard-check" class="w-4 h-4"></i>
                        <span>Verifikasi Harian</span>
                    </button>
                    
                    <button onclick="switchTab('pengaduan')" id="btn-pengaduan" class="sidebar-item w-full flex items-center gap-3 px-4 py-3 text-sm font-medium rounded-lg text-slate-600 hover:bg-slate-50 transition-all">
                        <i data-lucide="alert-triangle" class="w-4 h-4"></i>
                        <span>Aduan & Kendala</span>
                        <span class="ml-auto bg-rose-100 text-rose-700 text-xs px-2 py-0.5 rounded-full font-bold" id="badge-aduan">1</span>
                    </button>

                    <button onclick="switchTab('riwayat')" id="btn-riwayat" class="sidebar-item w-full flex items-center gap-3 px-4 py-3 text-sm font-medium rounded-lg text-slate-600 hover:bg-slate-50 transition-all">
                        <i data-lucide="history" class="w-4 h-4"></i>
                        <span>Riwayat Log</span>
                    </button>
                    
                    <div class="pt-4 pb-2 px-4 text-xs font-semibold text-slate-400 uppercase tracking-wider">Simulasi Role</div>
                    
                    <button onclick="toggleRole('sekolah')" id="role-sekolah" class="w-full flex items-center gap-2 px-4 py-2 text-xs font-medium rounded-md bg-emerald-50 text-emerald-700 border border-emerald-200">
                        <span class="w-2 h-2 rounded-full bg-emerald-500"></span>
                        <span>Mode: Petugas Sekolah</span>
                    </button>
                    <button onclick="toggleRole('pusat')" id="role-pusat" class="w-full flex items-center gap-2 px-4 py-2 text-xs font-medium rounded-md bg-slate-100 text-slate-600 hover:bg-slate-200 mt-1">
                        <span class="w-2 h-2 rounded-full bg-slate-400"></span>
                        <span>Mode: Satgas / Pusat</span>
                    </button>
                </nav>
            </div>

            <!-- Footer Sidebar -->
            <div class="p-4 border-t border-slate-100">
                <div class="text-xs text-slate-400 text-center">
                    PantauMBG v1.0.0 &copy; 2026
                </div>
            </div>
        </aside>

        <!-- KONTEN UTAMA (Kanan) -->
        <main class="flex-1 flex flex-col min-w-0">
            
            <!-- HEADER ATAS -->
            <header class="h-16 bg-white border-b border-slate-200 flex items-center justify-between px-6 z-10">
                <div class="flex items-center gap-4">
                    <button class="md:hidden text-slate-600 p-1 rounded-lg hover:bg-slate-100">
                        <i data-lucide="menu" class="w-6 h-6"></i>
                    </button>
                    <h2 class="font-bold text-lg text-slate-800" id="page-title">Dashboard Pengawasan</h2>
                </div>
                
                <!-- Status & Aksi Cepat -->
                <div class="flex items-center gap-4">
                    <div class="bg-amber-50 border border-amber-200 text-amber-800 text-xs px-3 py-1.5 rounded-lg flex items-center gap-2 font-medium">
                        <span class="w-2 h-2 rounded-full bg-amber-500 animate-pulse"></span>
                        <span>Hari Ini: Menu Paket B (Ayam Saus Madu + Sayur)</span>
                    </div>
                    <div class="h-8 w-px bg-slate-200"></div>
                    <div class="flex items-center gap-2 text-sm font-medium text-slate-700">
                        <i data-lucide="calendar" class="w-4 h-4 text-slate-400"></i>
                        <span id="current-date-text">Selasa, 2 Juni 2026</span>
                    </div>
                </div>
            </header>

            <!-- AREA ISI DASHBOARD (Dynamic Content) -->
            <div class="p-6 overflow-y-auto flex-1 max-w-7xl w-full mx-auto space-y-6">
                
                <!-- Toast Notification Container -->
                <div id="toast-container" class="hidden fixed top-20 right-6 bg-emerald-600 text-white px-4 py-3 rounded-xl shadow-lg flex items-center gap-3 z-50 transition-all duration-300">
                    <i data-lucide="check-circle" class="w-5 h-5"></i>
                    <span id="toast-message" class="text-sm font-medium">Data berhasil disimpan!</span>
                </div>

                <!-- ================= TAB 1: DASHBOARD UTAMA ================= -->
                <section id="tab-dashboard" class="space-y-6">
                    
                    <!-- KPI CARDS GRID -->
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-5">
                        <!-- Card 1 -->
                        <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm flex items-start justify-between">
                            <div class="space-y-2">
                                <p class="text-xs font-semibold text-slate-500 uppercase tracking-wider">Total Porsi Hari Ini</p>
                                <h3 class="text-2xl font-bold text-slate-900" id="kpi-porsi">345 Porsi</h3>
                                <p class="text-xs text-emerald-600 font-medium flex items-center gap-1">
                                    <i data-lucide="arrow-up" class="w-3 h-3"></i> 100% Sesuai Kuota Siswa
                                </p>
                            </div>
                            <div class="bg-emerald-50 text-emerald-600 p-3 rounded-xl">
                                <i data-lucide="utensils" class="w-5 h-5"></i>
                            </div>
                        </div>

                        <!-- Card 2 -->
                        <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm flex items-start justify-between">
                            <div class="space-y-2">
                                <p class="text-xs font-semibold text-slate-500 uppercase tracking-wider">Status Kedatangan</p>
                                <h3 class="text-2xl font-bold text-slate-900" id="kpi-jam">09:45 WIB</h3>
                                <p class="text-xs text-emerald-600 font-medium flex items-center gap-1">
                                    <span class="w-1.5 h-1.5 rounded-full bg-emerald-500"></span> Tepat Waktu (Batas 10:30)
                                </p>
                            </div>
                            <div class="bg-blue-50 text-blue-600 p-3 rounded-xl">
                                <i data-lucide="clock" class="w-5 h-5"></i>
                            </div>
                        </div>

                        <!-- Card 3 -->
                        <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm flex items-start justify-between">
                            <div class="space-y-2">
                                <p class="text-xs font-semibold text-slate-500 uppercase tracking-wider">Skor Kelayakan Mutu</p>
                                <h3 class="text-2xl font-bold text-slate-900" id="kpi-mutu">98.4%</h3>
                                <p class="text-xs text-slate-500 font-medium">Berdasarkan uji sampel organoleptik</p>
                            </div>
                            <div class="bg-amber-50 text-amber-600 p-3 rounded-xl">
                                <i data-lucide="star" class="w-5 h-5"></i>
                            </div>
                        </div>

                        <!-- Card 4 -->
                        <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm flex items-start justify-between">
                            <div class="space-y-2">
                                <p class="text-xs font-semibold text-slate-500 uppercase tracking-wider">Aduan Belum Selesai</p>
                                <h3 class="text-2xl font-bold text-slate-900" id="kpi-aduan">1 Kasus</h3>
                                <p class="text-xs text-rose-600 font-medium flex items-center gap-1" id="kpi-aduan-desc">
                                    <i data-lucide="alert-circle" class="w-3 h-3"></i> Perlu Tindak Lanjut Segera
                                </p>
                            </div>
                            <div class="bg-rose-50 text-rose-600 p-3 rounded-xl">
                                <i data-lucide="shield-alert" class="w-5 h-5"></i>
                            </div>
                        </div>
                    </div>

                    <!-- CHARTS & SUMMARY GRAPH -->
                    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                        <!-- Grafik Batang Realisasi -->
                        <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm lg:col-span-2">
                            <div class="flex items-center justify-between mb-4">
                                <div>
                                    <h4 class="font-bold text-slate-900 text-base">Ketepatan Distribusi Makanan Harian</h4>
                                    <p class="text-xs text-slate-500">Perbandingan kuota siswa vs porsi riil terverifikasi</p>
                                </div>
                                <span class="text-xs bg-slate-100 px-2.5 py-1 rounded-md font-medium text-slate-600">7 Hari Terakhir</span>
                            </div>
                            <div class="h-64 relative">
                                <canvas id="chartDistribusi"></canvas>
                            </div>
                        </div>

                        <!-- Panel Quick Status & Tindakan -->
                        <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm flex flex-col justify-between">
                            <div>
                                <h4 class="font-bold text-slate-900 text-base mb-3">Status Pengiriman Hari Ini</h4>
                                
                                <!-- Timeline Proses Ringkas -->
                                <div class="space-y-4">
                                    <div class="flex gap-3 items-start">
                                        <div class="bg-emerald-100 text-emerald-700 p-1.5 rounded-full mt-0.5">
                                            <i data-lucide="check" class="w-3.5 h-3.5"></i>
                                        </div>
                                        <div>
                                            <p class="text-xs font-semibold text-slate-800">08:15 - Makanan Selesai Dimasak</p>
                                            <p class="text-xs text-slate-400">Dapur Umum Zona Pusat Jakarta</p>
                                        </div>
                                    </div>
                                    <div class="flex gap-3 items-start">
                                        <div class="bg-emerald-100 text-emerald-700 p-1.5 rounded-full mt-0.5">
                                            <i data-lucide="check" class="w-3.5 h-3.5"></i>
                                        </div>
                                        <div>
                                            <p class="text-xs font-semibold text-slate-800">09:10 - Kurir Berangkat (In Transit)</p>
                                            <p class="text-xs text-slate-400">Armada Mobil Box #04 (Suhu Box: 65°C)</p>
                                        </div>
                                    </div>
                                    <div class="flex gap-3 items-start">
                                        <div class="bg-emerald-100 text-emerald-700 p-1.5 rounded-full mt-0.5" id="step-3-icon">
                                            <i data-lucide="check" class="w-3.5 h-3.5"></i>
                                        </div>
                                        <div>
                                            <p class="text-xs font-semibold text-slate-800" id="step-3-title">09:45 - Tiba & Diverifikasi Sekolah</p>
                                            <p class="text-xs text-slate-400" id="step-3-desc">Oleh Petugas Lapangan: Ahmad Supardi</p>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <div class="pt-4 border-t border-slate-100 mt-4">
                                <button onclick="switchTab('verifikasi')" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-medium text-sm py-2.5 px-4 rounded-xl transition-all shadow-md shadow-emerald-100 flex items-center justify-center gap-2">
                                    <i data-lucide="scan-face" class="w-4 h-4"></i>
                                    <span>Input Hasil Cek Hari Ini</span>
                                </button>
                            </div>
                        </div>
                    </div>

                    <!-- DATA TABLE: DAFTAR KENDALA AKTIF -->
                    <div class="bg-white rounded-2xl border border-slate-200 shadow-sm overflow-hidden">
                        <div class="p-5 border-b border-slate-100 flex items-center justify-between">
                            <div>
                                <h4 class="font-bold text-slate-900 text-base">Aduan / Masalah yang Sedang Diproses</h4>
                                <p class="text-xs text-slate-500">Daftar laporan kritis dari lapangan yang memerlukan mitigasi</p>
                            </div>
                            <span class="bg-rose-50 text-rose-700 text-xs font-semibold px-2.5 py-1 rounded-full">Butuh Respons Cepat</span>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="w-full text-left border-collapse">
                                <thead class="bg-slate-50 text-slate-400 text-xs font-semibold uppercase tracking-wider border-b border-slate-100">
                                    <tr>
                                        <th class="p-4">ID Tiket</th>
                                        <th class="p-4">Kategori Masalah</th>
                                        <th class="p-4">Pelapor</th>
                                        <th class="p-4">Waktu Lapor</th>
                                        <th class="p-4">Tingkat Keparahan</th>
                                        <th class="p-4">Status Alur</th>
                                    </tr>
                                </thead>
                                <tbody class="text-sm divide-y divide-slate-100" id="dashboard-aduan-table">
                                    <tr>
                                        <td class="p-4 font-semibold text-slate-900">#MBG-9402</td>
                                        <td class="p-4">
                                            <div class="font-medium text-slate-800">Susu Kemasan Bocor / Rusak</div>
                                            <div class="text-xs text-slate-400">Ditemukan 12 kotak susu penyok dan merembes</div>
                                        </td>
                                        <td class="p-4 text-slate-600">Panitia Sekolah (Bu Rina)</td>
                                        <td class="p-4 text-slate-500">Hari Ini, 10:02 WIB</td>
                                        <td class="p-4">
                                            <span class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-full text-xs font-medium bg-amber-50 text-amber-800 border border-amber-200">
                                                <span class="w-1.5 h-1.5 bg-amber-500 rounded-full"></span> Sedang (Medium)
                                            </span>
                                        </td>
                                        <td class="p-4">
                                            <span class="inline-flex items-center gap-1 px-2.5 py-1 rounded-md text-xs font-semibold bg-blue-50 text-blue-700">
                                                Investigasi Dapur Umum
                                            </span>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </section>


                <!-- ================= TAB 2: INPUT VERIFIKASI HARIAN ================= -->
                <section id="tab-verifikasi" class="hidden space-y-6">
                    <div class="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm max-w-3xl mx-auto">
                        <div class="border-b border-slate-100 pb-4 mb-6">
                            <h3 class="font-bold text-lg text-slate-900">Lembar QC & Verifikasi Penerimaan Makanan</h3>
                            <p class="text-xs text-slate-500">Wajib diisi oleh perwakilan komite sekolah/guru sesaat setelah makanan diserahterimakan dari kurir.</p>
                        </div>

                        <form id="form-qc-harian" onsubmit="handleQCVermit(event)" class="space-y-5">
                            <!-- Kuantitas -->
                            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-xs font-semibold uppercase tracking-wider text-slate-500 mb-2">Jumlah Porsi Target (Sesuai Data)</label>
                                    <input type="number" value="345" disabled class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl font-medium text-slate-500 cursor-not-allowed">
                                </div>
                                <div>
                                    <label class="block text-xs font-semibold uppercase tracking-wider text-slate-700 mb-2">Jumlah Porsi Riil Diterima <span class="text-rose-500">*</span></label>
                                    <input type="number" id="input-riil-porsi" required value="345" class="w-full p-3 bg-white border border-slate-200 rounded-xl font-medium focus:ring-2 focus:ring-emerald-500 focus:outline-none">
                                </div>
                            </div>

                            <!-- Parameter Kualitas Kelayakan -->
                            <div>
                                <label class="block text-xs font-semibold uppercase tracking-wider text-slate-700 mb-3">Uji Kelayakan Organoleptik & Fisik</label>
                                <div class="space-y-2 bg-slate-50 p-4 rounded-xl border border-slate-100">
                                    <label class="flex items-center gap-3 bg-white p-3 rounded-lg border border-slate-200 cursor-pointer hover:bg-slate-50">
                                        <input type="checkbox" checked required class="w-4 h-4 text-emerald-600 border-slate-300 rounded focus:ring-emerald-500">
                                        <div>
                                            <span class="text-sm font-semibold text-slate-800">Suhu Makanan Hangat</span>
                                            <p class="text-xs text-slate-400">Makanan dikirim menggunakan thermal box terisolasi dengan baik.</p>
                                        </div>
                                    </label>
                                    <label class="flex items-center gap-3 bg-white p-3 rounded-lg border border-slate-200 cursor-pointer hover:bg-slate-50">
                                        <input type="checkbox" checked required class="w-4 h-4 text-emerald-600 border-slate-300 rounded focus:ring-emerald-500">
                                        <div>
                                            <span class="text-sm font-semibold text-slate-800">Aroma & Rasa Segar (Tidak Basi)</span>
                                            <p class="text-xs text-slate-400">Uji acak sampel rasa membuktikan nasi dan lauk dalam kondisi prima.</p>
                                        </div>
                                    </label>
                                    <label class="flex items-center gap-3 bg-white p-3 rounded-lg border border-slate-200 cursor-pointer hover:bg-slate-50">
                                        <input type="checkbox" checked required class="w-4 h-4 text-emerald-600 border-slate-300 rounded focus:ring-emerald-500">
                                        <div>
                                            <span class="text-sm font-semibold text-slate-800">Kemasan Segel & Higienis</span>
                                            <p class="text-xs text-slate-400">Wadah tidak robek, penutup rapat, sendok higienis lengkap tersedia.</p>
                                        </div>
                                    </label>
                                </div>
                            </div>

                            <!-- Bukti Foto Korlap -->
                            <div>
                                <label class="block text-xs font-semibold uppercase tracking-wider text-slate-700 mb-2">Unggah Foto Sampel Makanan & Berita Acara <span class="text-rose-500">*</span></label>
                                <div class="border-2 border-dashed border-slate-200 rounded-xl p-6 text-center hover:bg-slate-50 transition-all cursor-pointer">
                                    <i data-lucide="camera" class="w-8 h-8 text-slate-400 mx-auto mb-2"></i>
                                    <span class="text-sm font-medium text-emerald-600 block">Klik untuk ambil foto atau pilih berkas</span>
                                    <span class="text-xs text-slate-400 block mt-1">Format JPG, PNG (Maks. 5MB) dengan geotag otomatis aktif</span>
                                </div>
                            </div>

                            <!-- Catatan Tambahan -->
                            <div>
                                <label class="block text-xs font-semibold uppercase tracking-wider text-slate-700 mb-2">Catatan/Keterangan Tambahan</label>
                                <textarea id="input-catatan" rows="3" placeholder="Contoh: Pengiriman lancar, buah pisang diganti jeruk manis atas persetujuan Satgas karena kendala stok vendor." class="w-full p-3 bg-white border border-slate-200 rounded-xl text-sm focus:ring-2 focus:ring-emerald-500 focus:outline-none"></textarea>
                            </div>

                            <!-- Tombol Kirim Dokumen -->
                            <div class="pt-4">
                                <button type="submit" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-3 px-6 rounded-xl shadow-lg shadow-emerald-100 transition-all flex items-center justify-center gap-2">
                                    <i data-lucide="send" class="w-4 h-4"></i>
                                    <span>Kirim Verifikasi Ke Pusat Data</span>
                                </button>
                            </div>
                        </form>
                    </div>
                </section>


                <!-- ================= TAB 3: PENGADUAN & KENDALA ================= -->
                <section id="tab-pengaduan" class="hidden space-y-6">
                    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                        <!-- Formulir Pengaduan (Kiri) -->
                        <div class="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm lg:col-span-1 h-fit">
                            <div class="border-b border-slate-100 pb-3 mb-4">
                                <h3 class="font-bold text-base text-slate-900">Form Lapor Kendala Lapangan</h3>
                                <p class="text-xs text-slate-500">Gunakan sistem ini jika terjadi ketidaksesuaian fatal untuk pelacakan akuntabilitas.</p>
                            </div>
                            
                            <form id="form-aduan" onsubmit="handleCreateAduan(event)" class="space-y-4">
                                <div>
                                    <label class="block text-xs font-semibold uppercase tracking-wider text-slate-700 mb-1">Kategori Masalah</label>
                                    <select id="aduan-kategori" class="w-full p-2.5 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500">
                                        <option value="Keterlambatan Logistik">Keterlambatan Pengiriman (>30 Menit)</option>
                                        <option value="Kerusakan Mutu Makanan">Kualitas Makanan (Basi/Hambar/Ulat)</option>
                                        <option value="Kekurangan Porsi">Kuantitas Kurang Berdasarkan Absen</option>
                                        <option value="Lainnya">Lain-lain</option>
                                    </select>
                                </div>

                                <div>
                                    <label class="block text-xs font-semibold uppercase tracking-wider text-slate-700 mb-1">Tingkat Kegentingan</label>
                                    <div class="grid grid-cols-3 gap-2">
                                        <label class="p-2 border border-slate-200 rounded-xl text-center cursor-pointer hover:bg-slate-50 flex items-center justify-center gap-1">
                                            <input type="radio" name="urgensi" value="Rendah" checked class="text-emerald-600">
                                            <span class="text-xs font-medium text-slate-700">Rendah</span>
                                        </label>
                                        <label class="p-2 border border-slate-200 rounded-xl text-center cursor-pointer hover:bg-slate-50 flex items-center justify-center gap-1">
                                            <input type="radio" name="urgensi" value="Sedang" class="text-amber-600">
                                            <span class="text-xs font-medium text-slate-700">Sedang</span>
                                        </label>
                                        <label class="p-2 border border-slate-200 rounded-xl text-center cursor-pointer hover:bg-slate-50 flex items-center justify-center gap-1">
                                            <input type="radio" name="urgensi" value="Tinggi" class="text-rose-600">
                                            <span class="text-xs font-medium text-slate-700">Tinggi</span>
                                        </label>
                                    </div>
                                </div>

                                <div>
                                    <label class="block text-xs font-semibold uppercase tracking-wider text-slate-700 mb-1">Kronologi Singkat Masalah</label>
                                    <textarea id="aduan-deskripsi" rows="4" required placeholder="Jelaskan detail masalah (misal: Kurir baru tiba jam 11:15 WIB sehingga anak-anak kelaparan)." class="w-full p-2.5 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500"></textarea>
                                </div>

                                <button type="submit" class="w-full bg-rose-600 hover:bg-rose-700 text-white font-bold py-2.5 px-4 rounded-xl shadow-md shadow-rose-100 text-sm transition-all flex items-center justify-center gap-2">
                                    <i data-lucide="megaphone" class="w-4 h-4"></i>
                                    <span>Kirim Aduan (Open Ticket)</span>
                                </button>
                            </form>
                        </div>

                        <!-- Daftar Tiket Log (Kanan) -->
                        <div class="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm lg:col-span-2">
                            <h3 class="font-bold text-base text-slate-900 mb-4">Semua Alur Tiket Pengaduan Sekolah</h3>
                            
                            <div class="space-y-4" id="list-tiket-aduan">
                                <!-- Item 1 -->
                                <div class="p-4 border border-slate-200 rounded-xl space-y-3 bg-slate-50">
                                    <div class="flex items-center justify-between flex-wrap gap-2">
                                        <div class="flex items-center gap-2">
                                            <span class="text-sm font-bold text-slate-900">#MBG-9402</span>
                                            <span class="bg-amber-100 text-amber-800 text-xs font-medium px-2 py-0.5 rounded-full border border-amber-200">Susu Kemasan Bocor</span>
                                        </div>
                                        <span class="text-xs text-slate-400">Hari ini, 10:02 WIB</span>
                                    </div>
                                    <p class="text-sm text-slate-600">Ditemukan 12 kotak susu penyok dan merembes saat pembongkaran dari truk termal.</p>
                                    <div class="flex items-center justify-between border-t border-slate-200 pt-2 text-xs">
                                        <div class="text-slate-500">Status: <span class="font-bold text-blue-600">Investigasi Dapur Umum</span></div>
                                        <div class="text-slate-400">Pelapor: Bu Rina (Guru Kelas 3)</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </section>


                <!-- ================= TAB 4: RIWAYAT LOG ================= -->
                <section id="tab-riwayat" class="hidden space-y-6">
                    <div class="bg-white rounded-2xl border border-slate-200 shadow-sm overflow-hidden">
                        <div class="p-5 border-b border-slate-100 flex items-center justify-between">
                            <div>
                                <h4 class="font-bold text-slate-900 text-base">Arsip Riwayat Pengiriman & Penerimaan</h4>
                                <p class="text-xs text-slate-500">Lembar rekaman data historis mingguan sebagai basis audit BPK</p>
                            </div>
                            <button onclick="alert('Mengekspor data ke format Excel/CSV...')" class="border border-slate-200 hover:bg-slate-50 text-slate-700 px-3 py-1.5 rounded-xl text-xs font-semibold flex items-center gap-1.5 transition-all">
                                <i data-lucide="download" class="w-3.5 h-3.5"></i> Export Excel
                            </button>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="w-full text-left border-collapse">
                                <thead class="bg-slate-50 text-slate-400 text-xs font-semibold uppercase tracking-wider border-b border-slate-100">
                                    <tr>
                                        <th class="p-4">Tanggal</th>
                                        <th class="p-4">Menu Terjadwal</th>
                                        <th class="p-4">Porsi (Target vs Riil)</th>
                                        <th class="p-4">Waktu Sampai</th>
                                        <th class="p-4">Status QC Sekolah</th>
                                        <th class="p-4">Keterangan</th>
                                    </tr>
                                </thead>
                                <tbody class="text-sm divide-y divide-slate-100">
                                    <tr>
                                        <td class="p-4 font-medium text-slate-900">01 Juni 2026</td>
                                        <td class="p-4">Paket A (Ikan Bakar + Sup Sayur)</td>
                                        <td class="p-4 font-semibold">345 / 345</td>
                                        <td class="p-4 text-slate-600">09:50 WIB</td>
                                        <td class="p-4"><span class="px-2 py-1 text-xs font-bold rounded-md bg-emerald-50 text-emerald-700">LOLOS QC</span></td>
                                        <td class="p-4 text-slate-400 text-xs">Suhu stabil 62°C, kemasan rapi.</td>
                                    </tr>
                                    <tr>
                                        <td class="p-4 font-medium text-slate-900">29 Mei 2026</td>
                                        <td class="p-4">Paket D (Daging Sapi Teriyaki + Capcay)</td>
                                        <td class="p-4 font-semibold">345 / 345</td>
                                        <td class="p-4 text-slate-600">10:10 WIB</td>
                                        <td class="p-4"><span class="px-2 py-1 text-xs font-bold rounded-md bg-emerald-50 text-emerald-700">LOLOS QC</span></td>
                                        <td class="p-4 text-slate-400 text-xs">Penerimaan aman terkendali.</td>
                                    </tr>
                                    <tr>
                                        <td class="p-4 font-medium text-slate-900">28 Mei 2026</td>
                                        <td class="p-4">Paket C (Telur Balado + Tumis Buncis)</td>
                                        <td class="p-4 font-semibold">345 / 340</td>
                                        <td class="p-4 text-slate-600">10:45 WIB</td>
                                        <td class="p-4"><span class="px-2 py-1 text-xs font-bold rounded-md bg-rose-50 text-rose-700">TERKENDALA</span></td>
                                        <td class="p-4 text-slate-500 text-xs font-medium">Kurang 5 porsi, kurir kirim susulan jam 11:10.</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </section>

            </div>
        </main>
    </div>

    <!-- SCRIPT LOGIKA JAVASCRIPT INTEGRATED -->
    <script>
        // Inisialisasi Ikon Lucide di awal load halaman
        lucide.createIcons();

        // Data Management Simulasi (State)
        let appRole = 'sekolah'; // 'sekolah' atau 'pusat'
        let chartInstance = null;

        // Pindah Tab Manajemen
        function switchTab(tabName) {
            // Sembunyikan semua section tab
            document.getElementById('tab-dashboard').classList.add('hidden');
            document.getElementById('tab-verifikasi').classList.add('hidden');
            document.getElementById('tab-pengaduan').classList.add('hidden');
            document.getElementById('tab-riwayat').classList.add('hidden');

            // Hapus kelas aktif di sidebar menu
            document.querySelectorAll('.sidebar-item').forEach(item => {
                item.classList.remove('sidebar-active');
            });

            // Tampilkan tab terpilih
            document.getElementById('tab-' + tabName).classList.remove('hidden');
            document.getElementById('btn-' + tabName).classList.add('sidebar-active');

            // Judul Header Dinamis
            const titleMap = {
                'dashboard': 'Dashboard Pengawasan Utama',
                'verifikasi': 'Validasi & Sertifikasi Mutu Harian',
                'pengaduan': 'Pusat Pengaduan & Manajemen Masalah',
                'riwayat': 'Arsip Histori Audit Distribusi'
            };
            document.getElementById('page-title').innerText = titleMap[tabName];
            
            // Re-render chart jika masuk dashboard agar tidak bug dimensi canvas
            if(tabName === 'dashboard') {
                renderCharts();
            }
        }

        // Simulasi Pergantian Peran Pengguna (Role Simulation)
        function toggleRole(role) {
            appRole = role;
            const btnSekolah = document.getElementById('role-sekolah');
            const btnPusat = document.getElementById('role-pusat');
            const nameDisp = document.getElementById('user-display-name');
            const roleDisp = document.getElementById('user-role-display');

            if (role === 'sekolah') {
                btnSekolah.className = "w-full flex items-center gap-2 px-4 py-2 text-xs font-medium rounded-md bg-emerald-50 text-emerald-700 border border-emerald-200";
                btnPusat.className = "w-full flex items-center gap-2 px-4 py-2 text-xs font-medium rounded-md bg-slate-100 text-slate-600 hover:bg-slate-200 mt-1";
                nameDisp.innerText = "SDN 01 Menteng";
                roleDisp.innerText = "User Sekolah (Petugas)";
                
                // Reset KPI versi Sekolah
                document.getElementById('kpi-porsi').innerText = "345 Porsi";
                document.getElementById('kpi-jam').innerText = "09:45 WIB";
                document.getElementById('kpi-mutu').innerText = "98.4%";
            } else {
                btnPusat.className = "w-full flex items-center gap-2 px-4 py-2 text-xs font-medium rounded-md bg-emerald-50 text-emerald-700 border border-emerald-200";
                btnSekolah.className = "w-full flex items-center gap-2 px-4 py-2 text-xs font-medium rounded-md bg-slate-100 text-slate-600 hover:bg-slate-200 mt-1";
                nameDisp.innerText = "Satgas Pusat MBG";
                roleDisp.innerText = "Super Admin (Pengawas)";
                
                // Ubah KPI versi agregasi Nasional/Pusat
                document.getElementById('kpi-porsi').innerText = "1.24 Miliar";
                document.getElementById('kpi-jam').innerText = "94.2% Tepat";
                document.getElementById('kpi-mutu').innerText = "96.8%";
            }
            showToast("Berhasil beralih profil peran ke: " + roleDisp.innerText);
            switchTab('dashboard');
        }

        // Toast Helper
        function showToast(message) {
            const toast = document.getElementById('toast-container');
            document.getElementById('toast-message').innerText = message;
            toast.classList.remove('hidden');
            setTimeout(() => {
                toast.classList.add('hidden');
            }, 3500);
        }

        // Submit Lembar Verifikasi QC Makanan
        function handleQCVermit(e) {
            e.preventDefault();
            const riilPorsi = document.getElementById('input-riil-porsi').value;
            const catatan = document.getElementById('input-catatan').value;

            // Perbarui KPI riil di dashboard utama sebagai simulasi reaktif
            document.getElementById('kpi-porsi').innerText = riilPorsi + " Porsi";
            
            // Perbarui visual timeline step harian
            document.getElementById('step-3-title').innerText = "Selesai Diverifikasi Sekolah Baru Saja";
            document.getElementById('step-3-desc').innerText = "Status: Lolos Standar Kelayakan Gizi Mutu";

            showToast("Sukses! Laporan Berita Acara QC Makanan terkirim ke Blockchain & DB Pusat.");
            document.getElementById('form-qc-harian').reset();
            switchTab('dashboard');
        }

        // Submit Pembuatan Tiket Pengaduan Baru (Whistleblowing)
        function handleCreateAduan(e) {
            e.preventDefault();
            const kat = document.getElementById('aduan-kategori').value;
            const desk = document.getElementById('aduan-deskripsi').value;
            const urgensi = document.querySelector('input[name="urgensi"]:checked').value;

            // Generate template row baru
            const listContainer = document.getElementById('list-tiket-aduan');
            const dashboardTable = document.getElementById('dashboard-aduan-table');
            
            const tiketId = "#MBG-" + Math.floor(1000 + Math.random() * 9000);
            
            // Badge warna urgensi
            let colorBadge = "bg-rose-50 text-rose-800 border-rose-200";
            if(urgensi === 'Sedang') colorBadge = "bg-amber-50 text-amber-800 border-amber-200";
            if(urgensi === 'Rendah') colorBadge = "bg-slate-50 text-slate-800 border-slate-100";

            // Tambahkan ke halaman aduan
            const newTiketHTML = `
                <div class="p-4 border border-rose-200 rounded-xl space-y-3 bg-rose-50/40 animate-pulse">
                    <div class="flex items-center justify-between flex-wrap gap-2">
                        <div class="flex items-center gap-2">
                            <span class="text-sm font-bold text-slate-900">${tiketId}</span>
                            <span class="bg-rose-100 text-rose-700 text-xs font-medium px-2 py-0.5 rounded-full border border-rose-200">${kat}</span>
                        </div>
                        <span class="text-xs text-slate-400">Baru Saja</span>
                    </div>
                    <p class="text-sm text-slate-600">${desk}</p>
                    <div class="flex items-center justify-between border-t border-slate-200 pt-2 text-xs">
                        <div class="text-slate-500">Status: <span class="font-bold text-rose-600">Menunggu Respon Satgas</span></div>
                        <div class="text-slate-400">Urgensi: ${urgensi}</div>
                    </div>
                </div>
            `;
            listContainer.insertAdjacentHTML('afterbegin', newTiketHTML);

            // Tambahkan juga ke tabel ringkasan dashboard depan
            const newRowHTML = `
                <tr class="bg-rose-50/30">
                    <td class="p-4 font-semibold text-slate-900">${tiketId}</td>
                    <td class="p-4">
                        <div class="font-medium text-slate-800">${kat}</div>
                        <div class="text-xs text-slate-400 truncate max-w-xs">${desk}</div>
                    </td>
                    <td class="p-4 text-slate-600">SDN 01 Menteng</td>
                    <td class="p-4 text-slate-500">Baru Saja</td>
                    <td class="p-4">
                        <span class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-full text-xs font-medium ${colorBadge}">
                            <span class="w-1.5 h-1.5 bg-rose-500 rounded-full"></span> ${urgensi}
                        </span>
                    </td>
                    <td class="p-4">
                        <span class="inline-flex items-center gap-1 px-2.5 py-1 rounded-md text-xs font-semibold bg-rose-100 text-rose-700">
                            Baru Terkirim
                        </span>
                    </td>
                </tr>
            `;
            dashboardTable.insertAdjacentHTML('afterbegin', newRowHTML);

            // Update Counter Badge
            document.getElementById('badge-aduan').innerText = "2";
            document.getElementById('kpi-aduan').innerText = "2 Kasus";
            document.getElementById('kpi-aduan-desc').innerHTML = `<i data-lucide='alert-circle' class='w-3 h-3'></i> Kritis & Butuh Respons!`;
            
            lucide.createIcons();
            showToast("Aduan Kritis berhasil dilayangkan ke sistem pusat pelacakan aduan.");
            document.getElementById('form-aduan').reset();
            switchTab('dashboard');
        }

        // Render Data Visualization via Chart.js
        function renderCharts() {
            const ctx = document.getElementById('chartDistribusi').getContext('2array') || document.getElementById('chartDistribusi');
            
            // Destory instance lama jika ada biar tidak tumpang tindih saat ganti menu/role
            if (chartInstance) {
                chartInstance.destroy();
            }

            chartInstance = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Rabu', 'Kamis', 'Jumat', 'Senin', 'Selasa (Hari Ini)'],
                    datasets: [
                        {
                            label: 'Kuota Target Siswa',
                            data: [345, 345, 345, 345, 345],
                            backgroundColor: '#cbd5e1',
                            borderRadius: 6
                        },
                        {
                            label: 'Realisasi Makanan Diterima',
                            data: [345, 340, 345, 345, 345],
                            backgroundColor: '#10b981',
                            borderRadius: 6
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: { font: { family: 'Plus Jakarta Sans', size: 11 } }
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: false,
                            min: 300,
                            grid: { borderDash: [4, 4] }
                        },
                        x: { grid: { display: false } }
                    }
                }
            });
        }

        // Run otomatis saat web pertama kali diakses
        window.onload = function() {
            renderCharts();
        };
    </script>
</body>
</html>
