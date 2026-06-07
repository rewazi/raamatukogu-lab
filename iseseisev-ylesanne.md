# Raamatukogu labor
## 2 × 90 minutit | Windows

---

## Stsenaarium

Sa oled just liitunud arendustiimiga. Tiimijuht saadab sulle emaili:

---

*Tere!*

*Meie raamatukogu rakendus on valmis aga vajab tähelepanu. Sul on kaks tundi. Palun tee järgmist:*

*1. Seadista projekt GitHubis — Kanban tahvel, Issues, sprint planeerimine*
*2. Lisa automaattestid GitHub Actions-iga*
*3. Täida README dokumentatsioon*
*4. Lisa kaks uut funktsiooni mida klient ootab:*
   *- Otsing autori järgi*
   *- Statistikasse tagastatud laenude arv*

*Veendu et kõik automaattestid läbivad enne kui pushid main branchi.*

*Edu!*
*Tiimijuht*

---

## Etapp 1 — Seadistamine

**Ülesanne:** Fork repo, klooni ja käivita rakendus.

Kasuta GitHub dokumentatsiooni: **docs.github.com**

Rakendus peab jooksma aadressil **http://localhost:3000** enne kui jätkad.

---

## Etapp 2 — GitHub Project ja Issues

**Ülesanne:** Loo Kanban tahvel ja lisa ülesanded.

Kasuta GitHub dokumentatsiooni kuidas luua:
- GitHub Project Kanban tahvliga
- Issues backlogist
- Sprindi planeerimine — lohista Issues veergudesse

**Loo järgmised Issues:**

1. `Lisa automaattestid GitHub Actions-iga`
2. `Täida README dokumentatsioon`
3. `Lisa otsing autori järgi`
4. `Lisa tagastatud laenude arv statistikasse`

**Kanban veerud:** Todo → In Progress → Review → Done

Planeeri sprint — vali millised Issues lähevad **In Progress** veergu.

---

## Etapp 3 — GitHub Actions

**Ülesanne:** Kirjuta automaattestide workflow.

Loo fail `.github\workflows\ci.yml` ja kopeeri sisu:

```yaml
name: CI Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    name: Automaattestid
    runs-on: ubuntu-latest
    steps:
      - name: Lae kood alla
        uses: actions/checkout@v4

      - name: Paigalda Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Paigalda sõltuvused
        run: npm install

      - name: Käivita rakendus
        run: node src/server.js &

      - name: Oota kuni server käivitub
        run: sleep 3

      - name: Käivita testid
        run: node src/test.js

  lint:
    name: Koodi kontroll
    runs-on: ubuntu-latest
    steps:
      - name: Lae kood alla
        uses: actions/checkout@v4

      - name: Paigalda Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Paigalda sõltuvused
        run: npm install

      - name: Kontrolli süntaksi vigu
        run: node --check src/server.js src/routes/users.js src/routes/books.js src/routes/loans.js

  deploy:
    name: Deploy kontroll
    runs-on: ubuntu-latest
    needs: [test, lint]
    if: github.ref == 'refs/heads/main'
    steps:
      - name: Lae kood alla
        uses: actions/checkout@v4

      - name: Ehita Docker image
        run: docker compose build

      - name: Teata edukast deployst
        run: |
          echo "Kõik testid läbisid!"
          echo "Commit: ${{ github.sha }}"
```

Commit ja push:

```powershell
git add .github
git commit -m "Lisa GitHub Actions closes #1"
git push
```

Kontrolli GitHubis → **Actions** — pipeline peab läbima.

---

## Etapp 4 — Koodimuudatused

**Ülesanne:** Tee kaks muudatust mida klient ootab.

Loo sprint branch:

```powershell
git checkout -b sprint-1
```

**Muudatus 1 — Otsing autori järgi**

Ava `src/routes/books.js` ja leia `/search` endpoint. Uuri kuidas see praegu töötab. Lisa autori otsing nii et see töötab — kasuta internetist Express.js dokumentatsiooni kui vaja.

Testi:
```powershell
Invoke-WebRequest "http://localhost:3000/api/books/search?author=Tammsaare" | Select-Object -ExpandProperty Content
```

Commit:
```powershell
git add src/routes/books.js
git commit -m "Lisa otsing autori järgi closes #3"
```

**Muudatus 2 — Tagastatud laenude arv**

Ava `src/server.js` ja leia `/api/stats` endpoint. Lisa `returnedLoans` väli — tagastatud laenude arv.

Testi:
```powershell
Invoke-WebRequest http://localhost:3000/api/stats | Select-Object -ExpandProperty Content
```

Commit:
```powershell
git add src/server.js
git commit -m "Lisa tagastatud laenude arv statistikasse closes #4"
```

---

## Etapp 5 — Testid ja merge

**Ülesanne:** Veendu et kõik testid läbivad.

```powershell
docker compose down
npm install
node src/test.js
```

Kõik testid peavad näitama `PASS`. Kui mõni ebaõnnestub — paranda viga enne merge-i.

Merge ja push:

```powershell
git checkout main
git merge sprint-1
git push
```

Kontrolli GitHubis → **Actions** — kõik kolm tööd peavad läbima.

---

## Etapp 6 — Dokumentatsioon

**Ülesanne:** Täida README.

Ava `README.md` ja täida kõik `<!-- TODO -->` kohad. README peab olema piisavalt hea et uus arendaja saab rakenduse käivitada ilma sinu abita.

Commit ja push:

```powershell
git add README.md
git commit -m "Täida README closes #2"
git push
```

---

## Etapp 7 — Sprint lõpetamine

**Ülesanne:** Sulge kõik Issues ja uuenda Kanban tahvlit.

1. Mine GitHubis → **Projects** → `Raamatukogu Sprint 1`
2. Kontrolli et kõik Issues on **Done** veerus
3. Kui mõni Issue pole automaatselt suletud — sulge käsitsi

Vaata git ajalugu:

```powershell
git log --oneline
```

Peata konteiner:

```powershell
docker compose down
```

---

## Hindamine

Sprint on edukalt lõpetatud kui:

- Kõik 4 Issue-t on **Done** veerus
- GitHub Actions pipeline on roheline
- README on täidetud
- Kaks uut funktsiooni töötavad
- Kõik automaattestid läbivad
