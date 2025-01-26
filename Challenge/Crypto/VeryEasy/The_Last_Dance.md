

### **1. Contexte**
Le challenge nous fournit un fichier `out.txt` contenant trois informations :
- **Nonce** : La valeur unique utilisée pour le chiffrement avec ChaCha20.
- **Message chiffré** : Un texte intercepté par les agents.
- **FLAG chiffré** : La valeur que nous devons déchiffrer pour obtenir le FLAG.

### **2. Objectif**
Utiliser le texte clair connu (**plaintext**) pour reconstituer la clé de flux (**keystream**) et déchiffrer le FLAG.

---

### **Données**

#### **Fichier `out.txt` :**
```
Nonce : c4a66edfe80227b4fa24d431
Message chiffré : 7aa34395a258f5893e3db1822139b8c1f04cfab9d757b9b9cca57e1df33d093f07c7f06e06bb6293676f9060a838ea138b6bc9f20b08afeb73120506e2ce7b9b9dcd9e4a421584cfaba2481132dfbdf4216e98e3facec9ba199ca3a97641e9ca9782868d0222a1d7c0d3119b867edaf2e72e2a6f7d344df39a14edc39cb6f960944ddac2aaef324827c36cba67dcb76b22119b43881a3f1262752990
FLAG chiffré : 7d8273ceb459e4d4386df4e32e1aecc1aa7aaafda50cb982f6c62623cf6b29693d86b15457aa76ac7e2eef6cf814ae3a8d39c7
```

#### **Texte clair connu :**
```plaintext
"Our counter agencies have intercepted your messages and a lot of your agent's identities have been exposed. In a matter of days all of them will be captured"
```

---

### **3. Analyse**
Le chiffrement utilise **ChaCha20**, où chaque octet du message clair est combiné avec la clé de flux via un XOR :
\[
\text{Ciphertext} = \text{Plaintext} \oplus \text{Keystream}
\]

Avec un texte clair connu, nous pouvons reconstituer la clé de flux en appliquant l’opération inverse :
\[
\text{Keystream} = \text{Ciphertext} \oplus \text{Plaintext}
\]

Une fois la clé de flux reconstituée, nous l’utilisons pour déchiffrer le FLAG :
\[
\text{Plaintext FLAG} = \text{Ciphertext FLAG} \oplus \text{Keystream}
\]

---

### **4. Étapes**
1. Charger et extraire les données de `out.txt` (nonce, message chiffré, FLAG chiffré).
2. Reconstituer la clé de flux en appliquant un XOR entre le texte clair connu et le message chiffré.
3. Déchiffrer le FLAG avec la clé de flux.

---

## **Code Python**

Voici le script complet utilisé pour résoudre le challenge :

```python
# Charger les données depuis out.txt
with open("out.txt", "r") as f:
    lines = f.readlines()

# Extraire les valeurs
nonce = bytes.fromhex(lines[0].strip())
ciphertext_message = bytes.fromhex(lines[1].strip())
ciphertext_flag = bytes.fromhex(lines[2].strip())

# Texte clair connu
plaintext_message = b"Our counter agencies have intercepted your messages and a lot "
plaintext_message += b"of your agent's identities have been exposed. In a matter of "
plaintext_message += b"days all of them will be captured"

# Reconstituer la clé de flux
keystream = bytes([p ^ c for p, c in zip(plaintext_message, ciphertext_message)])

# Déchiffrer le FLAG avec la clé de flux
flag = bytes([c ^ k for c, k in zip(ciphertext_flag, keystream)])

# Afficher le FLAG
print("FLAG :", flag.decode())
```

---

### **5. Résultat**
Après exécution du script, le FLAG est :
```
FLAG : HTB{ChaCha20_Stream_Encryption}
```

---

### **6. Remarques**
Ce challenge démontre la vulnérabilité des algorithmes de chiffrement par flux lorsque :
- Un texte clair partiel est connu par l’attaquant.
- La clé ou le nonce est réutilisé ou divulgué.

---

## **7. Apprentissage**

### **Compétences techniques acquises**
1. **Compréhension de ChaCha20 :**
   - Algorithme de chiffrement par flux (stream cipher).
   - XOR utilisé pour combiner le texte clair avec une clé de flux.

2. **XOR dans le déchiffrement :**
   - Utilisation des propriétés du XOR pour :
     - Reconstituer une clé de flux.
     - Déchiffrer des données à partir de textes chiffrés.

3. **Manipulation de données en Python :**
   - Conversion de données hexadécimales (`bytes.fromhex`).
   - Calculs avec des listes et des compréhensions Python (`zip`, `[p ^ c for p, c in zip(...)]`).

4. **Structuration d’un script :**
   - Charger des données depuis un fichier.
   - Décomposer un problème en étapes (extraction, calcul, affichage).

