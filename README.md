# n8n Auto-Responder Email & AI Image Generator

Repositori ini berisi kumpulan workflow otomasi **n8n** untuk **Praktikum Modul 06**:
1. **Topik 1:** Auto-Responder Email NF Academy
2. **Topik 2:** AI Image Generator Workflow (Grok Imagine API & Telegram Bot)

---

## 📁 Berkas Proyek

### 1. `Tugas 1 - Auto-Responder Email NF Academy.json`
Workflow otomatisasi pendaftaran pelatihan SIB Mandiri NF Academy dengan respon email konfirmasi instan:
- **n8n Form Trigger** (`n8n-nodes-base.formTrigger`): Menyediakan form pendaftaran online publik dengan field:
  - `Nama` (Text)
  - `Email` (Email)
  - `Kelas` (Dropdown: AI for Business, Codeless Data Science, DevOps Engineer, Digital Marketing, Fullstack Developer, Mobile Apps Developer).
- **Gmail Node** (`n8n-nodes-base.gmail`): Mengirimkan email konfirmasi berbasis template HTML profesional secara otomatis ke alamat email pendaftar.

### 2. `Tugas 2 - AI Image Generator Workflow.json`
Workflow pembuatan gambar berbasis AI otomatis dari form input teks lalu dikirimkan langsung ke Telegram:
- **Form Trigger** (`n8n-nodes-base.formTrigger`): Mengambil input `prompt`, `task_type`, dan `api_key`.
- **HTTP Request (Create Task)**: Mengirim request generate gambar ke API Kie AI (`model: grok-imagine/text-to-image`, `aspect_ratio: 3:2`).
- **Wait Node**: Menunggu proses rendering gambar selesai (delay 10 detik).
- **HTTP Request (Record Info)**: Memeriksa status proses pembuatan gambar (`recordInfo` polling).
- **If Node**: Memvalidasi status generasi (`state === "success"`).
- **Code Nodes (`Format Result` & `Loop Image`)**: Mengurai data JSON hasil URL gambar menjadi list per item.
- **HTTP Request (Download Gambar)**: Mengunduh gambar sebagai berkas binary.
- **Telegram Node** (`n8n-nodes-base.telegram`): Mengirimkan hasil gambar secara otomatis ke akun / grup Telegram menggunakan operasi `sendPhoto`.

---

## 🚀 Panduan Import ke n8n

1. Buka instance **n8n** Anda.
2. Masuk ke menu **Workflows**.
3. Klik menu **Options** (ikon titik tiga di kanan atas kanvas) lalu pilih **Import from File...** (atau salin isi file JSON dan paste langsung ke kanvas).
4. Pilih file yang ingin diimpor:
   - [`Tugas 1 - Auto-Responder Email NF Academy.json`](./Tugas%201%20-%20Auto-Responder%20Email%20NF%20Academy.json)
   - [`Tugas 2 - AI Image Generator Workflow.json`](./Tugas%202%20-%20AI%20Image%20Generator%20Workflow.json)
5. Konfigurasikan kredensial yang dibutuhkan:
   - **Tugas 1:** Kredensial **Gmail OAuth2 API** pada node *Gmail*.
   - **Tugas 2:** Kredensial **Telegram API** (Bot Token) pada node *Telegram (Kirim Hasil Generate)*.
6. Aktifkan workflow (`Active / Publish`) untuk menggunakan URL form publik.
