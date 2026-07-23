# Writeups PicoCTF — Session 2
**Auteur :** Agbenonzan Kossivi Jacques Junior, Étudiant L3 RIST  
**Date :** 30 avril 2026  
**Catégories :** General Skills, Cryptography  
**Challenges résolus :** 5

---

## 6. 2Warm — General Skills

### Énoncé
> "Can you convert the number 42 (base 10) to binary (base 2)?"

### Approche
Conversion d'un nombre décimal en binaire par divisions successives par 2.

### Solution
```
42 ÷ 2 = 21  reste 0
21 ÷ 2 = 10  reste 1
10 ÷ 2 = 5   reste 0
 5 ÷ 2 = 2   reste 1
 2 ÷ 2 = 1   reste 0
 1 ÷ 2 = 0   reste 1

→ On lit les restes de bas en haut : 101010
```

### Flag
```
picoCTF{101010}
```

### Leçon retenue
Les conversions de bases (binaire, décimal, hexadécimal) sont omniprésentes en cybersécurité : adresses mémoire, shellcode, paquets réseau. Les maîtriser de tête est un réflexe fondamental.

---

## 7. Caesar — Cryptography

### Énoncé
> "Decrypt this message."  
> Message : `rgdhhxcviwtgjqxrdcbxpotman`

### Approche
Chiffre de César — chaque lettre a été décalée d'un nombre fixe de positions. Sans connaître le décalage, on applique une **attaque par force brute** : tester les 26 décalages possibles jusqu'à trouver un résultat lisible.

**Outil utilisé :** [dcode.fr/caesar-cipher](https://www.dcode.fr/caesar-cipher)

### Solution
```
Décalage trouvé : 15
rgdhhxcviwtgjqxrdcbxpotman → crossingtherubiconmiazexly
```

"Crossing the Rubicon" — expression de Jules César signifiant franchir le point de non-retour. Beau clin d'œil historique pour un challenge César !

```
Code Python pour décryptage :
ciphertext = "Votre Hash Ici"

for shift in range(26):
    decoded = "".join(
        chr((ord(char) - 97 + shift) % 26 + 97) for char in ciphertext
    )
    print(f"Shift +{shift:2d} : {decoded}")
```

### Flag
```
picoCTF{crossingtherubiconmiazexly}
```

### Leçon retenue
En CTF, toujours soumettre le résultat brut complet — même les caractères qui semblent être du bruit font souvent partie du flag. La force brute sur 26 possibilités est triviale pour César, mais illustre le principe général : un espace de clés petit = chiffrement faible.

---

## 8. First Grep — General Skills

### Énoncé
> "Can you find the flag in the file? This would be really tedious to look through manually, something tells me there is a better way."  
> Un fichier volumineux est fourni.

### Approche
Le fichier contient des milliers de lignes. Le lire manuellement est impossible — on utilise `grep` pour chercher directement le pattern `picoCTF`.

### Solution
```bash
grep "picoCTF" file
```

### Flag
```
picoCTF{grep_is_good_to_find_things_29f42460}
```

### Leçon retenue
`grep` est l'un des outils les plus utilisés en cybersécurité. Il permet de chercher des patterns dans des fichiers de logs, de config, ou de code. Variantes essentielles :
```bash
grep -r "pattern" .      # recherche récursive
grep -i "pattern" file   # insensible à la casse
grep -n "pattern" file   # affiche les numéros de ligne
```

---

## 9. Strings It — General Skills

### Énoncé
Trouver le flag caché dans un fichier binaire.

### Approche
Un fichier binaire n'est pas lisible directement. La commande `strings` extrait toutes les chaînes de caractères lisibles (ASCII) contenues dans un binaire. En combinant avec `grep`, on filtre directement le flag.

### Solution
```bash
strings fichier | grep "picoCTF"
```

### Leçon retenue
Le combo `strings + grep` est un réflexe fondamental en **reverse engineering** et **forensics**. Avant d'analyser un binaire complexe, on commence toujours par `strings` pour voir ce qu'il contient en clair : URLs, mots de passe hardcodés, messages d'erreur, flags.

---

## 10. Plumbing — General Skills

### Énoncé
> "Sometimes you need to handle process data outside of a file. Can you find a way to keep the output of this program and search for the flag?"  
> Connexion : `fickle-tempest.picoctf.net 56934`

### Approche
Le serveur envoie un flux continu de fausses lignes pour noyer le flag :
```
This is definitely not a flag
Not a flag either
Again, I really don't think this is a flag
picoCTF{xxxxxxxxxxxxxxx}   ← flag noyé au milieu
...
```

On utilise **netcat** pour se connecter au serveur, et on **pipe** la sortie vers `grep` pour filtrer uniquement la ligne contenant le flag.

### Solution
```bash
nc fickle-tempest.picoctf.net 56934 | grep "picoCTF"
```

**Décomposition de la commande :**
- `nc` (netcat) → se connecte au serveur distant et reçoit le flux de données
- `|` (pipe) → redirige la sortie vers la commande suivante au lieu de l'afficher
- `grep "picoCTF"` → filtre et n'affiche que la ligne contenant le flag

### Leçon retenue
**Netcat** est le "couteau suisse du réseau" — il sert à se connecter à des services, écouter sur des ports, transférer des fichiers, et établir des shells distants (reverse shell, bind shell). Le **pipe** `|` est fondamental pour chaîner les outils en ligne de commande : c'est la philosophie Unix — des petits outils simples combinés pour accomplir des tâches complexes.

---

## Bilan Session 2

| # | Challenge | Catégorie | Technique | Statut |
|---|-----------|-----------|-----------|--------|
| 6 | 2Warm | General Skills | Conversion binaire | ✅ |
| 7 | Caesar | Cryptography | Force brute / César | ✅ |
| 8 | First Grep | General Skills | Commande grep | ✅ |
| 9 | Strings It | General Skills | strings + grep | ✅ |
| 10 | Plumbing | General Skills | netcat + pipe | ✅ |

**Compétences acquises :** conversions de bases, force brute sur César, grep, strings, netcat, pipes et redirections Linux.

**Prochaines étapes :** Base64, forensics (analyse de fichiers), Wireshark, introduction au Web Exploitation.

---

## Bilan Global (Sessions 1 & 2)

| Session | Challenges | Catégories couvertes |
|---------|------------|----------------------|
| Session 1 | 5 | General Skills, Cryptography (Vigenère, ROT13) |
| Session 2 | 5 | General Skills, Cryptography (César) |
| **Total** | **10** | **General Skills, Cryptography** |
