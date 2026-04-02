Softrack-Sena es una aplicación web progresiva (PWA) desarrollada para instructores del SENA que permite gestionar y hacer seguimiento a los aprendices en Etapa Productiva. Se conecta directamente con Google Drive instalado en el PC para leer y contabilizar los archivos de bitácoras (F-147) y seguimientos (F-023) sin necesidad de servidores externos ni configuraciones complejas.

✨ Características

📊 Dashboard en tiempo real con métricas KPI y barras de avance
📁 Conexión directa con Google Drive desde el PC (sin OAuth ni API keys)
📄 Conteo automático de archivos F-147 Bitácoras y F-023 Seguimientos
⚠️ Alertas inteligentes por contratos sin firmar y vencimientos próximos
📊 Exportación a Excel con 4 hojas: Resumen, Aprendices, Drive y Alertas
📱 PWA instalable como app de escritorio sin necesidad de navegador
💾 Persistencia local — los datos se guardan entre sesiones
🔍 Búsqueda y filtros por nombre, grupo, nivel y estado


🚀 Instalación
Opción A — Usar directamente (sin instalar nada)
bash# 1. Clona el repositorio
git clone https://github.com/TU_USUARIO/Softrack-Sena.git

# 2. Instala el servidor estático (solo una vez)
npm install -g serve

# 3. Abre la app
serve Softrack-Sena/dist -p 3000
Luego abre Chrome en http://localhost:3000

Opción B — Modo desarrollo (para modificar el código)
bash# 1. Clona el repositorio
git clone https://github.com/TU_USUARIO/Softrack-Sena.git
cd Softrack-Sena

# 2. Instala las dependencias
npm install

# 3. Corre en modo desarrollo con hot-reload
npm run dev
# → http://localhost:5173

# 4. Compilar para producción
npm run build

Opción C — Lanzador automático (.bat)
Descarga AprendiTrack.bat, ponlo al lado de la carpeta del proyecto y haz doble clic. Instala todo automáticamente y abre Chrome.
📁 MisDocumentos/
    ├── 📁 Softrack-Sena/
    └── ▶ AprendiTrack.bat

📖 Uso
1. Conectar Google Drive
Softrack-Sena lee los archivos directamente desde tu PC usando la File System Access API del navegador.

Instala Google Drive para escritorio y sincroniza tu carpeta de aprendices
En la app, haz clic en "Conectar con Google Drive" en el sidebar izquierdo
Navega hasta tu carpeta raíz (ejemplo: H:\Mi unidad\SEGUIMIENTO ETAPA PRODUCTIVA)
Selecciónala — la app escaneará automáticamente todas las fichas

2. Estructura de carpetas esperada en Drive
SEGUIMIENTO ETAPA PRODUCTIVA/
    ├── FICHA 3068389/
    │     ├── NOMBRE APELLIDO APRENDIZ/
    │     │     ├── F-147 - BITACORAS/
    │     │     │     └── bitacora_01.xlsx ...
    │     │     └── F-023 - SEGUIMIENTO/
    │     │           └── seguimiento_01.docx ...
    │     └── OTRO APRENDIZ/
    ├── FICHA 3147234/
    ├── FICHA 3147235/
    └── FICHA 3147251/

💡 Los nombres de carpetas son flexibles — el sistema usa búsqueda inteligente (fuzzy match) por palabras del nombre, acepta cualquier formato de F-147 o F-023.

3. Instalar como app de escritorio (PWA)

Abre la app en Chrome via http://localhost:3000
En la barra de direcciones aparece el ícono ⊕ Instalar
Haz clic → Instalar AprendiTrack
La app queda en el escritorio y se abre sin navegador


📊 Funcionalidades
MóduloDescripción📊 DashboardKPIs generales, barras de avance por ficha, alertas rápidas👥 AprendicesListado completo con búsqueda, filtros y edición🗂 Grupos / FichasVista por ficha con progreso de contratos y archivos Drive📄 F-147 BitácorasConteo real de archivos desde Drive con fecha del último✅ F-023 SeguimientosConteo de seguimientos con acceso directo a carpetas⚠️ AlertasSin contrato · Vencidos · Críticos (<30 días) · Próximos (<60 días)📈 RAPsAvance académico por Resultado de Aprendizaje📤 ExportarExcel .xlsx con 4 hojas + CSV por categoría

🗂 Estructura del proyecto
Softrack-Sena/
├── dist/                    ← Versión compilada lista para usar
├── public/
│   └── favicon.svg
├── src/
│   ├── App.jsx              ← Shell principal y routing
│   ├── main.jsx
│   ├── store/
│   │   └── index.js         ← Estado global (Zustand)
│   ├── components/
│   │   ├── UI.jsx           ← Componentes reutilizables
│   │   ├── Sidebar.jsx      ← Navegación lateral
│   │   ├── Modals.jsx       ← Modales (detalle, nuevo, editar)
│   │   └── Toast.jsx        ← Notificaciones
│   ├── pages/
│   │   ├── Dashboard.jsx    ← Página principal
│   │   └── OtherPages.jsx   ← Resto de páginas
│   ├── utils/
│   │   ├── config.js        ← Constantes y configuración
│   │   ├── data.js          ← Datos de los 95 aprendices
│   │   ├── helpers.js       ← Funciones utilitarias puras
│   │   ├── driveFS.js       ← File System Access API
│   │   └── exportExcel.js   ← Exportación .xlsx
│   └── styles/
│       └── global.css       ← Estilos globales
├── index.html
├── package.json
└── vite.config.js           ← Configuración Vite + PWA

🛠 Tecnologías
TecnologíaVersiónUsoReact19Framework UIVite8Bundler y dev serverZustand5Estado global reactivoxlsx0.18Exportación a Excelvite-plugin-pwa1.2PWA e instalación de escritorioFile System Access API—Lectura directa de carpetas locales

⚙️ Configuración
Edita src/utils/config.js para personalizar:
js// Número de archivos requeridos por aprendiz
export const BIT_REQUIRED = 12;   // F-147 Bitácoras
export const SEG_REQUIRED = 4;    // F-023 Seguimientos

// Datos del instructor
export const INSTRUCTOR = {
  nombre:   'Tu Nombre',
  iniciales: 'TN',
  rol:      'Instructor SENA'
};

// Fichas de formación
export const PROG_MAP = {
  '3068389': 'Programación de Software',
  '3147234': 'Análisis y Desarrollo — A',
  // Agrega más fichas aquí...
};

🔧 Agregar nuevos aprendices
Opción 1 — Desde la app: clic en + Nuevo en cualquier página, llena el formulario. Los datos se guardan en localStorage.
Opción 2 — Editar el código: abre src/utils/data.js y agrega el aprendiz al array APRENDICES_DATA:
js{
  nombre:       "NOMBRE APELLIDO",       // en MAYÚSCULAS
  doc:          "1234567890",
  programa:     "ANALISIS Y DESARROLLO DE SOFTWARE",
  nivel:        "TÉCNICO",               // o "TECNÓLOGO"
  grupo:        "3147234",
  correo:       "correo@gmail.com",
  inicio_ep:    "2026-02-10",            // formato YYYY-MM-DD
  fin_contrato: "2026-08-10",            // null si no tiene
  sem:          "G"                      // G=Vigente Y=Terminado R=Sin contrato
}

📋 Requisitos

Node.js 18 o superior
Chrome 86+ o Edge 86+ (requerido para File System Access API)
Google Drive para escritorio instalado y sincronizado
Windows 10/11

autor:
Gian Andrei Gómez
Instructor SENA — Centro CMLTI
Bogotá, Colombia
