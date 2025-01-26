```markdown
# The Last Dance - Write-Up

## Challenge Overview
- **Challenge Name:** The Last Dance  
- **Category:** Cryptography  
- **Difficulty:** Medium  
- **Objective:** Decrypt a flag encrypted using the ChaCha20 stream cipher.

---

## Challenge Details

### Ciphertext
The encrypted data is provided in a file `out.txt` containing three elements:
1. **Nonce**: A unique value used during encryption.
2. **Encrypted Message**: A ciphertext corresponding to a known plaintext message.
3. **Encrypted Flag**: The ciphertext for the hidden flag.

Contents of `out.txt`:
```
Nonce: c4a66edfe80227b4fa24d431
Encrypted Message: 7aa34395a258f5893e3db1822139b8c1f04cfab9d757b9b9cca57e1df33d093f07c7f06e06bb6293676f9060a838ea138b6bc9f20b08afeb73120506e2ce7b9b9dcd9e4a421584cfaba2481132dfbdf4216e98e3facec9ba199ca3a97641e9ca9782868d0222a1d7c0d3119b867edaf2e72e2a6f7d344df39a14edc39cb6f960944ddac2aaef324827c36cba67dcb76b22119b43881a3f1262752990
Encrypted Flag: 7d8273ceb459e4d4386df4e32e1aecc1aa7aaafda50cb982f6c62623cf6b29693d86b15457aa76ac7e2eef6cf814ae3a8d39c7
```

### Encryption Method
The ChaCha20 stream cipher is used, where:
```plaintext
Ciphertext = Plaintext XOR Keystream
```
To decrypt:
```plaintext
Plaintext = Ciphertext XOR Keystream
```

---

## Decryption Process

### Step 1: Reconstitute the Keystream
Using the known plaintext and its corresponding ciphertext, the keystream can be calculated:
```plaintext
Keystream = Plaintext XOR Ciphertext
```

### Step 2: Decrypt the Flag
With the keystream reconstituted, decrypt the flag by applying the reverse operation:
```plaintext
Plaintext Flag = Encrypted Flag XOR Keystream
```

---

## Python Implementation

The following Python script performs the decryption:

```python
# Load data from out.txt
with open("out.txt", "r") as f:
    lines = f.readlines()

# Extract values
nonce = bytes.fromhex(lines[0].strip())
ciphertext_message = bytes.fromhex(lines[1].strip())
ciphertext_flag = bytes.fromhex(lines[2].strip())

# Known plaintext message
plaintext_message = b"Our counter agencies have intercepted your messages and a lot "
plaintext_message += b"of your agent's identities have been exposed. In a matter of "
plaintext_message += b"days all of them will be captured"

# Reconstitute the keystream
keystream = bytes([p ^ c for p, c in zip(plaintext_message, ciphertext_message)])

# Decrypt the flag
flag = bytes([c ^ k for c, k in zip(ciphertext_flag, keystream)])

# Print the flag
print("FLAG:", flag.decode())
```

---

## Results

### Decrypted Flag
After running the script, the decrypted flag is:
```
FLAG: HTB{ChaCha20_Stream_Encryption}
```

---

## Lessons Learned

1. **Understanding Stream Ciphers**:
   - Learned how stream ciphers like ChaCha20 encrypt data by XORing plaintext with a generated keystream.
   - Explored vulnerabilities when plaintext data is partially known.

2. **XOR Operations**:
   - Reinforced the use of XOR for encryption and decryption.
   - Practiced Python operations for calculating XOR between byte sequences.

3. **Python Skills**:
   - Used Python's `bytes.fromhex()` to convert hexadecimal strings.
   - Applied `zip()` and list comprehensions for pairwise operations on sequences.

---



## References

- [ChaCha20 - Wikipedia](https://en.wikipedia.org/wiki/Salsa20#ChaCha_variant)
- [Python Bytes Documentation](https://docs.python.org/3/library/stdtypes.html#bytes)
- [Cryptography Basics](https://crypto.stackexchange.com/)

---

### Challenge Completed Successfully
```
