---
title: Wie ich meine Hugo-Website ohne lokale Kommandozeile mit GitHub Actions bereitstelle
date: 2024-08-31
draft: false
summary: 
categories:
- Hirnfurz
---

 ### TL;DR
Ich nutze GitHub Actions, um meine Hugo-Website automatisch zu bauen und mit rclone zu deployen. Der Workflow umfasst das Einrichten von Hugo, das Optimieren von CSS und JavaScript mit Webpack und PurgeCSS und das Hochladen der generierten Dateien auf den Webserver via SFTP mit rclone. Das spart Zeit und erhöht die Flexibilität, mit der ich Inhalte auf meiner Webseite erstellen und bearbeiten kann.

{{<centerimg alt="Build and Doploy Hugo Pages With Github Actions" src="feature_buildanddeploywithgithubaction.webp" maxwidth="100%">}}

## Wie ich meine Hugo-Website ohne lokale Kommandozeile mit GitHub Actions bereitstelle

Mit GitHub Actions kann ich meine Hugo-Website automatisch generieren und auf den Server laden. Das Ganze ist super praktisch und spart mir eine Menge Zeit. Wieso und wie ich das mit GitHub Actions mache, ist das Thema dieses Beitrags.

### Warum GitHub Actions?

Ich verwende Hugo, um diese Website zu erstellen. Während es viele Anleitungen gibt, wie man Hugo-Webseiten erstellt und jedes Theme auch eine mehr oder weniger umfassende Anleitung hat, wie es zu benutzen ist, gibt es leider sehr unterschiedliche Informationen darüber, wie man die generierten Dateien auf den Server bekommt, auf dem die Seite gehostet ist. Das liegt sicherlich daran, dass es dafür einfach sehr viele Möglichkeiten gibt.

Ich wollte diesen Prozess möglichst automatisieren. Da man für Hugo sowieso eine lokale Maschine mit Kommandozeile braucht, war die Wahl recht einfach: in der Kommandozeile bleiben und nachdem die Seite generiert wurde, mit `rclone` die Dateien auf den Webserver senden.

### Mobil und flexibel

Soweit so gut, aber ich wollte auch Beiträge nicht nur von meiner lokalen Maschine, auf der Hugo, Git und rclone installiert sind, erstellen, sondern auch, wenn ich mal an einem anderen Gerät bin, z.B. einem Tablet – das nicht mal eine Kommandozeile hat (was sehr schade ist, by the way).

Was tun? Mit Googles Hilfe bin ich auf diesen Blogeintrag gestoßen: [Hugo Deploy Static Page Using GitHub Actions](https://ruddra.com/hugo-deploy-static-page-using-github-actions/). Auf dessen Basis und mit der Hilfe von OpenAI habe ich mir folgende GitHub Action ([Einführung in GitHub Actions](https://medium.com/@innovativejude.tech/introduction-to-github-actions-b093753c9c2b)) erstellt, welche, wenn ich Dateien in meinem Git-Repository verändere, automatisch die neue Version erstellt und auf den Host-Server lädt.

So kann ich nicht nur meine Inhalte mobil von anderen Geräten hochladen oder verändern, sondern ich spare mir auch jedes Mal, wenn ich etwas erstelle oder anpasse, die wiederholte Ausführung mehrerer Befehle in der Kommandozeile.

Hier das komplette skript, im folgenden werden die einzelnen Schritte erläutert:
```yaml
name: HUGO-Website-Build-and-Deploy
on:
  workflow_dispatch:
  push:
    branches:
      - main

jobs:
  build:
    name: Build Hugo Page
    runs-on: ubuntu-latest
    steps:
      - name: 🛒 Checkout
        uses: actions/checkout@v3

      - name: ✨ Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "0.128.2"

      - name: Download blowfish-theme
        run: |
          git submodule update --init --recursive

      - name: 🛠️ Build with Hugo Pipes
        run: |
          hugo --gc --minify

      - name: Install Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - name: Install PurgeCSS and Webpack
        run: |
          npm install purgecss-webpack-plugin webpack webpack-cli glob style-loader css-loader

      - name: Create Webpack Config
        run: |
          echo "const path = require('path');" > webpack.config.js
          echo "const { PurgeCSSPlugin } = require('purgecss-webpack-plugin');" >> webpack.config.js
          echo "const glob = require('glob');" >> webpack.config.js
          echo "module.exports = {" >> webpack.config.js
          echo "  entry: './src/index.js'," >> webpack.config.js
          echo "  output: {" >> webpack.config.js
          echo "    filename: 'bundle.js'," >> webpack.config.js
          echo "    path: path.resolve(__dirname, 'public/js')" >> webpack.config.js
          echo "  }," >> webpack.config.js
          echo "  module: {" >> webpack.config.js
          echo "    rules: [" >> webpack.config.js
          echo "      {" >> webpack.config.js
          echo "        test: /\.css$/," >> webpack.config.js
          echo "        use: ['style-loader', 'css-loader']" >> webpack.config.js
          echo "      }" >> webpack.config.js
          echo "    ]" >> webpack.config.js
          echo "  }," >> webpack.config.js
          echo "  plugins: [" >> webpack.config.js
          echo "    new PurgeCSSPlugin({" >> webpack.config.js
          echo "      paths: glob.sync(\`\${path.join(__dirname, 'public')}/**/*\`, { nodir: true })," >> webpack.config.js
          echo "    })" >> webpack.config.js
          echo "  ]," >> webpack.config.js
          echo "  optimization: {" >> webpack.config.js
          echo "    usedExports: true," >> webpack.config.js
          echo "  }" >> webpack.config.js
          echo "};" >> webpack.config.js

      - name: Create Dummy JS Entry Point
        run: |
          mkdir -p src
          echo "console.log('Hello, world!');" > src/index.js

      - name: Run Webpack
        run: |
          npx webpack --mode production

      - name: Set up rclone
        run: |
          sudo -v ; curl https://rclone.org/install.sh | sudo bash
          echo "${{ secrets.RCLONE_CONFIG }}" >> rclone.conf
          chmod 600 rclone.conf
          mkdir -p $HOME/.config/rclone/
          mv rclone.conf $HOME/.config/rclone/
          rclone version

      - name: Sync files via SFTP
        run: |
          rclone sync public/ web-ripert:es
```

### GitHub Action: HUGO-Website-Build-and-Deployes
Diese GitHub Action wird auf zwei Arten ausgelöst:
- **Manuell:** Über `workflow_dispatch` kann ich den Workflow jederzeit manuell starten.
- **Automatisch:** Bei jedem Push auf den `main` Branch wird der Workflow automatisch ausgeführt.

#### Jobs und Schritte
Der Workflow läuft auf einem `ubuntu-latest` Betriebssystem und führt mehrere Schritte aus, um die Website zu bauen und zu deployen.

#### 🛒 Checkout
Zuerst wird der Code aus dem Repository ausgecheckt:
```yaml
- name: 🛒 Checkout
  uses: actions/checkout@v3
```

#### ✨ Setup Hugo
Dann wird Hugo eingerichtet:
```yaml
- name: ✨ Setup Hugo
  uses: peaceiris/actions-hugo@v2
  with:
    hugo-version: "0.128.2"
```

#### Download Blowfish Theme
Falls ihr ein Theme als Submodul verwendet, wird dieses hier aktualisiert:
```yaml
- name: update theme 
  run: |
    git submodule update --init --recursive
```

#### 🛠️ Build with Hugo Pipes
Jetzt wird die Website mit Hugo gebaut, inklusive Garbage Collection und Minifizierung:
```yaml
- name: 🛠️ Build with Hugo Pipes
  run: |
    hugo --gc --minify
```

### Optimierung mit Webpack und PurgeCSS – Optional!
Diese Schritte können ausgelassen werden, dann wird das Skript schlanker. Ich verwende Webpack und PurgeCSS, um die CSS und JavaScript vom verwendeten Theme zu optimieren. Dies habe ich erst kürzlich hinzugefügt, um den Energieverbrauch meines Blogs zu reduzieren (Spoiler für einen folgenden Blogbeitrag).

#### Install Node.js
Um Webpack und PurgeCSS zu nutzen, brauchen wir Node.js:
```yaml
- name: Install Node.js
  uses: actions/setup-node@v3
  with:
    node-version: '16'
```

#### Install PurgeCSS and Webpack
Dann installieren wir die nötigen Pakete:
```yaml
- name: Install PurgeCSS and Webpack
  run: |
    npm install purgecss-webpack-plugin webpack webpack-cli glob style-loader css-loader
```

#### Create Webpack Config
Hier erstellen wir die Webpack-Konfigurationsdatei:
```yaml
- name: Create Webpack Config
  run: |
    echo "const path = require('path');" > webpack.config.js
    echo "const { PurgeCSSPlugin } = require('purgecss-webpack-plugin');" >> webpack.config.js
    echo "const glob = require('glob');" >> webpack.config.js
    echo "module.exports = {" >> webpack.config.js
    echo " entry: './src/index.js'," >> webpack.config.js
    echo " output: {" >> webpack.config.js
    echo " filename: 'bundle.js'," >> webpack.config.js
    echo " path: path.resolve(__dirname, 'public/js')" >> webpack.config.js
    echo " }," >> webpack.config.js
    echo " module: {" >> webpack.config.js
    echo " rules: [" >> webpack.config.js
    echo " {" >> webpack.config.js
    echo " test: /\.css$/," >> webpack.config.js
    echo " use: ['style-loader', 'css-loader']" >> webpack.config.js
    echo " }" >> webpack.config.js
    echo " ]" >> webpack.config.js
    echo " }," >> webpack.config.js
    echo " plugins: [" >> webpack.config.js
    echo " new PurgeCSSPlugin({" >> webpack.config.js
    echo " paths: glob.sync(\`\${path.join(__dirname, 'public')}/**/*\`, { nodir: true })," >> webpack.config.js
    echo " })" >> webpack.config.js
    echo " ]," >> webpack.config.js
    echo " optimization: {" >> webpack.config.js
    echo " usedExports: true," >> webpack.config.js
    echo " }" >> webpack.config.js
    echo "};" >> webpack.config.js
```

#### Create Dummy JS Entry Point
Wir brauchen auch einen Dummy-JavaScript-Einstiegspunkt:
```yaml
- name: Create Dummy JS Entry Point
  run: |
    mkdir -p src
    echo "console.log('Hello, world!');" > src/index.js
```

#### Run Webpack
Jetzt führen wir Webpack im Produktionsmodus aus:
```yaml
- name: Run Webpack
  run: |
    npx webpack --mode production
```

### Deployment mit rclone
Dies ist der letzte Schritt. Ich verwende rclone, um die generierten und optimierten Dateien auf den Webserver zu laden.

#### Set up rclone
Wir richten rclone ein, um die Dateien auf den Webserver zu übertragen:
```yaml
- name: Set up rclone
  run: |
    sudo -v ; curl https://rclone.org/install.sh | sudo bash
    echo "${{ secrets.RCLONE_CONFIG }}" >> rclone.conf
    chmod 600 rclone.conf
    mkdir -p $HOME/.config/rclone/
    mv rclone.conf $HOME/.config/rclone/
    rclone version
```

Das RCLONE_CONFIG Secret ist wie folgt strukturiert:

```
[web-ripert] 
type = sftp 
host = sftp.example.com 
user = myusername 
port = 22 
pass = mypassword
```
 
#### Sync Files via SFTP
Zum Schluss synchronisieren wir die generierten Dateien, welche im Verzeichnis `public` liegen, mit dem Webserver: 
```yaml
- name: Sync files via SFTP
  run: |
    rclone sync public/ web-ripert:
```

---

### Zusammenfassung
Mit diesem GitHub Action Workflow baue und deploye ich meine Hugo-Website automatisch. Hier sind die drei wichtigsten Punkte:
1. **Automatisierung:** Der Workflow wird bei jedem Push auf den `main` Branch oder manuell ausgelöst.
2. **Optimierung:** Webpack und PurgeCSS sorgen für optimierte und schnelle Ladezeiten.
3. **Flexibilität:** Die Inhalte können von überall bearbeitet und erstellt werden, es ist nur noch der GitHub Zugang erforderlich.