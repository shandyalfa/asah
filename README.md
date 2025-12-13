# ChemAI — Novel Chemicals Discovery Agent 🧪

Platform **Agentic AI** untuk eksplorasi dan penemuan kandidat senyawa petrokimia berbasis kriteria fisik & kimia, dengan **reasoning transparan** menggunakan **Google Gemini 2.5 Flash**.

> Status: prototype / research tool (bisa dikembangkan untuk produksi)

---

## 🖼️ Tampilan Aplikasi (Screenshots)

> Tambahkan file gambar ke folder `docs/images/` lalu update path berikut.

<img width="646" height="234" alt="image" src="https://github.com/user-attachments/assets/c2a42715-8bd8-4e8e-957d-09fafd22db02" />
![ChemAI Screenshot 2](docs/images/screenshot-2.png)
<img width="285" height="77" alt="image" src="https://github.com/user-attachments/assets/77f401d3-36fa-4f87-b4ba-3e08f174cd08" />
![ChemAI Screenshot 4](docs/images/screenshot-4.png)

---

## 📖 Deskripsi Proyek

ChemAI adalah aplikasi web full-stack yang mengintegrasikan **Agentic AI Pipeline** untuk membantu peneliti dan praktisi industri petrokimia menemukan senyawa kimia yang sesuai dengan spesifikasi teknis.

Multi-step reasoning yang dilakukan:

1. **Interpretasi Preferensi**: mengubah deskripsi naratif menjadi bobot properti terstruktur  
2. **Filtering Dataset**: menyaring 10,000+ senyawa berdasarkan kriteria numerik & kategorikal  
3. **Relaksasi Kriteria**: melonggarkan parameter otomatis jika tidak ada hasil yang cocok  
4. **Penjelasan Ilmiah**: memberikan rationale berbasis data untuk kandidat yang direkomendasikan  

---

## 🎯 Fitur Utama

- ✨ **Agentic AI Pipeline**: multi-step reasoning dengan Gemini 2.5 Flash  
- 🎤 **Multimodal Input**: voice recognition, PDF upload, file text (`.txt/.csv`)  
- 🔬 **Molecular Visualization**: render struktur 2D dari SMILES (RDKit)  
- 📊 **Agent Trace Transparency**: jejak keputusan AI yang dapat diaudit  
- 💾 **Search History Management**: simpan, review, hapus riwayat pencarian  
- 🔐 **Authentication System**: JWT-based auth + user management  
- 📱 **PWA-Ready**: offline support & caching strategy  
- 🎨 **Responsive Design**: desktop & mobile  

---

## 🛠️ Tech Stack

### Backend
- **FastAPI** (Python 3.10+)
- **Google Gemini 2.5 Flash API** (SDK: `google-genai`)
- **RDKit** (render struktur molekul)
- **SQLite + SQLAlchemy ORM**
- **JWT Auth** (`python-jose`) + hashing password (`passlib[bcrypt]`)
- **Pandas + NumPy** untuk filtering & ranking

### Frontend
- **Vanilla JavaScript (ES6+)**
- **Webpack 5**
- **Workbox (PWA / service worker)**
- **Pure CSS** responsive
- **Web Speech API** (voice)
- **PDF.js** (ekstraksi teks PDF)

---

## ✅ Prerequisites

Pastikan sudah ada:
- **Python 3.10+**
- **Node.js 18+** & **npm 9+**
- **Git**
- **Google Gemini API Key** (Google AI Studio)

Cek versi:
```bash
python --version   # >= 3.10
node --version     # >= 18
npm --version      # >= 9
```

---

## ⚙️ Setup Environment

### 1) Clone repository
```bash
git clone https://github.com/your-username/chemai.git
cd chemai
```

### 2) Backend setup (Python)

#### a) Virtual environment
**Windows**
```bash
python -m venv venv
venv\Scripts\activate
```

**macOS/Linux**
```bash
python3 -m venv venv
source venv/bin/activate
```

#### b) Install dependencies
```bash
pip install --upgrade pip
pip install -r requirements.txt
```

**Catatan dependencies utama:**
- `fastapi`, `uvicorn[standard]`
- `google-genai` (Gemini SDK)
- `pandas`, `numpy<2` (kompatibilitas RDKit)
- `rdkit-pypi`
- `sqlalchemy`
- `python-dotenv`
- `python-jose[cryptography]`, `passlib[bcrypt]`

#### c) (Opsional tapi disarankan) Security dependencies
> Jika belum termasuk di `requirements.txt`, jalankan:
```bash
pip install python-jose[cryptography] passlib[bcrypt]
```

#### d) Environment variables
Buat `.env` dari template:
```bash
cp .env.example .env
```

Isi API key:
```env
# .env
GEMINI_API_KEY=your_actual_gemini_api_key_here
GOOGLE_API_KEY=your_actual_gemini_api_key_here

# Database (default: SQLite)
DATABASE_URL=sqlite:///./chem_ai.db

# Optional: Dataset path
DATA_PATH=data/petrochemical_dataset_10000.csv
```

⚠️ **Penting**
- Jangan commit `.env` (pastikan sudah ada di `.gitignore`)
- API key bersifat rahasia

#### e) Setup database
SQLite akan otomatis dibuat saat aplikasi pertama kali dijalankan:
- File DB: `./chem_ai.db`
- Schema auto-generated dari `models.py`

### 3) Frontend setup (Node.js)
```bash
npm install
```

**Konfigurasi API base URL (opsional)**  
Edit `src/scripts/config.js` jika backend tidak di `localhost:8000`:
```js
export const API_BASE = "http://localhost:8000";
```

---

## 🏃 Menjalankan Aplikasi

### Development mode (recommended)
**Terminal 1 — Backend**
```bash
# pastikan venv aktif
uvicorn agent.main:app --reload --host 0.0.0.0 --port 8000
```

**Terminal 2 — Frontend**
```bash
npm run start-dev
```

Akses:
- Frontend: `http://localhost:3000`
- Swagger: `http://localhost:8000/docs`
- Redoc: `http://localhost:8000/redoc`

### Production mode
1) Build frontend
```bash
npm run build
```

2) Serve production build
```bash
npm run serve
```
Aplikasi: `http://localhost:8080`

3) Jalankan backend production
```bash
uvicorn agent.main:app --host 0.0.0.0 --port 8000 --workers 4
```

---

## 🔑 Penggunaan Aplikasi

### 1) Registrasi
Buka:
- `http://localhost:3000/#/register`

Isi:
- Name
- Email
- Password (min. 8 karakter)

### 2) Login
- `http://localhost:3000/#/login`  
Token JWT disimpan di `localStorage`.

### 3) Mencari kandidat senyawa

#### Input criteria (manual)
Isi parameter pencarian:
- Boiling point range (K)
- Density range (g/cm³)
- Solubility: `high/medium/low`
- Family: `alkane/alkene/aromatic/ketone/alcohol/...`
- Thermal stability (opsional)

#### Input naratif (extra notes)
Contoh:
> Saya butuh solvent polar dengan titik didih menengah (300–350K), cukup larut di air, dan stabil hingga temperatur 400K untuk proses ekstraksi pada suhu tinggi.

Agent akan:
- Mengubah naratif menjadi bobot properti
- Memprioritaskan kandidat sesuai bobot & constraints
- Memberi penjelasan berbasis data

#### Input voice 🎤
- Klik tombol rekam
- Izinkan microphone
- Bicara Bahasa Indonesia
- Stop untuk mengakhiri; teks otomatis masuk ke textarea

> Catatan: Web Speech API biasanya optimal di Chrome/Edge.

#### Input file upload 📄
- `.txt` (plain text)
- `.csv`
- `.pdf` (ekstraksi teks via PDF.js)

### 4) Review hasil
Hasil biasanya menampilkan:
- **Agent Trace** (jejak keputusan)
- **Tabel kandidat** + skor + status match
- **Detail senyawa** (klik row):
  - Struktur 2D (RDKit)
  - Properti lengkap
  - Checklist kesesuaian (✅/❌)
  - Penjelasan ilmiah dari Gemini

### 5) Riwayat pencarian
Panel “Search History”:
- 👁️ View
- 🗑️ Delete

---

## 📚 API Documentation

### Authentication
**POST** `/auth/register`
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "securepass123"
}
```

**POST** `/auth/login` (form-data)
- `username`: email
- `password`

Response:
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "bearer"
}
```

### Chemical Discovery
**POST** `/recommend` (requires auth)
Header:
- `Authorization: Bearer <your_jwt_token>`

Body:
```json
{
  "boiling_point_range": [250, 400],
  "density_range": [0.7, 1.0],
  "solubility": "high",
  "family": "ketone",
  "thermal_stability": null,
  "extra_notes": "Butuh solvent polar untuk ekstraksi"
}
```

**GET** `/history` (requires auth)  
**DELETE** `/history/{id}` (requires auth)

### Structure rendering
**GET** `/structure?smiles=CC(=O)CC`  
Returns: `image/png` (struktur 2D)

> Contoh SMILES di atas adalah **2-butanone**. Silakan ganti sesuai senyawa.

### Metadata
**GET** `/metadata`
```json
{
  "families": ["alkane", "alkene", "aromatic", "ketone"],
  "solubility_options": ["high", "medium", "low"],
  "boiling_point_range": [177.91, 430.97],
  "density_range": [0.757, 0.935]
}
```

---

## 🗂️ Project Structure

```
chemai/
├── agent/                          # Backend Python
│   ├── agents/
│   │   └── novel_discovery_agent.py  # Agentic AI logic
│   ├── services/
│   │   ├── gemini_client.py          # Gemini API integration
│   │   └── structure_renderer.py     # RDKit molecular rendering
│   ├── utils/
│   │   ├── filters.py                # Dataset filtering & ranking
│   │   └── thermal.py                # Thermal stability calculation
│   ├── database/
│   │   ├── models.py                 # SQLAlchemy models
│   │   ├── database.py               # Database config
│   │   ├── auth.py                   # Authentication routes
│   │   └── security.py               # JWT & password hashing
│   └── main.py                       # FastAPI app entry
│
├── src/                            # Frontend
│   ├── scripts/
│   │   ├── app.js
│   │   ├── index.js
│   │   ├── config.js
│   │   ├── sw.js                     # Service worker (PWA)
│   │   ├── pages/
│   │   │   └── agentAI/
│   │   │       └── agenticai.js
│   │   └── data/
│   │       └── api.js
│   ├── styles/
│   │   └── styles.css
│   ├── public/
│   │   ├── images/
│   │   └── manifest.json
│   └── index.html
│
├── data/
│   └── petrochemical_dataset_10000.csv
│
├── dist/                           # Production build (generated)
├── webpack.common.js
├── webpack.dev.js
├── webpack.prod.js
├── package.json
├── requirements.txt
├── .env.example
└── README.md
```

---

## 🧪 Testing

### 1) Backend API
```bash
curl http://localhost:8000/health
curl http://localhost:8000/metadata
```

Register:
```bash
curl -X POST http://localhost:8000/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Test User","email":"test@example.com","password":"testpass123"}'
```

Login:
```bash
curl -X POST http://localhost:8000/auth/login \
  -F "username=test@example.com" \
  -F "password=testpass123"
```

### 2) Molecular rendering
Buka:
- `http://localhost:8000/structure?smiles=CC(=O)CC`

### 3) PWA offline
- DevTools → Application → Service Workers → centang **Offline** → refresh  
Aplikasi seharusnya tetap load dari cache.

---

## 🐛 Troubleshooting

### `GEMINI_API_KEY belum diset`
- Pastikan `.env` ada di root
- Pastikan `GEMINI_API_KEY=...` sudah diisi
- Restart backend

### `ModuleNotFoundError: No module named 'rdkit'`
```bash
pip install rdkit-pypi
```
Jika masih error:
```bash
pip uninstall rdkit rdkit-pypi
pip install rdkit-pypi
```

### Port 8000/3000 sudah dipakai
**macOS/Linux**
```bash
lsof -ti:8000 | xargs kill -9
```

Atau ganti port:
```bash
uvicorn agent.main:app --reload --port 8001
```

### Voice recognition tidak jalan
- Umumnya hanya support Chrome/Edge
- Pastikan permission microphone
- Gunakan `https://` atau `localhost` (konteks aman)

### CORS error
Tambahkan middleware di `agent/main.py`:
```py
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

### `database is locked` (SQLite)
SQLite kurang ideal untuk concurrent writes. Untuk production, gunakan PostgreSQL:
```bash
pip install psycopg2-binary
```
Update `.env`:
```env
DATABASE_URL=postgresql://user:password@localhost/chemai
```

---

## 🚀 Deployment (opsional)

### Frontend
- Vercel / Netlify (static hosting)

### Backend
- Railway / Render / Cloud Run  
Set env:
- `GEMINI_API_KEY`
- `DATABASE_URL` (disarankan PostgreSQL)

Command:
```bash
uvicorn agent.main:app --host 0.0.0.0 --port $PORT
```

---

## 📝 Notes & Limitations

- Proyek ini **tidak** melatih model ML custom (tidak ada `.pkl/.pt/.h5`).  
- AI reasoning menggunakan **Gemini API** (cloud).  
- Filtering/ranking berbasis rule & data processing (Pandas).  
- Dataset `petrochemical_dataset_10000.csv` berisi properti fisik & termal untuk 10,000 senyawa.

---

## 🤝 Contributing

1. Fork repo  
2. Buat branch fitur: `git checkout -b feature/amazing-feature`  
3. Commit: `git commit -m "Add amazing feature"`  
4. Push: `git push origin feature/amazing-feature`  
5. Buat Pull Request  

---

## 📄 License

Distributed under the **ISC License**. Lihat file `LICENSE`.

---

## 👥 Team

**ChemAI Development Team**  
Contact:
- Email: `yourmail@mail.com`
- GitHub: `github.com/your-username/chemai`

---

## 🙏 Acknowledgments

- Google Gemini AI  
- RDKit (open-source cheminformatics toolkit)  
- FastAPI  
- PubChem (chemical database reference)  

---

## 📞 Support

Jika mengalami masalah:
- Cek bagian **Troubleshooting**
- Buka issue di GitHub repository
- Hubungi email tim

Happy Coding! 🚀🧪
