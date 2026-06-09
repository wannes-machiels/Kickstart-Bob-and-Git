# IBM i BOB — Installatiegids

## Inhoud

1. [Installatie van Node.js](#1-installatie-van-nodejs)
2. [Installatie van Git](#2-installatie-van-git)
3. [Installatie van IBM i BOB](#3-installatie-van-ibm-i-bob)
4. [Inloggen met IBM account](#4-inloggen-met-ibm-account)
5. [Oefenbestanden downloaden van GitHub en importeren](#5-oefenbestanden-downloaden-van-github-en-importeren)
6. [Connecteren met een IBM i host](#6-connecteren-met-een-ibm-i-host)

---

## Vereisten

Voor je met IBM i BOB kan werken, moet je volgende tools geïnstalleerd hebben:

1. Node.js (laatste LTS versie)
2. Git

---

## 1. Installatie van Node.js

### Via winget

Open cmd als administrator en voer uit:

```bash
winget install -e --id OpenJS.NodeJS.LTS
```

### Stappenplan

1. Open Startmenu
2. Zoek naar `cmd`
3. Klik rechts → **Als administrator uitvoeren**
4. Voer het installatiecommando uit
5. Wacht tot installatie klaar is
6. Controleer de installatie:

```bash
node --version
```

### Resultaat

- Zie je een versie → Node.js is geïnstalleerd ✅
- Krijg je een fout → Node.js is niet geïnstalleerd ❌

---

## 2. Installatie van Git

### 2.1 Installatie via winget

Open cmd en voer uit:

```bash
winget install Git.Git
```

### 2.2 Stappen

1. Open Command Prompt
2. Voer het commando uit
3. Wacht tot installatie voltooid is
4. Controleer de installatie:

```bash
git --version
```

Als je een versie ziet, is Git correct geïnstalleerd ✅

---

## 3. Installatie van IBM i BOB

### 3.1 Download IBM i BOB

Ga naar [IBM Bob](https://bob.ibm.com/)

### 3.2 Stappen

1. Open de website in je browser
2. Klik op **Download**
3. Kies de juiste versie voor jouw systeem
4. Start de download

### 3.3 Installatie

1. Open het gedownloade bestand
2. Volg de installatie-wizard:
   - Next
   - Accept voorwaarden
   - Install
3. Wacht tot installatie voltooid is
4. Start IBM i BOB

---

## 4. Inloggen met IBM account

Om IBM i BOB te gebruiken heb je een IBM account nodig.

### 4.1 Stappen

1. Start IBM i BOB
2. Je krijgt een login scherm
3. Klik op **Sign in / Log in**
4. Meld je aan met je IBM-account
   - Heb je nog geen account? → Kies **Create account** en volg de registratie

Na succesvolle login kan je beginnen met werken in IBM i BOB.

---

## 5. Oefenbestanden downloaden van GitHub en importeren

Repository: [GitHub — bmarolleau/IBM-i-Application-Modernization-with-Bob](https://github.com/bmarolleau/IBM-i-Application-Modernization-with-Bob)

### 5.1 Stappen

1. Open IBM i BOB
2. Ga naar: **Source Control**
3. Kies **Clone repository**
4. Kopieer en plak de repository URL:

```bash
https://github.com/bmarolleau/IBM-i-Application-Modernization-with-Bob.git
```

5. Kies een lokale map waar het project opgeslagen wordt
6. Klik op **Clone**

### Resultaat

- De repository wordt lokaal gedownload
- Het project wordt automatisch beschikbaar in IBM i BOB
- De oefenbestanden zijn terug te vinden in IBM i BOB onder het menu item **Explorer**

---

## 6. Connecteren met een IBM i host

### 6.1 Stappen

1. Open IBM i BOB
2. Open het menu item **IBM i**
3. Klik op de **`+`** knop
4. Vul de volgende gegevens in:
   - Connection name (vrij te kiezen)
   - Host or IP address
   - Port
   - Username
5. Klik op **Save & Exit**
6. Je server verschijnt nu in het overzicht — verbind door op de **play** knop te klikken
7. Bovenaan in het midden verschijnt een prompt met de vraag om je wachtwoord in te geven — geef je wachtwoord in en druk op **Enter**
8. Er wordt nu verbinding gemaakt met je IBM i host

### Resultaat

Als je **User Library List** verschijnt, is de verbinding gelukt ✅
