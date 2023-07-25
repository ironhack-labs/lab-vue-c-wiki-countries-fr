![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# LAB | Vue.js WikiCountries (Composition API)

## Introduction

Après avoir passé trop de temps sur GitHub, vous avez trouvé un ensemble de [données JSON de pays](https://ih-countries-api.herokuapp.com/countries) et avez décidé de l'utiliser pour créer votre propre Wikipédia des pays !

<p align="center">
  <img src="https://education-team-2020.s3.eu-west-1.amazonaws.com/web-dev/labs/lab-wiki-countries-1.gif" alt="Example - Finished LAB" />
</p>

## Configuration

- Forkez ce repo
- Clonez ce repo
- Ouvrez le LAB et commencez :

  ```bash
  $ cd lab-vue-c-wiki-countries
  $ npm install
  $ npm run dev
  ```

## Soumission

- Une fois terminé, exécutez les commandes suivantes :ab: 

  ```bash
  git add .
  git commit -m "done"
  git push origin main
  ```

- Créez une pull request afin que vos TA puissent vérifier votre travail.

## Pour commencer

Nettoyez le composant `App.vue` de manière à ce qu'il ait la structure suivante entre les balises template :

```vue
<!-- src/App.js -->
<template>
  <div class="app"></div>
</template>
```

<br>

## Instructions

### Itération 0 | Installation de vue Router

N'oubliez pas d'installer vue Router :

```shell
$ yarn install vue-router
```

ou

```bash
npm install vue-router
```

Et configurez le router dans votre fichier `src/router/index.js` :

```js
// src/router/index.js
import { createRouter, createWebHistory } from "vue-router";

const routes = [
  {
    path: "/",
    name: "list",
    component: () => import("../components/CountriesList.vue"),
    children: [
      {
        path: "/list/:alpha3Code",
        name: "list",
        component: () => import("../components/CountryDetails.vue"),
      },
    ],
  },
];

const router = createRouter({
  history: createWebHistory("/"),
  routes,
  scrollBehavior() {
    document.getElementById("app").scrollIntoView();
  },
});

export default router;
```

Ensuite, utilisez-le dans le fichier main.js comme suit :accept: 

```js
// src/main.js
import { createApp } from "vue";
import App from "./App.vue";
import router from "./router";

createApp(App).use(router).mount("#app");
```

### Installation de Bootstrap

Nous allons utiliser [Bootstrap](https://getbootstrap.com/) pour le design :+1:

```shell
$ yarn install bootstrap
```

ou

```bash
npm install bootstrap@v5.2.2
```

Pour rendre les styles de Bootstrap disponibles dans toute l'application, importez la feuille de style dans `main.js` avant la ligne `createApp` :

```javascript
// src/main.js

import "bootstrap/dist/css/bootstrap.css";
```

### Itération 1.1 | Création des composants

Dans cette itération, nous nous concentrerons sur la mise en page générale. Avant de commencer, créez le dossier `components` dans le dossier `src`. Vous y créerez au moins 3 composants :

- `Navbar`:  Affiche la barre de navigation de base avec le nom du LAB.
- `CountriesList`: Affiche la liste des liens avec les noms des pays. Chaque lien doit être un `vue-router-dom` `router-link` que nous utiliserons pour  <u>envoyer</u> le code du pays (`alpha3Code`) via l'URL.
- `CountryDetails`: C'est le composant que nous allons afficher via `Route` de `vue-router` et qui va <u>recevoir</u> le code du pays (alpha3Code) via l'URL.

  C'est en fait l'identifiant du pays (par exemple : `/ESP` pour l'Espagne, `/FRA` pour la France).

Pour vous aider avec la structure des composants, nous vous avons donné un exemple de page dans `example.html`.

Si vous souhaitez le styliser, rafraîchissez votre mémoire sur Bootstrap dans la [documentation](https://getbootstrap.com/docs/5.2/getting-started/introduction/), ou consultez notre approche de stylisation dans `example.html`.

### Itération 1.2 | Composant Navbar

La manière la plus simple de définir un composant en vue est d'écrire une fonction JavaScript, également appelée fonction de composant. La navbar devrait afficher le titre _LAB - WikiCountries_.

### Itération 1.3 | Composant CountriesList

Ce composant doit afficher une liste de `router-link`utilisés pour déclencher le changement d'URL du navigateur. Cliquez sur un composant `router-link` activera la `Route` correspondante affichant le détail du pays.

### Itération 1.4 | Composant CountryDetails et configuration de `router-view`

Maintenant que notre liste de pays est prête, nous devons créer la page `CountryDetails`. `CountryDetails` affiche les détails du pays en fonction du lien sur lequel nous avons cliqué. Ce composant doit être affiché/rendered de manière dynamique avec `<router-vue />`.

```vue
<!-- Example -->

<router-view>
```

Les composants rendus avec Vue.js peuvent lire la requête avec `this.$route`. Nous pouvons l'utiliser pour obtenir les informations provenant de la barre d'URL du navigateur, par exemple le code `alpha3Code` du pays.

**NOTE:** Pour l'image miniature du drapeau, vous pouvez utiliser le code `alpha2Code` en minuscules et l'intégrer dans l'URL comme indiqué ci-dessous :

- France : https://flagpedia.net/data/flags/icon/72x54/fr.png
- Allemagne : https://flagpedia.net/data/flags/icon/72x54/de.png
- Brésil : https://flagpedia.net/data/flags/icon/72x54/br.png
- etc.

---

### Itération 2 | Lier le tout ensemble

Une fois que vous avez créé les composants, la structure des éléments que votre `App.vue` va rendre devrait ressembler à ceci :

```vue
<template>
  <div class="app">
    <NavBar />
    <router-view />
  </div>
</template>

<script setup>
import NavBar from "./components/NavBar.vue";
</script>

<style></style>
```

---

### Itération 3 | Définir l'état lorsque le composant est monté

Notre application `App.vue` doit extraire les `countries` dans la méthode `data` de Vue, en stockant les données provenant du fichier `src/countries.json`.

---

### Itération 4 | Bonus | Récupérer les données des pays à partir d'une API

Au lieu de vous fier aux données statiques provenant d'un fichier  `json`, faisons quelque chose de plus intéressant et récupérons les données à partir d'une véritable API.

Effectuons une requête `GET` à l'URL [https://ih-countries-api.herokuapp.com/countries](https://ih-countries-api.herokuapp.com/countries) et utilisons les données renvoyées par la réponse comme liste des pays. Vous pouvez utiliser `fetch` ou `axios`  pour effectuer la requête.

Si vous utilisez l'API Options, vous devez utiliser le crochet `mounted()` pour définir le crochet du cycle de vie qui s'exécute une seule fois et effectue une requête à l'API.

Vous devriez utiliser `<script setup>` pour définir la méthode du cycle de vie de la composition API qui vous permet de travailler au début du cycle de vie.

<br>

<p align="center">
  <img src="https://vuejs.org/assets/lifecycle.16e4c08e.png" alt="Where is the setup lifecycle" />
</p>

<br>

La requête doit être effectuée dès que l'application est chargée, donc réfléchissez à quand et d'où nous devons effectuer la requête à l'API.

---

### Itération 5 | Bonus | Récupérer les données d'un pays à partir d'une API
Configurez le composant `CountriesDetails`. Il devrait effectuer une requête vers l'API RestCountries et récupérer les données du pays spécifique. Vous pouvez construire l'URL de requête en utilisant le `alpha3Code` du pays. Exemple :

- États-Unis : [https://ih-countries-api.herokuapp.com/countries/USA](https://ih-countries-api.herokuapp.com/countries/USA)
- Japon : [https://ih-countries-api.herokuapp.com/countries/JPN](https://ih-countries-api.herokuapp.com/countries/JPN)
- Inde : [https://ih-countries-api.herokuapp.com/countries/IND](https://ih-countries-api.herokuapp.com/countries/IND)

L'effet devrait s'exécuter après le rendu initial et à chaque fois que le paramètre d'URL avec le `alpha3Code` change.

<br>

Bon codage ! :heart: