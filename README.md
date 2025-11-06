🛡️ Proyecto: DevSecOps CI/CD - Node & Docker Security Scan
📘 Descripción General

Este proyecto implementa una pipeline DevSecOps completa utilizando GitHub Actions, Node.js y Snyk para integrar análisis de seguridad automatizado tanto en el código fuente como en las imágenes Docker.

El objetivo principal es detectar vulnerabilidades en dependencias y contenedores durante la fase de integración continua (CI), garantizando que el código desplegado sea seguro, actualizado y conforme a buenas prácticas de seguridad.

⚙️ Tecnologías Utilizadas
Herramienta	Descripción
Node.js 18	Entorno de ejecución para la aplicación base.
Docker	Para construir y escanear la imagen del proyecto.
Snyk CLI	Analiza vulnerabilidades en dependencias npm y en imágenes Docker.
GitHub Actions	Automatiza la integración continua (CI) y los escaneos de seguridad.
YAML Workflow	Define los pasos del pipeline dentro de .github/workflows/devsecops.yml.
📂 Estructura del Proyecto
devsecops-lab/
├── .github/
│   └── workflows/
│       └── devsecops.yml        # Archivo principal del pipeline CI/CD
├── Dockerfile                   # Imagen Docker base
├── package.json                 # Dependencias del proyecto Node.js
├── package-lock.json
└── README.md                    # Documentación del proyecto

🚀 Pipeline de Seguridad (GitHub Actions)

El flujo de trabajo CI/CD está definido en .github/workflows/devsecops.yml:

name: DevSecOps CI - Node & Docker Security Scan

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  security-scan:
    runs-on: ubuntu-latest
    name: Ejecutar escaneo de seguridad con Snyk

    steps:
      - name: Checkout del repositorio
        uses: actions/checkout@v4

      - name: Configurar Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18

      - name: Instalar dependencias del proyecto
        run: npm install

      - name: Instalar Snyk CLI
        run: npm install -g snyk

      - name: Autenticarse con Snyk
        run: snyk auth ${{ secrets.SNYK_TOKEN }}

      - name: Escanear vulnerabilidades en dependencias (npm)
        run: snyk test --severity-threshold=low

      - name: Construir imagen Docker
        run: docker build -t devsecops-lab .

      - name: Escanear vulnerabilidades en imagen Docker
        run: snyk container test devsecops-lab --severity-threshold=low

      - name: Monitorear vulnerabilidades (subir a Snyk Dashboard)
        run: snyk monitor

🔐 Configuración de Snyk

Crea una cuenta gratuita en https://snyk.io

Obtén tu token en Account Settings → API Token

En tu repositorio de GitHub:

Ve a Settings → Secrets → Actions

Crea un nuevo secreto:

Name: SNYK_TOKEN
Value: <tu_token_aquí>

🧪 Comandos Útiles
1. Instalar dependencias
npm install

2. Ejecutar análisis de seguridad local
snyk test

3. Escanear la imagen Docker
docker build -t devsecops-lab .
snyk container test devsecops-lab

4. Actualizar dependencias vulnerables
npm audit fix --force

5. Subir resultados al dashboard de Snyk
snyk monitor

🧰 Flujo de Trabajo Típico
# 1. Clonar el repositorio
git clone https://github.com/xluisx14/devsecops-lab.git
cd devsecops-lab

# 2. Instalar dependencias
npm install

# 3. Ejecutar análisis local con Snyk
npm install -g snyk
snyk auth <tu_token>
snyk test

# 4. Hacer commit de los cambios
git add .
git commit -m "Fix vulnerabilities"

# 5. Subir al repositorio
git push origin main


El pipeline CI/CD se ejecutará automáticamente al hacer push o pull request hacia la rama main.

🧩 Resultados del Pipeline

✅ Instalación Node.js

✅ Instalación de dependencias

✅ Escaneo de dependencias con Snyk

✅ Construcción de imagen Docker

✅ Escaneo de vulnerabilidades en Docker

✅ Monitoreo y subida a Dashboard de Snyk

✅ Pipeline completado con éxito

📊 Ventajas del Proyecto

✔️ Automatización total del análisis de seguridad
✔️ Detección temprana de vulnerabilidades
✔️ Integración con GitHub Actions y Snyk Dashboard
✔️ Código listo para escalar hacia CI/CD completo (testing, build, deploy)
✔️ 100% funcional para entornos de laboratorio o producción

✍️ Autor

Luis Perez Y Natalia Buendia
Estudiante de Ingeniería de Sistemas
📍 Colombia
💻 DevSecOps | Cloud | Backend | Ciberseguridad
