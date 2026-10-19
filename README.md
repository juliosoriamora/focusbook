# FocusBook 1.4

Lector de libros en español con biblioteca privada en el navegador, progreso, lectura compacta, voz y asistente de comprensión mediante un backend de OpenAI.

## Probar en el ordenador

Requiere Node.js 22.9 o superior. No necesita instalar paquetes.

```sh
npm start
```

Abre http://127.0.0.1:4174. Para activar la IA, copia `.env.example` a `.env`, configura `OPENAI_API_KEY` y reinicia. Nunca subas `.env` al repositorio.

## Instrucciones

- [Configuración, uso y migración desde 1.3.4](LEEME.md)
- [Pruebas realizadas y límites](PRUEBAS.md)

```sh
npm test
```

GitHub Actions ejecuta estas pruebas al subir cambios y al abrir una propuesta de cambio. Las pruebas no llaman a OpenAI ni necesitan secretos.

## Uso en el teléfono con el ordenador apagado

Este repositorio guarda el código. **Todavía requiere desplegar la aplicación y su backend en un alojamiento con HTTPS y acceso protegido.** GitHub Pages no ejecuta el servidor de IA de este proyecto.

El servidor actual escucha solo en el ordenador local. Antes de alojarlo hay que adaptar la escucha y la validación del dominio, añadir autenticación y configurar la clave como secreto del alojamiento. No se debe exponer este servidor directamente a internet ni poner la clave en GitHub Pages.

Los libros y el progreso permanecen en cada navegador. La exportación/importación permite trasladarlos; la sincronización entre teléfono y PC todavía no está implementada.
