# Esperto Alberi — App Android (build via GitHub Actions)

Questo progetto impacchetta `esperto_alberi.html` + `alberi.csv` dentro un
contenitore nativo Android usando **Capacitor**. La compilazione dell'APK
avviene **interamente su GitHub** (runner Linux), quindi sul tuo Mac con
Big Sur non serve installare Android Studio: ti basta Node.js, che hai già.

## Struttura del progetto

```
esperto-alberi-app/
├── www/                        ← il tuo codice web (sorgente di verità)
│   ├── index.html              ← era esperto_alberi.html
│   ├── alberi.csv
│   └── immagini/
│       ├── foglie/
│       ├── foglie_dettaglio/
│       ├── tronchi/
│       ├── frutti/
│       └── chiome/
├── android/                    ← progetto nativo generato da Capacitor
├── capacitor.config.ts         ← configurazione (nome app, id pacchetto)
└── .github/workflows/
    └── build-apk.yml           ← il workflow che compila l'APK
```

**Importante:** da qui in avanti modifichi SOLO i file dentro `www/`
(HTML, CSV, immagini). La cartella `android/` non va toccata a mano:
si rigenera automaticamente con `npx cap sync`.

## 1. Aggiungi le immagini reali

Le sottocartelle in `www/immagini/` sono vuote (placeholder). Copiaci
dentro i tuoi file `.jpg` con i nomi corrispondenti agli `id` del CSV,
es: `www/immagini/foglie/abete_douglas.jpg`.

## 2. Primo push su GitHub

Sul tuo Mac, dentro questa cartella:

```bash
git init
git add .
git commit -m "Setup iniziale app Capacitor"
git branch -M main
git remote add origin https://github.com/TUO-USERNAME/esperto-alberi-app.git
git push -u origin main
```

(Crea prima il repository vuoto su GitHub, senza README, da
github.com/new — poi usa l'URL che ti propone per il remote.)

## 3. La build parte automaticamente

Ogni `git push` su `main` fa partire il workflow
`.github/workflows/build-apk.yml`. Puoi seguirlo dal vivo nel tab
**Actions** del tuo repository GitHub.

Puoi anche farla partire manualmente senza nuovi push: tab **Actions** →
seleziona "Build APK Android" → **Run workflow**.

## 4. Scarica l'APK

A build completata (3-5 minuti circa la prima volta, poi più veloce
grazie alla cache):
- Apri il workflow run completato (icona verde ✔)
- In fondo alla pagina, sezione **Artifacts**
- Scarica `esperto-alberi-debug-apk` (è uno zip contenente `app-debug.apk`)

## 5. Installa l'APK sul telefono

Trasferisci `app-debug.apk` sul telefono Android (es. via cavo, email a
te stesso, Google Drive...) e aprilo per installarlo. Dovrai consentire
"Installa da sorgenti sconosciute" la prima volta — è normale per un APK
non pubblicato sul Play Store.

## Modifiche successive (workflow quotidiano)

1. Modifichi `www/index.html`, `www/alberi.csv` o le immagini
2. `git add . && git commit -m "..." && git push`
3. Aspetti che GitHub Actions finisca la build
4. Scarichi il nuovo APK dagli Artifacts

## Testare velocemente senza ricompilare l'APK

Per modifiche solo all'HTML/CSV/logica web, puoi continuare a testare
direttamente in un browser locale (es. con `npx serve www` oppure aprendo
un server statico) — è lo stesso identico codice che gira anche dentro
l'app nativa. La build Android serve solo per il pacchetto finale
installabile.

## Quando vorrai distribuire pubblicamente (Play Store)

L'APK di debug generato qui va benissimo per test personali (lo installi
tu, da sideload). Per pubblicare su Google Play servirà invece un APK/AAB
**firmato con una chiave di release** — un passaggio successivo che
richiede di generare un keystore e salvarlo come "secret" su GitHub
(mai nel repository in chiaro). Fammi sapere quando sei a quel punto e
ti preparo anche quello.
