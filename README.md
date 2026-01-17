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

```bash
npm run build
```

Build output akan berada di folder `dist/` dan siap untuk di-deploy.

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