# Writeups PicoCTF — Session 1
**Auteur :** Étudiant L3 RIST  
**Date :** 30 avril 2026  
**Catégories :** General Skills, Cryptography  
**Challenges résolus :** 5

---

## 1. Obedient Cat — General Skills (1 pt)

### Énoncé
> "This file has a flag in plain sight (aka in-the-clear)."

Un fichier `flag` est fourni en téléchargement.

### Approche
L'énoncé dit "in plain sight" — le flag est visible directement sans aucun décodage. Il suffit d'ouvrir le fichier avec un éditeur de texte ou un navigateur.

### Solution
```
Téléchargement du fichier → ouverture directe → flag visible en clair
```

### Flag
```
picoCTF{s4n1ty_v3r1f13d_9b8fa0bc}
```

### Leçon retenue
Toujours lire l'énoncé attentivement. "In the clear" signifie non chiffré, non encodé — l'information est directement lisible. C'est le point d'entrée de tout CTF : identifier si une transformation est nécessaire ou non.

---

## 2. Wave a Flag — General Skills

### Énoncé
Utilisation d'un binaire en ligne de commande pour extraire le flag.

### Approche
Challenge introductif au terminal Linux. On exécute le binaire fourni avec le bon argument pour qu'il affiche le flag.

### Solution
```bash
chmod +x warm       # rendre le fichier exécutable
./warm -h           # afficher l'aide → le flag apparaît
```

### Leçon retenue
Premier réflexe terminal : `chmod +x` pour rendre un fichier exécutable, et toujours tester `-h` ou `--help` sur un binaire inconnu. Le terminal est l'outil central du pentesteur.

---

## 3. Warmed Up — General Skills

### Énoncé
Conversion d'un nombre hexadécimal en décimal.

### Approche
Challenge de conversion de base numérique. La valeur fournie est en hexadécimal (base 16), il faut la convertir en décimal (base 10).

### Solution
```
0x3D → 3×16 + 13 = 48 + 13 = 61
```

### Leçon retenue
Les bases numériques (binaire, octal, décimal, hexadécimal) sont fondamentales en cybersécurité. L'hexadécimal est omniprésent : adresses mémoire, hash, paquets réseau, shellcode.

---

## 4. Mod 26 / ROT13 — Cryptography

### Énoncé
Déchiffrer un message chiffré avec ROT13.

### Approche
ROT13 est un cas particulier du chiffre de César avec un décalage de 13. Comme l'alphabet contient 26 lettres, appliquer ROT13 deux fois redonne le message original.

**Formule :**
```
déchiffré = (position + 13) mod 26
```

### Solution
Application directe de ROT13 sur le message chiffré — en ligne de commande :
```bash
echo "MESSAGE_CHIFFRE" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

### Leçon retenue
L'arithmétique modulaire (`mod`) est le fondement de toute la cryptographie moderne. ROT13 en est l'exemple le plus simple. Ce concept réapparaît dans RSA, AES, les courbes elliptiques.

---

## 5. Easy1 (Vigenère) — Cryptography

### Énoncé
> "The one time pad can be cryptographically secure, but not when you know the key."  
> Message chiffré : `UFJKXQZQUNB`  
> Clé : `SOLVECRYPTO`  
> Une table de Vigenère est fournie.

### Approche
Le chiffre de Vigenère est une extension du César : chaque lettre du message est chiffrée avec un décalage différent, défini par la lettre correspondante de la clé.

**Formule de déchiffrement :**
```
déchiffré = (chiffré - clé) mod 26
```

**Déchiffrement lettre par lettre :**

| # | Chiffré | Clé | Déchiffré |
|---|---------|-----|-----------|
| 1 | U | S | C |
| 2 | F | O | R |
| 3 | J | L | Y |
| 4 | K | V | P |
| 5 | X | E | T |
| 6 | Q | C | O |
| 7 | Z | R | I |
| 8 | Q | Y | S |
| 9 | U | P | F |
| 10 | N | T | U |
| 11 | B | O | N |

**Résultat :** `CRYPTOISFUN`

### Solution
Utilisation de la table de Vigenère fournie : pour chaque lettre, trouver la ligne correspondant à la lettre clé, puis chercher la lettre chiffrée dans cette ligne — la colonne indique la lettre déchiffrée.

### Flag
```
picoCTF{CRYPTOISFUN}
```

### Leçon retenue
Vigenère = plusieurs César en parallèle. Si la clé est aussi longue que le message et utilisée une seule fois, c'est un **One Time Pad** — théoriquement incassable. Mais avec une clé connue ou réutilisée, il se casse facilement. La sécurité d'un chiffrement dépend autant de la gestion de la clé que de l'algorithme lui-même.

---

## Bilan de la session

| Challenge | Catégorie | Technique | Statut |
|-----------|-----------|-----------|--------|
| Obedient Cat | General Skills | Lecture de fichier | ✅ |
| Wave a Flag | General Skills | Terminal Linux | ✅ |
| Warmed Up | General Skills | Conversion de base | ✅ |
| Mod 26 | Cryptography | ROT13 / César | ✅ |
| Easy1 | Cryptography | Chiffre de Vigenère | ✅ |

**Compétences acquises :** manipulation de fichiers, terminal Linux, bases numériques, arithmétique modulaire, cryptographie classique (César, ROT13, Vigenère).

**Prochaines étapes :** Base64, chiffrement XOR, introduction au réseau (Wireshark, nmap).
