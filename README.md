## 📌 Descripción de la problemática
Muchas personas no tienen una forma sencilla y rápida de organizar las películas que quieren ir a ver al cine o recordar cuáles han visto y recomendarían. Usualmente se pierde el seguimiento entre recomendaciones, estrenos y gustos personales.
## 💡 Solución propuesta
Esta aplicación web permite al usuario ver una lista de películas populares y actuales usando una API pública real (The Movie Database - TMDb), y seleccionar sus favoritas para guardarlas en una lista personal, junto con notas personalizadas. Todo se almacena localmente en el navegador con localStorage, sin necesidad de conexión a una base de datos externa.
## 🧑💻 Tecnologías utilizadas
- Vue 3
- Vite
- JavaScript
- TMDb API (API Key v3)
- localStorage (CRUD local)
- HTML / CSS
## ⚙️ Funcionalidades
✅ SPA construida con Vue 3
✅ Uso de al menos 2 componentes personalizados
✅ Carga de datos desde una API pública (TMDb)
✅ Funcionalidad CRUD completa:
- Crear: Agregar películas a favoritos
- Leer: Ver lista de favoritos
- Actualizar: Agregar/editar nota personal
- Eliminar: Quitar de favoritos
✅ 100% funcional en entorno local
✅ Datos almacenados en localStorage
✅ Proyecto alojado en GitHub
🚀 Instrucciones para ejecutar el proyecto
1. Clonar el repositorio:

```bash
git clone https://github.com/TU_USUARIO/pelis-app.git
cd pelis-app
```

2. Instalar dependencias:

```bash
npm install
```

3. Crear archivo `.env` en la raíz del proyecto con la siguiente línea:

```env
VITE_TMDB_KEY=TU_API_KEY_AQUI
```

4. Ejecutar el proyecto:

```bash
npm run dev
```

5. Abrir en el navegador:

```text
http://localhost:5173
```
🔑 Configuración de API Key
Para que la aplicación funcione correctamente, es necesario que exista un archivo `.env` en la raíz con esta variable:

```env
VITE_TMDB_KEY=TU_API_KEY_AQUI
```

Si no tienes una API Key propia, puedes solicitarla al autor del proyecto por privado.
🧩 Fragmento clave en `App.vue`
```js
<script setup>
import { ref, onMounted } from 'vue'

const API_KEY = import.meta.env.VITE_TMDB_KEY

const URL_POPULAR = `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}&language=es-ES&page=1`

// Resto del código para fetch y CRUD aquí
</script>
```
📚 Licencia
Este proyecto fue desarrollado con fines educativos.
