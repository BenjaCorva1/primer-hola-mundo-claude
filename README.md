# Harborline

Prototipo de UI de helpdesk/ticketing (estilo Zendesk), construido como un único archivo HTML estático. Todo el markup, CSS y JS vive en `index.html`. No hay build system, package manager, framework ni tests.

## Ver la demo

🔗 https://benjacorva1.github.io/primer-hola-mundo-claude/

## Correrlo localmente

Abrí `index.html` directamente en el navegador, o servilo:

```
python3 -m http.server
```

## Notas

- No hay backend ni persistencia: los tickets viven en un array en memoria dentro del JS y el estado se resetea al recargar la página.
- La interfaz está en español.
