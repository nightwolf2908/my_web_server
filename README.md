# Yollotl — Contabilidad y tecnología

Sitio web estático para presentar servicios de contabilidad e impuestos, desarrollo de software, DevOps y ciberseguridad.

🌐 Sitio: https://abdiel2908.com (https://abdiel2908.com)

## Tecnologías e infraestructura

- HTML y CSS: contenido, diseño y adaptación a dispositivos móviles.
- Netlify: alojamiento, despliegues y certificado HTTPS.
- Cloudflare: administración del DNS del dominio.
- GitHub: repositorio del código fuente.

## Cómo funciona

Visitante abre abdiel2908.com
             ↓
Cloudflare resuelve el DNS
             ↓
El navegador se conecta a Netlify
             ↓
Netlify entrega el sitio mediante HTTPS

Cloudflare está configurado como DNS only: administra la resolución del dominio y el tráfico web se conecta directamente a Netlify.

## Publicar un sitio en Netlify

1. Crear el sitio con un archivo index.html y sus recursos.
2. Subir el proyecto a un repositorio de GitHub.
3. En Netlify, importar el repositorio.
4. Seleccionar la rama que se utilizará para producción.
5. Configurar el directorio de publicación: la carpeta que contiene el index.html que se servirá.
6. Para un sitio HTML y CSS sin compilación, dejar vacío el comando de build.
7. Ejecutar el despliegue.

Netlify asigna una dirección inicial al proyecto:

jade-moxie-b13b10.netlify.app

También se puede publicar manualmente mediante Netlify Drop (https://app.netlify.com/drop), subiendo la carpeta que contiene el sitio.

## Conectar el dominio de Cloudflare

En Netlify, entrar en Domain management y añadir:

abdiel2908.com
www.abdiel2908.com

En Cloudflare, abrir DNS → Records y configurar:

| Tipo | Nombre | Destino | Proxy | TTL |
|---|---|---|---|---|
| CNAME | @ | apex-loadbalancer.netlify.com | DNS only | Auto |
| CNAME | www | jade-moxie-b13b10.netlify.app | DNS only | Auto |

Los destinos deben escribirse sin https:// y sin / al final.

### Qué significa cada elemento

- DNS: sistema que permite localizar los servidores asociados a un dominio.
- CNAME: registro que establece un alias hacia otro nombre de dominio.
- @: representa el dominio raíz, en este caso abdiel2908.com.
- www: representa el subdominio www.abdiel2908.com.
- DNS only: Cloudflare responde las consultas DNS sin actuar como proxy del tráfico web.
- TTL: tiempo durante el que una respuesta DNS puede permanecer en caché.

Cloudflare admite CNAME flattening en el dominio raíz, lo que permite utilizar el destino indicado por Netlify para @.

## Activar HTTPS

Después de guardar los registros DNS:

1. Abrir Domain management → HTTPS en Netlify.
2. Pulsar Verify DNS configuration.
3. Si aparece la opción, pulsar Provision certificate.
4. Esperar a que se muestre Your project has HTTPS enabled.

El certificado de Let's Encrypt debe incluir ambos nombres:

abdiel2908.com
www.abdiel2908.com

Netlify administra su renovación automática. Los cambios DNS pueden necesitar tiempo para propagarse.

## Dominio principal

El dominio principal del proyecto es:

https://abdiel2908.com

Netlify está configurado para redirigir las visitas de www.abdiel2908.com al dominio principal.

La redirección HTTP la realiza Netlify; el registro CNAME permite localizar el servidor.

## Acceso público

El sitio debe estar publicado con acceso público para que los visitantes puedan abrirlo sin una cuenta de Netlify.

Para comprobarlo, abrir la dirección en una ventana de incógnito. Si aparece This site is private, revisar la protección de acceso del proyecto y utilizar Make public cuando corresponda.

## Actualizar el sitio

Si Netlify está conectado al repositorio:

git add .
git commit -m "Actualizar contenido del sitio"
git push

Un cambio en la rama configurada para producción inicia un nuevo despliegue, siempre que los despliegues automáticos estén habilitados.

Si se utiliza publicación manual, volver a subir la carpeta actualizada a Netlify.

## Contacto

- Contabilidad e impuestos: operaciones@ulloaservicews.com
- Software, DevOps y ciberseguridad: abdielitopro4800s@gmail.com

## Problemas frecuentes

| Problema | Qué revisar |
|---|---|
| Pending DNS verification | Nombres y destinos de los registros CNAME |
| Waiting on DNS propagation | Verificación de ambos dominios y propagación de los cambios |
| www no funciona | Que el destino termine en .netlify.app, sin / |
| El navegador muestra “No es seguro” | Abrir explícitamente con https:// y comprobar el certificado |
| Funciona con sesión, pero no en incógnito | Protección de acceso o estado privado del proyecto |
| No aparecen los cambios | Estado del último despliegue, rama configurada y caché del navegador |
