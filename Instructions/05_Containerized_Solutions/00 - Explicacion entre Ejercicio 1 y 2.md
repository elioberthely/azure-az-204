# 🚀 Primer ejercicio: Deploy a container to Azure Container Instances (ACI)

**Objetivo:** <span style="color:blue">Ejecutar un contenedor en Azure sin preocuparte demasiado por almacenar la imagen permanentemente.</span>

## 🔹 Flujo:

- **Crear un recurso de ACI:** <span style="color:green">Azure Container Instance actúa como un “servidor temporal” que ejecuta tu contenedor.</span>
- **Desplegar un contenedor:** <span style="color:green">Usas una imagen existente (local o de un repositorio público) y la levantas en Azure.</span>
- **Verificar que esté corriendo:** <span style="color:green">Compruebas que tu contenedor se ejecuta correctamente en la nube.</span>

## 💡 Claves:

- <span style="color:orange">No necesitas construir la imagen en Azure; puedes usar imágenes de Docker Hub o ya construidas.</span>
- <span style="color:orange">Es ideal para pruebas rápidas o aplicaciones ligeras.</span>
- <span style="color:orange">No implica almacenamiento persistente de la imagen más allá de la ejecución.</span>

---

# 🚀 Segundo ejercicio: Build and push to Azure Container Registry (ACR)

**Objetivo:** <span style="color:blue">Preparar tu aplicación para que sea containerizada, almacenar la imagen en Azure y luego poder usarla en cualquier lugar.</span>

## 🔹 Flujo:

- **Crear un ACR (Azure Container Registry):** <span style="color:green">Esto es como un “repositorio privado de Docker” en Azure.</span>
- **Construir una imagen desde tu Dockerfile:** <span style="color:green">Aquí sí transformas tu código en un contenedor.</span>
- **Subir la imagen a ACR:** <span style="color:green">Guardas tu imagen en la nube de forma permanente para usarla luego.</span>
- **Verificar y ejecutar la imagen:** <span style="color:green">Opcionalmente puedes levantar el contenedor desde ACR en ACI u otro servicio.</span>

## 💡 Claves:

- <span style="color:orange">Es más sobre gestión de imágenes que sobre ejecución inmediata.</span>
- <span style="color:orange">Permite reutilizar la misma imagen en múltiples entornos (ACI, AKS, App Service, etc.).</span>
- <span style="color:orange">Necesitas un Dockerfile y conocimiento de cómo containerizar tu app.</span>

---

# 📊 Resumen visual de la diferencia

| Aspecto                 | <span style="color:blue">ACI Deployment</span>            | <span style="color:purple">ACR Build & Push</span>            |
|-------------------------|--------------------------------|--------------------------------|
| Propósito               | Ejecutar contenedores directamente | Construir y almacenar imágenes de contenedor |
| Necesidad de Dockerfile | No necesariamente             | Sí, para construir la imagen |
| Imagen                  | Puede ser pública o existente | La construyes tú y la subes |
| Persistencia            | Temporal (mientras corra el contenedor) | Permanente en el registro |
| Uso típico               | Pruebas rápidas, demos        | Producción, múltiples despliegues |

---

# 🔑 En pocas palabras:

- **ACI:** <span style="color:blue">“Quiero ejecutar un contenedor ahora mismo en Azure.”</span>  
- **ACR:** <span style="color:purple">“Quiero crear, guardar y versionar mi contenedor para usarlo en cualquier parte.”</span>
