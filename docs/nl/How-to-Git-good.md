# 🐙 How to Git Good

## Inhoud

1. [Basis configuratie](#1-basis-configuratie)
2. [Repository aanmaken](#2-repository-aanmaken)
3. [Dagelijkse workflow](#3-dagelijkse-workflow)
4. [Werken met remote repositories](#4-werken-met-remote-repositories)
5. [Branches](#5-branches)
6. [Samenwerken](#6-samenwerken)
7. [Handige extra commando's](#7-handige-extra-commandos)
8. [Volledig voorbeeld: project publiceren](#8-volledig-voorbeeld-project-publiceren)

📋 [Snelle referentiekaart](git-referentiekaart.md)

🔙 [Terug naar hoofdmenu](../../README.md)

---

## 1. Basis configuratie

### Installatie

#### 1.1 Installatie via winget

1. Open Command Prompt
2. Voer onderstaand commando uit:

```bash
winget install Git.Git
```

3. Wacht tot installatie voltooid is
4. Controleer of de installatie is gelukt:

```bash
git --version
```

Als je een versie ziet, is Git correct geïnstalleerd ✅

#### 1.2 Alternatief

1. Download Git via [git-scm.com](https://git-scm.com)
2. Installeer Git, volg de stappen van de installer
3. Controleer of de installatie is gelukt:

```bash
git --version
```

Als je een versie ziet, is Git correct geïnstalleerd ✅

---

### Naam en e-mail instellen

Git koppelt elke commit aan jouw naam en e-mail. Dit is verplicht voordat je je eerste commit maakt.

```bash
git config --global user.name "Jouw Naam"
git config --global user.email "jij@voorbeeld.com"
```

> **Tip:** `--global` geldt voor alle projecten op je computer. Gebruik `--local` om alleen voor het huidige project een andere instelling te gebruiken.

Controleer je instellingen:

```bash
git config --list
```

---

### Authenticatie: SSH vs HTTPS

| Methode | Hoe het werkt | Aanbevolen voor |
|---|---|---|
| HTTPS | Werkt met gebruikersnaam + persoonlijk token (PAT) | Beginners |
| SSH | Eenmalig sleutelpaar aanmaken; daarna geen wachtwoord meer | Gevorderden |

```bash
# HTTPS-formaat
https://github.com/gebruiker/repo.git
# SSH-formaat
git@github.com:gebruiker/repo.git
```

> **Tip:** Begin met HTTPS. GitHub vraagt eenmalig om een persoonlijk toegangstoken (PAT) dat je aanmaakt via **Settings → Developer settings → Personal access tokens**.

---

## 2. Repository aanmaken

### Nieuw project initialiseren

Maakt een verborgen `.git`-map aan — dit is de database van Git voor jouw project.

```bash
mkdir mijn-project
cd mijn-project
git init
```

### Bestaande repository klonen

Downloadt een volledig project inclusief alle geschiedenis. Er wordt automatisch een map aangemaakt.

```bash
git clone https://github.com/gebruiker/repo.git
# Klonen in een zelfgekozen mapnaam
git clone https://github.com/gebruiker/repo.git mijn-map
```

> **Tip:** Na het klonen staat de remote al automatisch ingesteld als `origin`. Je hoeft niets extra te configureren.

---

## 3. Dagelijkse workflow

De kern van Git bestaat uit drie stappen die je telkens herhaalt:

```
Bestand aanpassen → git add → git commit
```

### Stap 1 — Status bekijken

Zie welke bestanden veranderd zijn, gestagd of nog ongewijzigd zijn.

```bash
git status
```

### Stap 2 — Bestanden stagen (`git add`)

Voeg bestanden toe aan de "wachtkamer" (staging area) voor de volgende commit.

```bash
# Eén specifiek bestand
git add bestand.txt
# Meerdere bestanden tegelijk
git add bestand1.txt bestand2.txt
# Alles in de huidige map (inclusief submappen)
git add .
```

> ⚠️ **Veelgemaakte fout:** `git add .` uitvoeren en dan vergeten te committen. Controleer altijd met `git status` wat er in de staging area staat.

### Stap 3 — Committen (`git commit`)

Sla de gestagede bestanden op als een permanent snapshot met een beschrijvend bericht.

```bash
git commit -m "Voeg inlogpagina toe"
```

Tips voor goede commit-berichten:

```bash
# ✓ Goed: beschrijft wat én waarom
git commit -m "Voeg validatie toe aan inlogformulier"
git commit -m "Los crash op bij legen winkelmandje"

# ✗ Slecht: te vaag
git commit -m "fix"
git commit -m "update"
```

### Geschiedenis bekijken (`git log`)

Toon alle commits in omgekeerde chronologische volgorde.

```bash
# Uitgebreid overzicht
git log
# Compacte weergave (één regel per commit)
git log --oneline
# Grafisch overzicht van alle branches
git log --oneline --graph --all
```

---

## 4. Werken met remote repositories

### Remote verbinding instellen (`git remote`)

Koppel je lokale project aan een repository op GitHub of GitLab. De naam `origin` is de standaardconventie.

```bash
git remote add origin https://github.com/jij/repo.git
# Controleer ingestelde remotes
git remote -v
```

### Code pushen (`git push`)

Stuur jouw lokale commits naar de remote server.

```bash
# Eerste keer: koppel de lokale branch aan de remote branch
git push -u origin main
# Daarna gewoon
git push
```

> **Tip:** De `-u` vlag hoef je maar één keer te gebruiken. Daarna onthoudt Git de koppeling automatisch.

### Code ophalen

```bash
# Haal op én voeg direct samen (meest gebruikt)
git pull
# Alleen ophalen, nog niet samenvoegen
git fetch origin
```

Zie [sectie 6](#6-samenwerken) voor het verschil tussen `pull` en `fetch`.

---

## 5. Branches

Een branch is een aparte werklijn. Zo werk je aan nieuwe functies zonder de stabiele hoofdcode te verstoren.

### Branches beheren

```bash
# Overzicht van alle lokale branches (* = huidige branch)
git branch
# Nieuwe branch aanmaken
git branch mijn-feature
# Wisselen naar een branch
git switch mijn-feature
# Aanmaken én meteen wisselen (handigste shortcut)
git switch -c mijn-feature
# Branch verwijderen (nadat je hem gemerged hebt)
git branch -d mijn-feature
```

### Branches samenvoegen (`git merge`)

Voeg de wijzigingen van een branch samen in de huidige branch.

```bash
# 1. Ga naar de doelbranch (meestal main)
git switch main
# 2. Voeg de feature-branch samen
git merge mijn-feature
```

### Best practice: feature branches

Maak voor elke nieuwe functie of bugfix een aparte branch. Zo blijft `main` altijd stabiel en werkend.

```bash
# Aanbevolen naamgeving
git switch -c feature/gebruikersinstellingen
git switch -c fix/crashbug-inlog
git switch -c docs/readme-bijwerken
```

> **Tip:** Verwijder branches na het mergen om het overzicht te bewaren:

```bash
git branch -d feature/gebruikersinstellingen
```

---

## 6. Samenwerken

### `git pull` vs `git fetch` — wat is het verschil?

| Commando | Wat doet het? |
|---|---|
| `git fetch` | Downloadt wijzigingen van de remote, maar past jouw lokale code nog niet aan. Veilig om eerst te bekijken. |
| `git pull` | Doet een fetch + merge in één stap. Jouw code wordt direct bijgewerkt. |

```bash
# Veilige aanpak: eerst bekijken wat er veranderd is
git fetch origin
git log origin/main    # bekijk nieuwe commits
git merge origin/main  # daarna pas samenvoegen

# Snelle aanpak (meest gebruikt)
git pull
```

### Pull Requests (GitHub / GitLab)

Een Pull Request (PR) of Merge Request (MR) is een verzoek om jouw branch samen te voegen in `main`. Het is een reviewproces — geen Git-commando.

**Stappen:**

1. Push je feature-branch naar de remote:

```bash
git push -u origin feature/mijn-feature
```

2. Ga naar GitHub/GitLab en klik op **New Pull Request** (of **Merge Request**)
3. Beschrijf je wijzigingen en wijs een reviewer aan
4. Na goedkeuring: de branch wordt gemerged in `main`

### Conflicten herkennen en oplossen

Een conflict ontstaat wanneer twee personen dezelfde regel anders hebben aangepast. Git kan dit niet automatisch oplossen.

Hoe ziet een conflict eruit?

```
<<<<<<< HEAD
Dit is jouw versie van de code
=======
Dit is de versie van de remote
>>>>>>> origin/main
```

**Stap voor stap oplossen:**

1. Open het conflictbestand in je editor
2. Kies welke versie je wilt bewaren (of combineer beide)
3. Verwijder alle conflictmarkeringen (`<<<<<<<`, `=======`, `>>>>>>>`)
4. Stage en commit het opgeloste bestand:

```bash
git add opgelost-bestand.txt
git commit -m "Los merge-conflict op in bestand.txt"
```

> **Tip:** Editors zoals VS Code markeren conflicten visueel met knoppen **Accept Current** / **Accept Incoming** — dat maakt het een stuk makkelijker.

---

## 7. Handige extra commando's

### `git diff` — wijzigingen vergelijken

Toont precies welke regels veranderd zijn tegenover de laatste commit.

```bash
# Niet-gestagede wijzigingen (werkmap vs. laatste commit)
git diff
# Gestagede wijzigingen (klaar voor commit)
git diff --staged
# Twee branches vergelijken
git diff main mijn-feature
```

### `git stash` — tijdelijk opzijzetten

Bewaar onafgewerkte wijzigingen tijdelijk zodat je van branch kunt wisselen zonder te committen.

```bash
# Huidige wijzigingen opzijzetten
git stash
# Overzicht van opgeslagen stashes
git stash list
# Meest recente stash terughalen (en verwijderen uit de lijst)
git stash pop
# Stash terughalen maar in de lijst bewaren
git stash apply
```

**Gebruik geval:** Je bent halverwege een feature en moet snel een bugfix op `main` doen. Gebruik `git stash` om je werk tijdelijk op te slaan, fix de bug, en haal je werk terug met `git stash pop`.

### `git reset` — ongedaan maken (veilig gebruik)

```bash
# Bestand unstagen — wijzigingen blijven behouden in je bestanden
git reset HEAD bestand.txt
# Laatste commit ongedaan maken — wijzigingen blijven behouden
git reset --soft HEAD~1
```

> ⚠️ **Waarschuwing:** Gebruik nooit `git reset --hard` als beginner — dit verwijdert ook je bestandswijzigingen permanent en is niet te herstellen.

---

## 8. Volledig voorbeeld: project publiceren

Hieronder vind je de volledige stroom van een nieuw project — van nul tot gepubliceerd op GitHub.

```bash
# Stap 1: Git instellen (eenmalig)
git config --global user.name "Jouw Naam"
git config --global user.email "jij@email.com"

# Stap 2: Project aanmaken en initialiseren
mkdir mijn-app
cd mijn-app
git init

# Stap 3: Eerste bestand en commit
echo "# Mijn App" > README.md
git add .
git commit -m "Eerste commit: voeg README toe"

# Stap 4: Feature-branch aanmaken en werken
git switch -c feature/startpagina
# ... bestanden aanmaken en bewerken ...
git add index.html style.css
git commit -m "Voeg startpagina met styling toe"

# Stap 5: Samenvoegen naar main
git switch main
git merge feature/startpagina
git branch -d feature/startpagina

# Stap 6: Verbinden met GitHub en pushen
git remote add origin https://github.com/jij/mijn-app.git
git push -u origin main

# Stap 7: Dagelijkse cyclus daarna
git pull                        # haal de laatste wijzigingen op
# ... werk aan code ...
git add .
git commit -m "Beschrijving van de wijziging"
git push
```

---

📋 Bekijk de [Snelle referentiekaart](git-referentiekaart.md) voor een compact overzicht van alle commando's.

---

🔙 [Terug naar hoofdmenu](../../README.md)
