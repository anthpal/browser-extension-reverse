---
name: "browser-extension-reverse"
version: 1.0.0
category: build
platforms: [CLAUDE_CODE]
tags: [reverse-skill]
description: "Úsala para ingeniería inversa de extensiones de navegador (Chrome/Firefox), incluyendo análisis del manifest, workers en segundo plano y recuperación de lógica de credenciales o de tráfico implementada en extensiones."
---
# Ingeniería inversa de extensiones de navegador

## ACCIÓN REQUERIDA (ejecutar inmediatamente tras leer)

1. `AHORA`: leer `../field-journal/precedent-reverse.md`
2. `AHORA`: confirmar que el objetivo es una **extensión de navegador** (crx/xpi/directorio descomprimido), no JS de una página web normal (si es normal → `js-reverse/`)
3. `LUEGO`: descomprimir la extensión; leer el manifest
4. `ACTUAR`: superficie de permisos → scripts en segundo plano → hooks de red/almacenamiento

## Casos de uso

- Análisis de extensiones MV2/MV3 de Chrome/Edge
- Extensiones de Firefox
- IOCs de extensiones maliciosas, investigación de envenenamiento de extensiones en la cadena de suministro
- Reconstrucción de lógica de firma/cifrado/proxy implementada en extensiones

## Flujo de trabajo

### 1. Paquete

```text
□ Descomprimir el crx / obtener el directorio de la extensión desde el perfil
□ manifest.json: permissions, host_permissions, background, content_scripts
□ Evaluar permisos excesivos (<all_urls>, webRequest, debugger)
```

### 2. Lógica

```text
□ Punto de entrada del service_worker / background
□ Puntos de inyección de content_script y su "mundo" (isolated)
□ Claves en chrome.storage / IndexedDB
□ Igual que en `js-reverse`: observar la red y el paso de mensajes (runtime.sendMessage)
```

### 3. Dinámico

```text
□ Cargar el directorio descomprimido en modo desarrollador
□ Revisar errores en chrome://extensions
□ Adjuntar DevTools al service worker
□ Si hace falta, Frida/CDP del navegador (jshookmcp)
```

## Herramientas

| Herramienta | Uso |
|-------------|-----|
| Descompresores / jq | manifest |
| Chrome DevTools | depurar el worker |
| Cadena de herramientas de js-reverse | JS en profundidad |
| YARA | reglas para extensiones maliciosas |

## Referencias

- `references/extension-analysis.md`
- Entradas del field-journal relacionadas con recuperación de extensiones
- `../js-reverse/` `../malware-analysis/`

## Contexto de enrutamiento

**Upstream (origen)**: MASTER R30
**Downstream (destino)**: JS muy ofuscado → `js-reverse`; investigación de envenenamiento → supply-chain / malware

## Autoverificación al terminar la tarea

- [ ] ¿Se listó la superficie de permisos y los scripts de entrada?
- [ ] ¿Se reconstruyó el flujo de datos clave?
- [ ] ¿Checklist completado?
