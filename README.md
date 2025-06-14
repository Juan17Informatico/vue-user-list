# 📋 SPA de Usuarios - Vue 3 + Vuetify

Aplicación web desarrollada como parte de una prueba técnica. Es una SPA (Single Page Application) en Vue 3 que consume una API pública para mostrar un listado de usuarios, permitiendo ver más detalles en un modal. Incluye búsqueda en tiempo real, diseño responsive y una experiencia visual cuidada gracias a Vuetify y estilos personalizados con **Sass**.

![Preview de la aplicación](/public/preview.png)

🔗 **Demo en vivo:**  
[https://juan17informatico.github.io/vue-user-list/](https://juan17informatico.github.io/vue-user-list/)

---

## 🛠️ Instalación

```bash
git clone https://github.com/Juan17Informatico/vue-user-list.git
cd vue-user-list
npm install
npm run dev
```

---

## 🧰 Tecnologías utilizadas

* ✅ **Vue 3** (Composition API)
* ✅ **Vuetify 3**
* ✅ **Axios**
* ✅ **Sass** para estilos personalizados
* ✅ **GitHub Pages** para despliegue
* ✅ **Vite** como bundler

---

## ⚙️ Funcionalidades implementadas

### 🧑‍💼 Listado de usuarios

* Muestra avatar, nombre completo, correo y botón **“Ver detalles”**
* Consumo de datos desde [https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)

### 🔍 Filtro en tiempo real

* Campo de búsqueda que filtra los usuarios por nombre a medida que se escribe.

### 🚧 Modal de detalles

* Al hacer clic en “Ver más” se despliega un modal con:

  * Dirección
  * Teléfono
  * Compañía
  * Sitio web

### 💻 Responsive Design

* Adaptado completamente a dispositivos móviles y de escritorio.
* Uso del sistema de grid de Vuetify (`v-container`, `v-row`, `v-col`).

---

## ✨ Mejoras visuales y extras implementados

* 🎯 **Indicador de carga** (`v-progress-circular`)
* 🧾 **Manejo de errores** con mensajes amigables
* 🎬 **Animación suave** en la aparición del modal
* 🧩 **Separación de lógica de API** (`services/UserService.js`)
* 🚀 **Deploy funcional** en GitHub Pages
* 🎨 **Estilos personalizados con Sass**: Se utilizó Sass para extender y personalizar los estilos de Vuetify, permitiendo una apariencia única y coherente en toda la aplicación. Los archivos `.scss` se encuentran en la carpeta `src/styles/` y se importan en los componentes según sea necesario.

---

## 🧠 Decisiones técnicas

* Se utilizó **Composition API** por su flexibilidad y mejor organización del código.
* El diseño visual se desarrolló completamente con **Vuetify 3**, aprovechando sus componentes como `v-card`, `v-dialog`, `v-text-field` y `v-btn`.
* Se modularizó la aplicación en componentes reutilizables y claros:

  * `App.vue`: Layout principal
  * `UserList.vue`: Muestra la lista de usuarios
  * `UserCard.vue`: Componente de cada tarjeta de usuario
  * `UserModal.vue`: Modal de detalles
  * `SearchBar.vue`: Búsqueda en tiempo real

* Para los estilos, se empleó **Sass** permitiendo el uso de variables, mixins y anidamiento para mantener el código CSS organizado y escalable.

---

## 📁 Estructura de carpetas destacada

```
src/
├── components/
│   ├── SearchBar.vue
│   ├── UserCard.vue
│   ├── UserList.vue
│   └── UserModal.vue
├── services/
│   └── UserService.js
├── styles/
│   └── components/
│       └── App.scss
├── App.vue
├── main.js
```
