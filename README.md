Phat inflated pre-build fork with all locales of [firebaseui-web](https://github.com/firebase/firebaseui-web)

# Phat fork of FirebaseUI for Web

## Why?

We needed to localize FirebaseUI and we couldn't use the CDN as that assumes
everything is on document 'window' space, and it uses the old import (non-tree shaking) where you could chain calls like firebase.initializeApp().auth().

If we tried to mix in the npm modules with the CDN code, we had to do some crazy patching that is a really bad solution, and we had to use the npm modules for firebase.

Because we had to reuse our firebase instance from the module with the localized
widgets, we found this phat fork the best solution for now.

It's about 20-30 MB, so you should just make shure you're using dynamic imports
and only loading the locale you care about (check browser debug network tab and make sure there is no large download).

## Use it
1. install from github by adding this line on package.json: `"tg-firebaseui": "github:tomegenius/firebaseui-web"`
2. `$ npm install` (or `$ pnpm install`).
3. import specific locale:

```
     const firebaseui = await import("tg-firebaseui/fr");
           let app;

      if (!getApps().length) {
        app = initializeApp({
          apiKey: "",
          authDomain: "",
          projectId: "",
          storageBucket: "",
          messagingSenderId: "",
          appId: "",
          measurementId: "",
        });
      } else {
        app = getApps()[0];
      }

      const {
        getAuth,
        onAuthStateChanged,
        EmailAuthProvider,
        GoogleAuthProvider,
        OAuthProvider,
        // FacebookAuthProvider,
      } = await import("firebase/auth");

      ui = firebaseui.auth.AuthUI.getInstance();
      if (!ui) {
        ui = new firebaseui.auth.AuthUI(getAuth(app));
      }

      uiConfig = {
        ...
      }

      ui.start("#firebaseui-auth-container", uiConfig); # Attaches localized widget
```


## Rebuild locales
```
$ cd scripts
$ chmod +X buildAll.zx
$ ./buildAll.zx
```

## Questions
1. Ask on Discord https://discord.gg/46rDVTTbut

