# BM Kuchyně – Návod na nasazení

## Co potřebujete
- **GitHub účet** (zdarma) – pro hosting stránky
- **Google účet** (zdarma) – pro Firebase databázi

---

## Krok 1: Firebase (databáze pro sdílení objednávek)

### 1.1 Vytvořte projekt
1. Otevřete https://console.firebase.google.com
2. Klikněte **"Add project"** → název: `bm-kuchyne` → pokračujte (Google Analytics můžete vypnout)
3. Počkejte na vytvoření projektu

### 1.2 Zapněte Firestore
1. V levém menu klikněte **"Firestore Database"**
2. Klikněte **"Create database"**
3. Vyberte **"Start in test mode"** → klikněte **Next**
4. Location: `europe-west1` (Belgie) → **Enable**

### 1.3 Nastavte bezpečnostní pravidla
1. V Firestore klikněte na záložku **"Rules"**
2. Nahraďte obsah tímto:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```
3. Klikněte **"Publish"**

### 1.4 Zapněte anonymní přihlašování
1. V levém menu **"Authentication"** → **"Get started"**
2. Záložka **"Sign-in method"**
3. Najděte **"Anonymous"** → zapněte → **Save**

### 1.5 Zkopírujte konfiguraci
1. V levém menu klikněte na ⚙️ **"Project settings"**
2. Scrollujte dolů k **"Your apps"** → klikněte na ikonu **Web** (`</>`)
3. Pojmenujte: `bm-kuchyne` → **Register app** (hosting nezaškrtávejte)
4. Zobrazí se konfigurační objekt:
```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "bm-kuchyne.firebaseapp.com",
  projectId: "bm-kuchyne",
  ...
};
```
5. **Zkopírujte tyto hodnoty** – budete je potřebovat v kroku 2

---

## Krok 2: Vložte Firebase config do aplikace

Otevřete soubor `index.html` a najděte sekci (ctrl+F hledejte `PASTE-YOUR`):
```js
const FIREBASE_CONFIG = {
  apiKey: "PASTE-YOUR-API-KEY",
  ...
};
```
Nahraďte placeholder hodnoty skutečnými z Firebase Console.

---

## Krok 3: GitHub Pages (hosting)

### 3.1 Vytvořte repozitář
1. Jděte na https://github.com/new
2. Název: `bm-kuchyne`
3. Viditelnost: **Public** (nebo Private – GitHub Pages funguje i s Private)
4. **Create repository**

### 3.2 Nahrajte soubory
Nejjednodušší způsob – přes webové rozhraní:
1. Na stránce nového repozitáře klikněte **"uploading an existing file"**
2. Přetáhněte soubory:
   - `index.html`
   - složku `data/` (se soubory `catalog.json` a `prices.json`)
3. Klikněte **"Commit changes"**

Alternativně přes git:
```bash
cd bm-kuchyne
git init
git add .
git commit -m "Initial: objednávkový systém BM Kuchyně"
git remote add origin https://github.com/VAŠE-JMÉNO/bm-kuchyne.git
git push -u origin main
```

### 3.3 Zapněte GitHub Pages
1. V repozitáři → **Settings** → **Pages** (v levém menu)
2. Source: **Deploy from a branch**
3. Branch: `main` / `/ (root)` → **Save**
4. Za 1–2 minuty bude stránka dostupná na: `https://VAŠE-JMÉNO.github.io/bm-kuchyne/`

---

## Krok 4: Sdílejte s kuchaři

Pošlete odkaz `https://VAŠE-JMÉNO.github.io/bm-kuchyne/` kuchařům.

**Na tabletu/mobilu:** otevřete odkaz v Chrome → menu (⋮) → **"Přidat na plochu"** – bude se chovat jako aplikace.

---

## Jak Claude aktualizuje ceny

1. Pošlete mi ceník od dodavatele (PDF, Excel, foto, e-mail – cokoliv)
2. Já zpracuji ceny a upravím soubor `data/prices.json` ve vašem repozitáři
3. GitHub Pages se automaticky aktualizuje (~30 sekund)
4. Kuchaři uvidí nové ceny při dalším načtení stránky

Stejně tak mohu přidávat/upravovat položky a dodavatele v `data/catalog.json`.

---

## Náklady
- **GitHub Pages**: zdarma
- **Firebase Spark (free tier)**: zdarma (50 000 čtení/den, 20 000 zápisů/den)
- Pro 2–5 uživatelů se nikdy nepřekročí
