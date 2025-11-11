# Registro de Venta · Frontend Web

Interfaz estática construida con HTML, Tailwind CSS y JavaScript para registrar ventas asociadas a Zelle. Permite que cada vendedor se autentique con un código breve, capture los datos del titular, adjunte un comprobante y envíe todo a una automatización externa mediante un webhook.

## Tabla de contenido
- [Registro de Venta · Frontend Web](#registro-de-venta--frontend-web)
  - [Tabla de contenido](#tabla-de-contenido)
  - [Descripción general](#descripción-general)
  - [Características principales](#características-principales)
  - [Requisitos previos](#requisitos-previos)
- [Elige el servidor estático que prefieras](#elige-el-servidor-estático-que-prefieras)
- [o](#o)
- [o](#o-1)
  - [Uso cotidiano](#uso-cotidiano)
  - [Estructura del proyecto](#estructura-del-proyecto)
  - [Solución de problemas](#solución-de-problemas)
  - [Buenas prácticas y personalización](#buenas-prácticas-y-personalización)
  - [Contribuciones](#contribuciones)

## Descripción general
El formulario guía al vendedor paso a paso:
- Solicita el **código de vendedor**, lo valida contra un listado local y lo almacena en `localStorage` para sesiones futuras.
- Recolecta **nombre del titular**, **importe** (con coma decimal) y **comprobante** (imagen ≤ 1 MB).
- Construye un `FormData` con la información y lo envía mediante `fetch` a un webhook externo.
- Muestra mensajes claros de éxito o error sin recargar la página.

## Características principales
- Diseño “glassmorphism” optimizado para escritorio y móvil.
- Validaciones en tiempo real: letras para el nombre, formato numérico para montos y tamaño máximo del archivo.
- Persistencia del código de vendedor por navegador.
- Envío multipart/form-data compatible con plataformas de automatización (Make, Zapier u otras).
- Mensajes de retroalimentación visibles y accesibles.

## Requisitos previos
- Navegador moderno con soporte para `fetch`, `FormData` y ES2015+.
- Opcional: `Node.js` o `Python` para servir el sitio localmente.
- Acceso al endpoint que recibirá la información (webhook HTTPS público).


# Elige el servidor estático que prefieras
npx serve .
# o
python -m http.server 3000
# o
npx http-server .
```
Luego abre `http://localhost:3000/index.html` (ajusta el puerto según tu herramienta). Cada vez que edites el archivo, refresca el navegador para ver los cambios.

## Configuración esencial
Edita `index.html` antes de desplegar o compartir el proyecto:
- **Listado de vendedores**  
  Dentro del script busca el objeto `const vendedores = { ... }` y reemplaza los códigos y nombres por los de tu organización. Usa identificadores genéricos (ej. `AA01`, `BB02`) para evitar exponer información sensible.
- **Webhook**  
  Actualiza la constante `WEBHOOK_URL` con la URL pública de tu automatización:
  ```javascript
  const WEBHOOK_URL = "https://tu-dominio.com/webhook";
  ```
  El README no incluye la URL real para proteger datos confidenciales.
- **Validaciones opcionales**  
  Ajusta expresiones regulares, límites de tamaño o campos adicionales según tu proceso.

## Uso cotidiano
1. El vendedor ingresa por primera vez, teclea su código y queda registrado en el navegador.
2. Completa el nombre del titular, el importe y adjunta la imagen del comprobante.
3. Al pulsar **Enviar Registro** se realiza la petición `POST` al webhook.  
   - Estado 200-299 ⇒ el formulario se resetea y se muestra un mensaje de éxito.  
   - Otro estado ⇒ aparece un mensaje de error y permanece la información para reintentar.

## Estructura del proyecto
```
zelleControl/
├── index.html   # Página principal con estilos y lógica embebida
└── README.md    # Guía de instalación y uso
```

## Solución de problemas
- **El formulario no aparece tras ingresar el código**  
  Verifica que el código esté listado en `vendedores`. Los valores son sensibles a mayúsculas.
- **El botón no envía nada**  
  Revisa la consola del navegador (`F12`) para detectar errores JavaScript o respuestas del servidor.
- **Mensaje “Debe adjuntar una imagen menor a 1 MB”**  
  Reduce el tamaño del comprobante o ajusta el límite en el código (`file.size > 1024 * 1024`).
- **El webhook recibe campos vacíos**  
  Comprueba que tu endpoint soporte `multipart/form-data` y que lea los campos `vendedor`, `nombre`, `monto`, `fecha` y `comprobante`.

## Buenas prácticas y personalización
- Mantén el repositorio privado si vas a incluir URLs o credenciales reales.
- Para controlar el acceso, puedes mover el listado de vendedores a un backend y exponer un endpoint de validación.
- Si deseas auditar envíos, registra la respuesta del webhook o añade un servicio intermedio que almacene el histórico.
- Considera automatizar pruebas manuales enviando registros de ejemplo a un entorno de pruebas.

## Contribuciones
1. Haz un fork del repositorio.
2. Crea una rama descriptiva: `git checkout -b feature/nueva-funcionalidad`.
3. Realiza commits claros, con mensajes que expliquen la intención.
4. Abre un Pull Request detallando cambios, pruebas y consideraciones de seguridad.

Para sugerencias o dudas abre un issue. ¡Gracias por usar Registro de Venta! 