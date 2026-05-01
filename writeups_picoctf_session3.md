# Writeups PicoCTF — Session 3
**Auteur :** AGBENONZAN Kossivi Jacques Junior, Étudiant L3 RIST  
**Date :** 01 Mai 2026  
**Catégories :** General Skills  
**Challenges résolus :** 5

---

## 11. Based — General Skills

### Énoncé
> "To get truly 1337, you must understand different data encodings, such as hexadecimal or binary. Can you get the flag from this program?"  
> Connexion : `nc fickle-tempest.picoctf.net 65332`

### Approche
Le serveur envoie des mots encodés en binaire, octal ou hexadécimal sous pression temporelle. Il faut identifier l'encodage et convertir rapidement.

**Identifier l'encodage :**
- `01001000...` → **Binaire** (chiffres 0 et 1 par groupes de 8)
- `o154 o151...` → **Octal** (précédé de `o`)
- `68656c6c6f` → **Hexadécimal** (chiffres 0-9 et lettres a-f)

### Solution
```python
# Binaire → texte
python3 -c "b='01100011 01101000'; print(''.join(chr(int(x,2)) for x in b.split()))"

# Octal → texte
python3 -c "o='o160 o151'; print(''.join(chr(int(x,8)) for x in o.split()))"

# Hexadécimal → texte
python3 -c "print(bytes.fromhex('70656172').decode())"
```

### Flag
```
picoCTF{learning_about_converting_values_6c3Fb625}
```

### Leçon retenue
Les trois encodages (binaire, octal, hexadécimal) sont omniprésents en cybersécurité. Les reconnaître visuellement et les convertir rapidement est un réflexe fondamental pour le forensics, le reverse engineering et l'analyse de paquets réseau.

---

## 12. Permissions — General Skills

### Énoncé
> "Can you read files in the root file?"  
> Connexion SSH fournie avec un compte utilisateur limité.

### Approche
Challenge d'**escalade de privilèges** via un binaire mal configuré en sudo. La première étape est toujours de vérifier ses permissions sudo :

```bash
sudo -l
```

Résultat : l'utilisateur peut lancer `/usr/bin/vi` en tant que root. Vi permet d'exécuter des commandes shell depuis l'intérieur — technique documentée sur **GTFOBins**.

### Solution
```bash
# 1. Lancer vi en root
sudo /usr/bin/vi

# 2. Dans vi, exécuter un shell root
:!/bin/bash

# 3. Lire le flag
cat /root/.flag
```

### Flag
```
picoCTF{uS1ng_v1m_3dit0r_1cee9dcb}
```

### Leçon retenue
`sudo -l` est la **première commande** à taper sur toute machine compromise. GTFOBins ([gtfobins.github.io](https://gtfobins.github.io)) recense tous les binaires Unix exploitables pour l'escalade de privilèges. Vi, less, find, python, wget... beaucoup de programmes courants peuvent devenir des vecteurs d'attaque si mal configurés en sudo.

---

## 13. Tab Tab Attack — General Skills

### Énoncé
Challenge d'autocomplétion Linux — naviguer dans une arborescence complexe en utilisant la touche Tab.

### Approche
L'autocomplétion avec `Tab` permet de naviguer rapidement dans des répertoires profondément imbriqués sans taper les noms complets. En combinant avec `file` et `strings`, on trouve et lit le flag.

### Solution
```bash
# Utiliser Tab pour naviguer
cd D<Tab><Tab>...

# Une fois dans le bon répertoire
strings fichier | grep "picoCTF"
# ou
cat fichier
```

### Flag
```
picoCTF{l3v3l_up!_t4k3_4_r35t!_fc588427}
```

### Leçon retenue
L'autocomplétion est un réflexe de productivité essentiel en terminal. En pentest, elle permet aussi d'explorer rapidement des systèmes de fichiers inconnus. Combinée à `find`, `ls -la` et `strings`, c'est un outil de reconnaissance puissant.

---

## 14. Static ain't always noise — General Skills

### Énoncé
> "Can you look at the data in this binary?"  
> Fichiers fournis : `static` (binaire) et `ltdis.sh` (script de désassemblage)

### Approche
Un binaire peut contenir des chaînes de caractères lisibles en clair. La commande `strings` extrait toutes ces chaînes, et `grep` filtre le flag.

**Deux méthodes :**

Méthode rapide (utilisée) :
```bash
strings static | grep "pico"
```

Méthode complète avec le script fourni :
```bash
chmod +x ltdis.sh
./ltdis.sh static
grep "pico" static.ltdis
```

### Flag
```
picoCTF{d15a5m_t34s3r_20335e41}
```

### Leçon retenue
En CTF, toujours essayer la solution simple d'abord (`strings + grep`) avant de sortir les gros outils. Le script `ltdis.sh` fait un désassemblage complet — utile quand le flag n'est pas en clair. C'est l'introduction au **reverse engineering** : analyser un binaire sans avoir le code source.

---

## 15. Blame Game — General Skills

### Énoncé
> "Someone's commits are preventing the program from running. Who is it?"  
> Un fichier `challenge.zip` contenant un dépôt Git est fourni.

### Approche
Challenge d'**analyse de l'historique Git**. Tous les commits portent le même message "important business work" pour noyer l'information. Il faut identifier le commit anormal et son auteur.

### Solution
```bash
# 1. Extraire et entrer dans le dépôt
unzip challenge.zip
cd drop-in

# 2. Voir l'historique des commits
git log --oneline

# 3. Les deux derniers commits sont différents
# 2466feb optimize file size of prod code
# 05f26d1 create top secret project

# 4. Inspecter le commit suspect
git show 05f26d1
git show 2466feb

# 5. L'auteur du commit problématique = le flag
```

### Flag
```
picoCTF{@git_gudde_@_blaming_7492022f}
```

### Leçon retenue
`git log` et `git show` sont des outils de **forensics** puissants. En pentest, l'historique Git d'un projet peut contenir des secrets : clés API, mots de passe, tokens d'accès commités par erreur. C'est une source d'information critique lors d'une phase de reconnaissance.

---

## Bilan Session 3

| # | Challenge | Catégorie | Technique | Statut |
|---|-----------|-----------|-----------|--------|
| 11 | Based | General Skills | Conversions binaire/octal/hex | ✅ |
| 12 | Permissions | General Skills | Escalade de privilèges (GTFOBins) | ✅ |
| 13 | Tab Tab Attack | General Skills | Autocomplétion Linux | ✅ |
| 14 | Static ain't always noise | General Skills | strings + reverse engineering | ✅ |
| 15 | Blame Game | General Skills | Forensics Git | ✅ |

**Compétences acquises :** encodages (binaire/octal/hex), escalade de privilèges sudo, GTFOBins, autocomplétion terminal, analyse de binaires, forensics Git.

---

## Bilan Global (Sessions 1, 2 & 3)

| Session | Challenges | Techniques couvertes |
|---------|------------|----------------------|
| Session 1 | 5 | Fichiers, terminal, ROT13, Vigenère |
| Session 2 | 5 | Binaire, César, grep, strings, netcat |
| Session 3 | 5 | Encodages, privesc, GTFOBins, reverse, git |
| **Total** | **15** | **General Skills & Cryptography** |

**Prochaines étapes recommandées :** Web Exploitation (injection SQL, XSS), Forensics (Wireshark, analyse d'images), introduction à TryHackMe Jr Penetration Tester.
