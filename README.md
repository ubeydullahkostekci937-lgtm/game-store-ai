# GameStore AI — Oyun Mağazası ve Gömülü Arcade Kütüphanesi (P0006 + P0014)

**Yapay Zeka Destekli Yazılım Geliştirme** dersi (`buraksenyurt/ai-based-development-lecture`) kapsamında geliştirilen bu proje, **P0006 (Oyun Mağazası / Steam Clone)** ile **P0014 (2048 & Yılan Clone)** projelerini tek bir mimaride birleştirir. Kullanıcılar mağaza kataloğundan oyunları kütüphanelerine ekleyebilir, **Yılan (Snake)** ve **2048** oyunlarını doğrudan tarayıcıda (HTML5 Canvas) oynayabilir ve oyun bitiminde skorlarını otomatik olarak `.NET Clean Architecture` API'sine kaydedebilir.

## Mimari ve Aşamalar

| Aşama | Klasör / Dosya | Açıklama |
| :--- | :--- | :--- |
| **1. Docker ve Altyapı** | `docker-compose.yml`, `scripts/init-db.sql` | PostgreSQL 16 (`gamestoredb` + `sonardb`) ve SonarQube LTS servislerini tek komutla ayağa kaldırır. |
| **2. .NET Clean Architecture API** | `backend/GameStore.*`, `insomnia/insomnia-collection.json` | Domain, Application, Infrastructure ve Minimal API katmanları; `/api/games`, `/api/library`, `POST /api/scores` uçları ve hazır Insomnia test koleksiyonu. |
| **3. Next.js Mağaza & Canvas Oyunları** | `frontend/components/SnakeGame.tsx`, `Game2048.tsx`, `app/page.tsx` | Mağaza/kütüphane arayüzü ve oyun bitiminde skoru API'ye gönderen gömülü Yılan ve 2048 HTML5 Canvas bileşenleri. |
| **4. Birim Testleri ve SonarQube** | `backend/GameStore.Tests/`, `scripts/run-sonarqube.sh` | xUnit birim testleri ve SonarQube statik kod kalitesi tarama betiği. |
| **5. LM Studio Yerel AI Katmanı** | `backend/GameStore.Application/Services/AiAssistantService.cs` | LM Studio (`http://localhost:1234/v1/chat/completions`) üzerinden oyuncu skorlarına göre strateji/oyun önerisi ve yorum özetleme servisi. |

## Hızlı Başlangıç

### 1. Docker Altyapısını Başlatın
```bash
docker compose up -d
```
- **PostgreSQL:** `localhost:5432` (`postgres` / `Password123!`)
- **SonarQube Paneli:** `http://localhost:9000` (Varsayılan giriş: `admin` / `admin`)

### 2. Backend API ve Insomnia Doğrulaması
```bash
cd backend/GameStore.Api
dotnet run
```
- `insomnia/insomnia-collection.json` dosyasını **Insomnia** içerisine aktarın (*Import -> From File*) ve 6 hazır isteği (`GET /api/games`, `POST /api/scores`, `POST /api/ai/recommend` vb.) doğrudan test edin.

### 3. Next.js Mağaza ve Gömülü Oyunları Çalıştırın
```bash
cd frontend
npm install
npm run dev
```
- Tarayıcıda `http://localhost:3000` adresini açarak kütüphaneden **Neon Yılan** ve **2048 Strateji Küpleri** oyunlarını oynayın.

### 4. xUnit Testleri ve SonarQube Taraması
```bash
chmod +x scripts/run-sonarqube.sh
./scripts/run-sonarqube.sh
```

### 5. GitHub Deposuna Gönderim (Push)
```bash
git init
git add .
git commit -m "feat: initial clean architecture backend, nextjs canvas games, tests and docker setup"
git branch -M main
git remote add origin https://github.com/ubeydullahkostekci937-lgtm/game-store-ai.git
git push -u origin main
```
