---
title: Despliega tu proyecto de Astro con Buddy
description: Cómo desplegar tu proyecto Astro usando Buddy.
layout: ~/layouts/DeployGuideLayout.astro
i18nReady: true
---

Puedes desplegar tu proyecto Astro usando [Buddy](https://buddy.works/), una solución de CI/CD que puede construir tu proyecto y enviarlo a muchos destinos de despliegue diferentes, incluidos servidores FTP y proveedores de alojamiento en la nube.

:::note
Buddy no alojará tu proyecto. Solo te ayudará a administrar el proceso de compilación y entregará el resultado a la plataforma de despliegue de tu elección.
:::

## Cómo desplegar

1. Crea una cuenta en [Buddy](https://buddy.works/sign-up).
2. Crea un nuevo proyecto y conéctalo con un repositorio Git (GitHub, GitLab, BitBucket, cualquier repositorio Git privado o puedes usar Buddy Git Hosting).
3. Agrega una nueva pipeline.
4. En la pipeline recién creada, agrega una action para **[Node.js](https://buddy.works/actions/node-js)**.
5. En esta action agrega:

   ```bash
   npm install
   npm run build
   ```

6. Agrega una action de despliegue: hay muchas para elegir, puedes explorarlas [aquí](https://buddy.works/actions). Aunque tus configuraciones pueden diferir, recuerda configurar la **ruta de origen** en `dist`.
7. Presiona el botón **Run**.

