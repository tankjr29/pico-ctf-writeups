# Writeups PicoCTF — Session 4
**Auteur : AGBENONZAN Kossivi Jacques Junior — Étudiant L3 RIST**
**Date :** 30 avril 2026  
**Catégories :** Web Exploitation  
**Challenges résolus :** 5

---

## 16. Insp3ct0r — Web Exploitation

### Énoncé
> "Kishor Balan nous a informés que le code suivant pourrait nécessiter une inspection."  
> URL fournie : `http://fickle-tempest.picoctf.net:53989`

### Approche
Challenge d'inspection du code source web. Le flag est splitté en 3 parties cachées dans les commentaires du HTML, CSS et JavaScript.

### Solution
```
1. Ctrl+U → View Page Source → commentaire HTML → partie 1
2. Ouvrir /style.css → commentaire CSS → partie 2
3. Ouvrir /script.js → commentaire JS → partie 3
```

### Flag
```
picoCTF{tru3_d3t3ct1ve_0r_ju5t_lucky?302945a7}
```

### Leçon retenue
**Toujours inspecter le code source** avant toute autre chose en web exploitation. Les développeurs laissent souvent des informations sensibles dans les commentaires : clés API, mots de passe, endpoints cachés, tokens d'accès. C'est le premier réflexe du pentesteur web.

---

## 17. Where are the robots — Web Exploitation

### Énoncé
> "Can you find the robots?"  
> URL fournie : `http://fickle-tempest.picoctf.net:54677`

### Approche
Le fichier `robots.txt` est utilisé par les sites pour indiquer aux moteurs de recherche les pages à ne pas indexer. En pentest, c'est une mine d'informations sur la structure interne du site.

### Solution
```
1. Ouvrir http://fickle-tempest.picoctf.net:54677/robots.txt
2. Résultat :
   User-agent: *
   Disallow: /cc6b1.html
3. Ouvrir la page cachée : /cc6b1.html → flag
```

### Flag
```
picoCTF{ca1cu1at1ng_Mach1n3s_cc6b1}
```

### Leçon retenue
`robots.txt` est l'un des premiers fichiers à vérifier lors d'une reconnaissance web. Il révèle souvent des pages d'administration, des backups, des endpoints sensibles. **Règle d'or :** `robots.txt` n'est pas une mesure de sécurité — un attaquant l'ignore complètement et l'utilise comme carte des zones sensibles.

---

## 18. Logon — Web Exploitation

### Énoncé
> "L'usine cache des choses à tous ses utilisateurs. Pouvez-vous vous connecter en tant que Joe et trouver ce qu'ils ont regardé ?"  
> URL fournie : `http://fickle-tempest.picoctf.net:60237`

### Approche
Challenge de **manipulation de cookies**. Après connexion, le serveur stocke un cookie `admin=false` dans le navigateur. Le serveur fait confiance à ce cookie sans vérification côté serveur.

### Solution
```
1. Se connecter avec n'importe quel identifiant
2. F12 → Application → Cookies
3. Modifier le cookie : admin=false → admin=true
4. Recharger la page → flag affiché
```

### Flag
```
picoCTF{th3_c0oki3_i5_@_l1e_3c22efa3}
```

### Leçon retenue
**Ne jamais faire confiance aux données côté client.** Les cookies, formulaires et headers peuvent tous être modifiés par l'utilisateur. Un serveur sécurisé vérifie toujours les permissions côté serveur, jamais uniquement côté client. Cette vulnérabilité permet en pentest réel d'accéder à des panneaux d'administration et des données privées.

---

## 19. Picobrowser — Web Exploitation

### Énoncé
> "This website can be rendered only by picobrowser, go and catch the flag!"  
> URL fournie : `http://fickle-tempest.picoctf.net:64797`

### Approche
Le site vérifie le **User-Agent** du navigateur — une chaîne d'identification envoyée à chaque requête HTTP. Il faut falsifier ce User-Agent pour se faire passer pour "picobrowser".

### Solution
```bash
# Falsifier le User-Agent avec curl
curl -A "picobrowser" http://fickle-tempest.picoctf.net:64797

# Le HTML retourné révèle un endpoint /flag
curl -A "picobrowser" http://fickle-tempest.picoctf.net:64797/flag
```

### Flag
```
picoCTF{p1c0_s3cr3t_ag3nt_fba5c48f}
```

### Leçon retenue
Le **User-Agent** est facilement falsifiable — on ne peut jamais l'utiliser comme mécanisme de sécurité. En pentest, changer le User-Agent permet de contourner des restrictions, se faire passer pour un bot, un mobile, ou un crawler spécifique. `curl -A` est l'outil idéal pour ce type de test.

---

## 20. GET aHEAD — Web Exploitation

### Énoncé
> "Find the flag being held on this server to get ahead of the competition."  
> URL fournie : `http://wily-courier.picoctf.net:52909/`

### Approche
Le titre est un indice : la méthode HTTP **HEAD**. En HTTP, plusieurs méthodes existent :
- `GET` → récupère le contenu d'une page
- `POST` → envoie des données
- `HEAD` → retourne uniquement les **headers** de réponse, sans le contenu

Le flag est caché dans les headers HTTP de la réponse.

### Solution
```bash
# Envoyer une requête HEAD avec curl
curl -I http://wily-courier.picoctf.net:52909/
```

Le flag apparaît dans les headers de réponse du serveur.

### Flag
```
picoCTF{r3j3ct_th3_du4l1ty_8b13f07}
```

### Leçon retenue
Les **headers HTTP** sont une source d'information précieuse en pentest : version du serveur, technologies utilisées, configurations de sécurité (ou leur absence), et parfois des données sensibles. `curl -I` et `curl -v` sont des outils essentiels pour inspecter les headers. En reconnaissance web, on analyse toujours les headers avant d'attaquer.

---

## Bilan Session 4

| # | Challenge | Catégorie | Technique | Statut |
|---|-----------|-----------|-----------|--------|
| 16 | Insp3ct0r | Web Exploitation | Inspection code source | ✅ |
| 17 | Where are the robots | Web Exploitation | robots.txt | ✅ |
| 18 | Logon | Web Exploitation | Manipulation de cookies | ✅ |
| 19 | Picobrowser | Web Exploitation | Falsification User-Agent | ✅ |
| 20 | GET aHEAD | Web Exploitation | Méthodes HTTP / Headers | ✅ |

**Compétences acquises :** inspection code source, robots.txt, manipulation de cookies, User-Agent spoofing, méthodes HTTP, headers HTTP.

---

## Bilan Global (Sessions 1 à 4)

| Session | Challenges | Catégories |
|---------|------------|------------|
| Session 1 | 5 | General Skills, Cryptography |
| Session 2 | 5 | General Skills, Cryptography |
| Session 3 | 5 | General Skills |
| Session 4 | 5 | Web Exploitation |
| **Total** | **20** | **General Skills, Cryptography, Web Exploitation** |

**Prochaines étapes recommandées :** Forensics (Wireshark, analyse d'images, métadonnées), puis TryHackMe Jr Penetration Tester pour consolider tout ça en scénarios réels.
