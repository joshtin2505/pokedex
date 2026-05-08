# Pokédex Angular

## Descripción del Proyecto

Aplicación web  **Angular** que muestra una Pokédex con los Pokémon de cada version. Permite filtrar por nombre, tipo y versión del juego. Los datos se obtienen desde la [PokéAPI](https://pokeapi.co/).

---

## Tecnologías Utilizadas

- **Framework:** Angular 14
- **Lenguaje:** TypeScript
- **Estilos:** CSS / SCSS
- **API:** PokéAPI (https://pokeapi.co/)
- **Hosting:** Microsoft Azure Static Web Apps
- **CI/CD:** GitHub Actions (integrado automáticamente con Azure)

---

## Requisitos Previos

- Node.js v18 o superior
- npm v9 o superior
- Angular CLI (`npm install -g @angular/cli`)
- Cuenta en [Microsoft Azure](https://portal.azure.com/)
- Repositorio en GitHub

---

## Instalación Local

```bash
# Clonar el repositorio
git clone https://github.com/tu-usuario/pokedex-angular.git
cd pokedex-angular

# Instalar dependencias
npm install

# Ejecutar en modo desarrollo
ng serve

# Abrir en el navegador
# http://localhost:4200
```

---

## Construcción para Producción

```bash
ng build --configuration production
```

Los archivos generados quedan en la carpeta `dist/`.

---

## Estructura del Proyecto

```
pokedex-angular/
├── src/
│   ├── app/
│   │   ├── components/       # Componentes Angular
│   │   ├── services/         # Servicios (llamadas a la API)
│   │   └── app.module.ts
│   ├── assets/               # Imágenes y recursos estáticos
│   ├── staticwebapp.config.json  # Configuración de seguridad Azure
│   └── index.html
├── angular.json
├── README.md
└── Despliegue.md
```

---

## Seguridad

La aplicación incluye cabeceras de seguridad HTTP configuradas mediante `staticwebapp.config.json`:

| Cabecera | Valor |
|---|---|
| `Strict-Transport-Security` | `max-age=10886400; includeSubDomains; preload` |
| `Referrer-Policy` | `same-origin` |
| `X-Content-Type-Options` | `nosniff` |
| `X-Frame-Options` | `SAMEORIGIN` |
| `Permissions-Policy` | `camera=(), microphone=(), geolocation=()` |
| `Content-Security-Policy` | `default-src 'self'; script-src 'self'; ...` |

Resultado en [securityheaders.com](https://securityheaders.com): **Calificación A+**

---

## URL Pública

🌐 https://black-flower-0247b8610.7.azurestaticapps.net/

---

## Autor

Desarrollado como proyecto académico para el curso de despliegue de aplicaciones en la nube.