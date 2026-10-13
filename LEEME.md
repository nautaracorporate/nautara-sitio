# Sitio web de Nautara Corporativa

Sitio estático. No tiene base de datos, ni plugins, ni nada que actualizar por seguridad.
Son archivos HTML y una hoja de estilos. Se edita con cualquier editor de texto.

---

## Qué hay en cada archivo

```
index.html                      Inicio
enfoque/index.html              Enfoque
servicios/index.html            Servicios
bitacora/index.html             Bitácora (índice de artículos)
bitacora/_plantilla.html        Plantilla para escribir un artículo nuevo
contacto/index.html             Contacto
gracias/index.html              Confirmación después de enviar el formulario
aviso-de-privacidad/index.html  Aviso de Privacidad
404.html                        Página de error
assets/estilos.css              Todos los estilos del sitio
robots.txt                      Instrucciones para buscadores
sitemap.xml                     Mapa del sitio para buscadores
```

---

## Pendientes antes de publicar

Están marcados en los archivos con la palabra **PENDIENTE**.

1. **Clave del formulario.** En `contacto/index.html`, línea del `access_key`.
   Ver la sección "Formulario de contacto" más abajo.
2. **Domicilio fiscal** en `aviso-de-privacidad/index.html`.
3. **Fecha de última actualización** del aviso de privacidad.
4. **Teléfono de WhatsApp**, si se decide poner uno, en `contacto/index.html`.
5. **Dos artículos** en la Bitácora. Mientras no existan, quitar Bitácora del menú:
   se edita en todos los archivos HTML, en el bloque `<nav>`.
6. **Favicon e isotipo**, cuando el símbolo de la marca esté cerrado.

El Aviso de Privacidad está redactado conforme a la ley mexicana con la información
que capta el formulario. **No es asesoría legal.** Conviene que lo revise un abogado
antes de publicarlo, sobre todo si después se captan datos financieros de clientes.

---

## Cómo publicar un artículo

1. Copiar `bitacora/_plantilla.html` a una carpeta nueva dentro de `bitacora/`,
   con el nombre del artículo en minúsculas y con guiones. El archivo se llama
   `index.html` dentro de esa carpeta.

   Ejemplo: `bitacora/los-covenants-que-firmo-sin-leer/index.html`
   queda publicado en `nautara.com.mx/bitacora/los-covenants-que-firmo-sin-leer/`

2. Cambiar dentro del archivo: el `<title>`, la `<meta name="description">`,
   el `<link rel="canonical">`, la fecha, la categoría, el título y el texto.

3. Agregar el enlace al artículo en `bitacora/index.html`, arriba de los demás,
   con este bloque:

```html
<a class="articulo" href="/bitacora/nombre-del-articulo/">
  <p class="meta">13 octubre 2026 · Flujo de efectivo</p>
  <h3>Título del artículo</h3>
  <p>Una o dos líneas que resuman de qué trata.</p>
</a>
```

4. Agregar la dirección nueva a `sitemap.xml`.

**Las direcciones no llevan fecha a propósito.** Así un artículo se puede corregir
o actualizar años después sin que la dirección quede desfasada y sin perder lo
ganado en buscadores. Una dirección publicada no se cambia nunca.

---

## Formulario de contacto

El formulario usa Web3Forms, un servicio gratuito que recibe el envío y lo manda
por correo. No requiere servidor propio.

1. Entrar a web3forms.com
2. Poner **contacto@nautara.com.mx** y pedir la clave de acceso.
3. Llega un correo con una clave larga.
4. Pegarla en `contacto/index.html`, reemplazando `PENDIENTE-CLAVE-WEB3FORMS`.
5. Para que después de enviar lleve a la página de gracias, agregar esta línea
   junto a los demás campos ocultos del formulario:

```html
<input type="hidden" name="redirect" value="https://nautara.com.mx/gracias/">
```

El campo oculto `botcheck` ya está puesto: atrapa robots de spam sin molestar
a las personas. No hay que tocarlo.

**Probar el formulario antes de anunciar el sitio.** Enviar un mensaje de prueba
y confirmar que llega a la bandeja, y que no cae en spam.

---

## Publicar el sitio en internet

### Primera vez

1. En GitHub, crear un repositorio nuevo llamado `nautara-sitio`. Dejarlo privado.
2. Subir todos estos archivos. En GitHub se puede arrastrar la carpeta
   directamente desde la computadora con el botón "uploading an existing file".
3. En Cloudflare, ir a Workers & Pages y crear un proyecto nuevo conectado a GitHub.
4. Elegir el repositorio `nautara-sitio`.
5. En la configuración de compilación, **dejar todo vacío**: no hay comando de
   compilación y el directorio de salida es la raíz. Este sitio no se compila.
6. Cloudflare publica el sitio en una dirección de prueba que termina en
   `.pages.dev`. **Revisar todo ahí antes de conectar el dominio real.**

### Cada cambio posterior

Subir el archivo cambiado a GitHub. Cloudflare lo publica solo, en menos de un minuto.
Si algo sale mal, GitHub guarda todas las versiones anteriores y se puede regresar.

---

## Conectar el dominio sin tumbar el correo

**Esta es la parte peligrosa.** El correo de Google Workspace depende de registros
DNS que viven en GoDaddy. Si se tocan por error, el correo del despacho se cae y los
mensajes que manden rebotan sin aviso.

### Antes de tocar nada

1. Entrar a GoDaddy, al panel de DNS de nautara.com.mx.
2. **Tomar captura de pantalla de todos los registros.** Todos, sin excepción.
3. Anotar aparte, en un documento, los que empiezan con:
   - **MX** — son los del correo. Apuntan a Google.
   - **TXT** que contenga `v=spf1` — autoriza a Google a mandar correo como usted.
   - **TXT** o **CNAME** de nombre `google._domainkey` o parecido — es la firma DKIM.
   - **TXT** de nombre `_dmarc` — la política contra suplantación.

Esa lista es el seguro. Si algo falla, con ella se restaura todo.

### Opción recomendada: dejar el DNS en GoDaddy

Es la más segura porque no se mueve nada del correo.

1. En Cloudflare Pages, en el proyecto, agregar el dominio personalizado
   `nautara.com.mx` y también `www.nautara.com.mx`.
2. Cloudflare indica qué registros crear. Normalmente son un **CNAME** para `www`
   y un registro para la raíz del dominio.
3. Crear únicamente esos en GoDaddy. **No borrar ni modificar ningún MX ni TXT.**
4. Esperar. El cambio puede tardar de unos minutos a 48 horas en verse en todos lados.

### Después de conectar

1. Mandarse un correo a contacto@nautara.com.mx desde una cuenta externa
   y confirmar que llega.
2. Mandar un correo desde contacto@nautara.com.mx a una cuenta externa
   y confirmar que no cae en spam.
3. Abrir https://nautara.com.mx y verificar que carga con candado de seguridad.
4. Verificar que https://www.nautara.com.mx también funcione.

Si el correo falla en cualquier momento, restaurar los registros de la captura.

---

## Después de publicar

- **Google Search Console**: dar de alta el dominio y mandar el sitemap.xml.
  Es lo que hace que Google encuentre el sitio y avise si algo se rompe.
- **Google Business Profile**: dar de alta el despacho. Es lo que mete a Nautara
  en el mapa cuando alguien busca asesoría financiera en Monterrey.
- **Analítica**: Cloudflare incluye una medición de visitas gratuita que no usa
  cookies. Al no usar cookies, el sitio no necesita banner de consentimiento.
  Se enciende desde el panel de Cloudflare, no hay que tocar los archivos.
