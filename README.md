# Vue.js Application

A modern Vue.js app built with Vite, Vue 3 Composition API, and Vuetify 3.

## Features

- ⚡️ Vite for fast build tools
- 🎨 Vuetify 3 for Material Design components
- 📱 Mobile-friendly and responsive
- ✨ Counter with increment/decrement
- ✅ Todo List with add, toggle, and remove features
- 📊 Todo statistics
- 🎯 Material Design Icons (MDI)

## Installation

```bash
npm install
```

## Running the Application

```bash
npm run dev
```

The application will run on `http://localhost:3000`

## Build for Production

### Manual Build

```bash
npm run build
```

Build output akan berada di folder `dist/` dan siap untuk di-deploy.

### Using Deployment Script

Script `deploy` menyediakan cara mudah untuk build dan deploy aplikasi:

```bash
# Build dan deploy (default)
./deploy

# Hanya build, skip deployment
./deploy --build-only

# Hanya deploy (menggunakan build yang sudah ada)
./deploy --deploy-only

# Deploy tanpa sudo (jika sudah punya permission)
./deploy --no-sudo

# Lihat bantuan
./deploy --help
```

**Deployment Steps:**

Script akan melakukan langkah-langkah berikut:
1. Build production aplikasi
2. Hapus directory `/var/www/html/imagex` (jika ada)
3. Copy hasil build dari `dist/` ke `/var/www/html/imagex`

**Konfigurasi Deployment:**

Edit file `deploy` untuk mengubah path deployment:

```bash
DEPLOY_PATH="/var/www/html/imagex"  # Path deployment
USE_SUDO=true  # Set false jika tidak perlu sudo
```

**Catatan:** Script akan menggunakan `sudo` untuk menulis ke `/var/www/html/`. Pastikan user memiliki permission sudo atau set `USE_SUDO=false` jika sudah punya write permission.

**Catatan:** Aplikasi dikonfigurasi untuk berjalan di subpath `/imagex/`. Pastikan server web dikonfigurasi untuk serve aplikasi di `https://domain.com/imagex/`.

## Preview Build

```bash
npm run preview
```

Untuk preview dengan base path yang benar:
```bash
npm run preview -- --base /imagex/
```

## Technology Used

- Vue 3 (Composition API)
- Vite
- Vuetify 3 (Material Design Framework)
- Material Design Icons (MDI)

## Project Structure

```
prompt/
├── src/
│ ├── App.vue # Main component
│ ├── main.js # Entry point
│ └── style.css # Global styles
├── index.html # HTML template
├── vite.config.js # Vite configuration
└── package.json # Dependencies
```