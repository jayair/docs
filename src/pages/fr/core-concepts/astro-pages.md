---
layout: ~/layouts/MainLayout.astro
title: Pages
description: Une introduction au pages Astro
i18nReady: true
---

Les **pages** sont des fichiers qui se trouvent dans le sous-répertoire `src/pages/` de votre projet Astro. Ils sont responsables de la gestion du routage, du chargement des données et de la mise en page globale de chaque page de votre site Web.

## Fichiers de page pris en charge 

Astro prend en charge les types de fichiers suivants dans le répertoire `src/pages/` :
- [`.astro`](#pages-astro)
- [`.md`](#pages-markdownmdx)
- `.mdx` (avec l'[Intégration MDX installée](/fr/guides/integrations-guide/mdx/#installation))
- [`.html`](#pages-html)
- [`.js`/`.ts`] (comme [endpoints](/fr/core-concepts/endpoints/))

## Routage basé sur les fichiers

Astro utilise une stratégie de routage appelée **routage par fichier**. Chaque fichier de votre répertoire `src/pages/` devient un point d'accès sur votre site en fonction de son chemin d'accès.

Rédigez des [éléments HTML `<a>`](https://developer.mozilla.org/fr/docs/Web/HTML/Element/a) standard dans votre modèle de composant pour créer des liens entre les pages.

📚 En savoir plus sur [Le routage en astro](/fr/core-concepts/routing/).

## Pages Astro

Les pages Astro utilisent l'extension de fichier `.astro` et prennent en charge les mêmes fonctionnalités que les [composants Astro](/fr/core-concepts/astro-components/).

```astro
---
// Example: src/pages/index.astro
---
<html lang="fr">
  <head>
    <title>Ma page d'accueil</title>
  </head>
  <body>
    <h1>Bienvenue sur mon site web !</h1>
  </body>
</html>
```

Pour éviter de répéter les mêmes éléments HTML sur chaque page, vous pouvez déplacer les éléments communs `<head>` et `<body>` dans vos propres [Composants Layout](/fr/core-concepts/layouts/). Vous pouvez utiliser autant de Composants Layout que vous le souhaitez.

```astro {3} /</?MySiteLayout>/
---
// Exemple : src/pages/index.astro
import MySiteLayout from '../layouts/MySiteLayout.astro';
---
<MySiteLayout>
  <p>Le contenu de ma page, enveloppé dans une mise en page !</p>
</MySiteLayout>
```

📚 En savoir plus sur les [Composants Layout](/fr/core-concepts/layouts/) dans Astro.

## Pages Markdown/MDX

Astro traite également tous les fichiers Markdown (`.md`) contenus dans `src/pages/` comme des pages de votre site Web final. Si vous avez [installé l'intégration MDX](/fr/guides/integrations-guide/mdx/#installation), les fichiers MDX (`.mdx`) sont traités de la même manière. Ces fichiers sont généralement utilisés pour les pages contenant beaucoup de texte, comme les articles de blog et la documentation.

Les Composants Layout sont particulièrement utiles pour les [fichiers Markdown](#pages-markdownmdx). Les fichiers Markdown peuvent utiliser la propriété spéciale `layout` du Frontmatter pour spécifier un [Composant Layout](/fr/core-concepts/layouts/) qui enveloppera leur contenu Markdown dans un document page complet `<html>...</html>`.


```md {3}
---
# Exemple : src/pages/page.md
layout: '../layouts/MySiteLayout.astro'
title: 'Ma page Markdown'
---
# Titre

Voici ma page, écrite en **Markdown**.

```

📚 En savoir plus sur [Markdown](/fr/guides/markdown-content/) dans Astro.

## Pages HTML

Les fichiers portant l'extension `.html` peuvent être placés dans le répertoire `src/pages/` et utilisés directement comme pages sur votre site. Notez que certaines fonctionnalités clés d'Astro ne sont pas prises en charge dans les [Composants HTML](/fr/core-concepts/astro-components/).

## Pages non-HTML

Des pages qui ne sont pas du HTML, comme des `.json` ou des `.xml`, ou même des fichiers, tel que des images, peuvent être générées à partir de chemins API ou appellés couramment "**Routes de Fichiers**".

Les **Routes de Fichiers** sont des fichiers de script qui se termine par l'extension `.js` ou `.ts` et sont présents dans le dossier `src/pages/`.

Les fichiers générés sont basés sur le nom du fichier source, ex: le résultat de la compilation de `src/pages/data.json.ts` correspondra à la route `/data.json` dans votre build final.

En mode SSR (_server-side rendering_) l'extension importe peu et peut être omise, car aucun fichier n'est généré à la compilation. À la place, Astro génère un seul fichier sur le serveur.

```js
// Example: src/pages/builtwith.json.ts
// Génères: /builtwith.json
// Les routes de fichiers doivent exporter une fonction get() qui est appelée et génère le fichier.
// Retournez un objet avec `body` pour sauvegarder le contenu du fichier dans votre build final.
export async function get() {
  return {
    body: JSON.stringify({
      name: 'Astro',
      url: 'https://astro.build/',
    }),
  };
}
```

Les routes d'API reçoivent un objet `APIContext` qui contient les paramètres [`params`](/fr/reference/api-reference/#params) de la requête et une requête [`Request`](https://developer.mozilla.org/fr/docs/Web/API/Request):

```ts
import type { APIContext } from 'astro';
export async function get({ params, request }: APIContext) {
  return {
    body: JSON.stringify({
      path: new URL(request.url).pathname
    })
  };
}
```

Optionnellement, vous pouvez également utiliser le type `APIRoute` pour votre route d'API. Cela vous donnera des messages d'erreur plus précis lorsque votre route d'API retourne un type incorrect.

```ts
import type { APIRoute } from 'astro';
export const get: APIRoute = ({ params, request }) => {
  return {
    body: JSON.stringify({
      path: new URL(request.url).pathname
    })
  };
};
```

## Page d'erreur 404 personnalisée

Pour une page d'erreur 404 personnalisée, vous pouvez créer un fichier `404.astro` ou `404.md` dans `/src/pages`.

Il sera construit en une page `404.html`. La plupart des [services de déploiement](/fr/guides/deploy/) le trouveront et l'utiliseront.