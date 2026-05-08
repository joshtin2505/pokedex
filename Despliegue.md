# Proceso de Despliegue en Azure Static Web Apps

## Plataforma Elegida

**Microsoft Azure Static Web Apps**

Se eligió esta plataforma porque ofrece integración nativa con GitHub Actions para CI/CD automático, certificado SSL gratuito, CDN global, y soporte oficial para aplicaciones Angular sin configuración adicional de servidor.

---

## Pasos para Crear la Cuenta en Azure

1. Ingresar a [https://portal.azure.com/](https://portal.azure.com/)
2. Hacer clic en **"Crear una cuenta gratuita"**
3. Iniciar sesión con una cuenta Microsoft (Outlook, Hotmail, etc.) o crear una nueva
4. Completar el registro con número de teléfono y tarjeta de crédito (solo para verificación, no se cobra en el plan gratuito)
5. Una vez dentro del portal, se tiene acceso a todos los servicios de Azure

---

## Proceso de Despliegue Paso a Paso

### Paso 1 — Preparar el Repositorio en GitHub

```bash
# Inicializar git en el proyecto (si no existe)
git init
git add .
git commit -m "Initial commit"

# Subir a GitHub
git remote add origin https://github.com/tu-usuario/pokedex-angular.git
git push -u origin main
```

### Paso 2 — Crear el Recurso en Azure

1. En el portal de Azure, hacer clic en **"Crear un recurso"**
2. Buscar **"Static Web Apps"** y seleccionarlo
3. Hacer clic en **"Crear"**
4. Completar el formulario:
   - **Suscripción:** Azure for Students (o la disponible)
   - **Grupo de recursos:** Crear nuevo → `rg-pokedex`
   - **Nombre:** `pokedex-angular`
   - **Plan de hospedaje:** Free
   - **Región:** East US 2 (o la más cercana)
5. En la sección **"Detalles de implementación"**, seleccionar **GitHub**
6. Autorizar a Azure a acceder a GitHub
7. Seleccionar el repositorio y la rama `main`

### Paso 3 — Configurar el Build

Azure detecta Angular automáticamente. Verificar que los valores sean:

| Campo | Valor |
|---|---|
| Valor preestablecido de compilación | Angular |
| Ubicación de la aplicación | `/` |
| Ubicación de la API | *(vacío)* |
| Ubicación de salida | `dist/pokedex-angular/browser` |

8. Hacer clic en **"Revisar y crear"** → **"Crear"**

### Paso 4 — GitHub Actions (CI/CD Automático)

Azure crea automáticamente un archivo de workflow en el repositorio:

```
.github/workflows/azure-static-web-apps-xxxx.yml
```

Cada `git push` a `main` dispara el pipeline automáticamente. El proceso toma aproximadamente 2-3 minutos.

### Paso 5 — Verificar el Despliegue

1. En el portal de Azure, ir al recurso creado
2. Copiar la **URL pública** generada (ej: `https://black-flower-0247b8610.7.azurestaticapps.net/`)
3. Abrir en el navegador y verificar que la app carga correctamente

---

## Configuración de Cabeceras de Seguridad

Para mejorar la calificación en [securityheaders.com](https://securityheaders.com), se configuró el archivo `staticwebapp.config.json` dentro de la carpeta `src/`:

```json
{
  "globalHeaders": {
    "Content-Security-Policy": "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; font-src 'self' data:; connect-src 'self' https://pokeapi.co https://raw.githubusercontent.com;",
    "X-Frame-Options": "SAMEORIGIN",
    "Permissions-Policy": "camera=(), microphone=(), geolocation=()",
    "Strict-Transport-Security": "max-age=10886400; includeSubDomains; preload",
    "Referrer-Policy": "same-origin",
    "X-Content-Type-Options": "nosniff"
  }
}
```

Se registró el archivo en `angular.json` para que sea incluido en el build:

```json
"assets": [
  "src/favicon.ico",
  "src/assets",
  {
    "glob": "staticwebapp.config.json",
    "input": "src",
    "output": "/"
  }
]
```

---

## Errores Encontrados y Soluciones

### ❌ Error 1 — Ruta incorrecta del archivo de configuración

**Mensaje:**
```
An unhandled exception occurred: The staticwebapp.config.json asset
path must start with the project source root.
```

**Causa:** El archivo `staticwebapp.config.json` fue colocado en la raíz del proyecto en lugar de dentro de `src/`.

**Solución:** Mover el archivo a `src/staticwebapp.config.json` y actualizar `angular.json` con la configuración de assets mostrada arriba.

---

### ❌ Error 2 — Calificación C en securityheaders.com

**Causa:** Faltaban las cabeceras `Content-Security-Policy`, `X-Frame-Options` y `Permissions-Policy`.

**Solución:** Agregar el archivo `staticwebapp.config.json` con todas las cabeceras necesarias. Después del nuevo despliegue, la calificación subió a **A+**.

---

## Resultado Final

| Ítem | Resultado |
|---|---|
| URL pública | https://black-flower-0247b8610.7.azurestaticapps.net/ |
| Estado HTTP | 200 OK |
| HTTPS | ✅ Activo (certificado automático de Azure) |
| Calificación de seguridad | **A+** en securityheaders.com |
| CI/CD | ✅ GitHub Actions activo |

---

## Comandos Útiles

```bash
# Build de producción local
ng build --configuration production

# Ver logs del pipeline en GitHub
# GitHub → repositorio → Actions → ver el workflow más reciente

# Forzar re-despliegue
git commit --allow-empty -m "trigger redeploy"
git push
```