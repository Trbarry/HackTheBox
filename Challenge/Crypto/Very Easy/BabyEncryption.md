

---

```markdown
# 🛠️ Baby Encryption - Write-Up

## 🎯 Challenge Overview
- **Challenge Name:** Baby Encryption  
- **Category:** Cryptography  
- **Difficulty:** Very Easy 
- **Objective:** Decrypt a message encrypted with a modular arithmetic formula.

---

## 🧩 Challenge Details

### 📜 Ciphertext
The encrypted message is provided in hexadecimal format:
```
6e0a9372ec49a3f6930ed8723f9df6f6720ed8d89dc4937222ec7214d89d1e0e352ce0aa6ec82bf622227bb70e7fb7352249b7d893c493d8539dec8fb7935d490e7f9d22ec89b7a322ec8fd80e7f8921
```

### 🔒 Encryption Formula
The encryption uses the following formula:
```math
char_cipher = (a × char_original + b) mod m
```
Where:
- **a = 123** (multiplicative constant)
- **b = 18** (additive constant)
- **m = 256** (modulo for 8-bit values)

---

## 🛠️ Decryption Process

### 🧮 Step 1: Decryption Formula
To reverse the encryption, use the formula:
```math
char_original = (a⁻¹ × (char_cipher - b)) mod m
```
Where:
- **a⁻¹** is the modular inverse of **a** modulo **256**.

### 🔎 Step 2: Find the Modular Inverse
The modular inverse of **123** modulo **256** is **179**:
```math
123 × 179 mod 256 = 1
```

### 🖥️ Step 3: Python Implementation
The following script decrypts the given ciphertext:
```python
# Ciphertext in hexadecimal
ciphertext = bytes.fromhex("6e0a9372ec49a3f6930ed8723f9df6f6720ed8d89dc4937222ec7214d89d1e0e352ce0aa6ec82bf622227bb70e7fb7352249b7d893c493d8539dec8fb7935d490e7f9d22ec89b7a322ec8fd80e7f8921")

# Function to decrypt a single character
def decrypt_char(char):
    temp = (char - 18) % 256
    char_original = (179 * temp) % 256
    return char_original

# Decrypting the ciphertext
decrypted = [decrypt_char(char) for char in ciphertext]

# Convert decrypted bytes to a string
plaintext = bytes(decrypted).decode('utf-8', errors='replace')
print("Decrypted Message:", plaintext)
```

---

## ✅ Results

### 📬 Decrypted Message

Voici le résultat final après déchiffrement :

![Decrypted Message](./Challenge/Crypto/Images/Very%20Easy/Images/image.png)


### 🔍 Observations
- The message is coherent and readable.
- The process confirms the modular arithmetic principles.

---

## 🧠 Lessons Learned

1. **Cryptography Basics**:
   - Understanding modular arithmetic and its role in encryption and decryption.
   - Importance of the modular inverse in reversing multiplicative transformations.

2. **Python Implementation**:
   - Using Python to handle hexadecimal data and perform modular operations.
   - Managing potential decoding errors with `errors='replace'`.

---

## 📂 Repository Structure

```
├── decrypt.py        # Python script for decryption
├── msg.enc           # (Optional) Encrypted message in binary or hex
└── README.md         # This write-up
```

---

## 🚀 How to Run

1. **Clone the repository**:
   ```bash
   git clone https://github.com/your-username/repo-name.git
   cd repo-name
   ```

2. **Run the decryption script**:
   ```bash
   python3 decrypt.py
   ```

3. **Output**:  
   The decrypted message will be printed to the console.

---

## 📖 References

- [Modular Arithmetic - Wikipedia](https://en.wikipedia.org/wiki/Modular_arithmetic)
- [Python Bytes Documentation](https://docs.python.org/3/library/stdtypes.html#bytes)
- [Cryptography Basics](https://crypto.stackexchange.com/)

---

### 🏆 Challenge Completed Successfully
```



