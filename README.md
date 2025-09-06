# HILL CIPHER
HILL CIPHER
EX. NO: 3 AIM:
 

IMPLEMENTATION OF HILL CIPHER
 
## To write a python program to implement the hill cipher substitution techniques.

## DESCRIPTION:

Each letter is represented by a number modulo 26. Often the simple scheme A = 0, B
= 1... Z = 25, is used, but this is not an essential feature of the cipher. To encrypt a message, each block of n letters is  multiplied by an invertible n × n matrix, against modulus 26. To
decrypt the message, each block is multiplied by the inverse of the m trix used for
 
encryption. The matrix used
 
for encryption is the cipher key, and it sho
 
ld be chosen
 
randomly from the set of invertible n × n matrices (modulo 26).


## ALGORITHM:

STEP-1: Read the plain text and key from the user.
STEP-2: Split the plain text into groups of length three. 
STEP-3: Arrange the keyword in a 3*3 matrix.
STEP-4: Multiply the two matrices to obtain the cipher text of length three.
STEP-5: Combine all these groups to get the complete cipher text.

## PROGRAM 

```py
import math

ALPHABET_SIZE = 26
A_ORD = ord('A')

def _egcd(a, b):
    if b == 0:
        return (a, 1, 0)
    g, x1, y1 = _egcd(b, a % b)
    return (g, y1, x1 - (a // b) * y1)

def modinv(a, m):
    g, x, _ = _egcd(a % m, m)
    if g != 1:
        raise ValueError(f"No modular inverse for {a} mod {m}")
    return x % m

def _determinant(matrix):
    n = len(matrix)
    if n == 1: return matrix[0][0]
    if n == 2: return matrix[0][0]*matrix[1][1] - matrix[0][1]*matrix[1][0]
    det = 0
    for c in range(n):
        minor = [row[:c] + row[c+1:] for row in matrix[1:]]
        det += ((-1)**c) * matrix[0][c] * _determinant(minor)
    return det

def _matrix_of_minors(matrix):
    n = len(matrix)
    minors = [[0]*n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            minor = [row[:j] + row[j+1:] for idx,row in enumerate(matrix) if idx != i]
            minors[i][j] = _determinant(minor)
    return minors

def _cofactor_matrix(matrix):
    n = len(matrix)
    minors = _matrix_of_minors(matrix)
    return [[(((-1)**(i+j)) * minors[i][j]) for j in range(n)] for i in range(n)]

def _transpose(matrix):
    return [list(col) for col in zip(*matrix)]

def matrix_mod_inv(matrix, mod):
    det = _determinant(matrix) % mod
    det_inv = modinv(det, mod)
    cof = _cofactor_matrix(matrix)
    adj = _transpose(cof)
    return [[(det_inv * (adj[i][j] % mod)) % mod for j in range(len(matrix))] for i in range(len(matrix))]

def _chunkify(lst, size):
    chunks = []
    for i in range(0, len(lst), size):
        chunk = lst[i:i+size]
        if len(chunk) < size:
            chunk += [0] * (size - len(chunk))  # pad with 'A'
        chunks.append(chunk)
    return chunks

def _mat_mul_vec(matrix, vec, mod):
    return [sum(matrix[i][j]*vec[j] for j in range(len(vec))) % mod for i in range(len(matrix))]

def _text_to_nums(text):
    cleaned = ''.join([c for c in text.upper() if c.isalpha()])
    return [ord(c) - A_ORD for c in cleaned]

def _nums_to_text(nums):
    return ''.join(chr((n % ALPHABET_SIZE) + A_ORD) for n in nums)

def key_to_matrix(key):
    k = ''.join([c for c in key.upper() if c.isalpha()])
    length = len(k)
    n = int(math.isqrt(length))
    if n*n != length:
        raise ValueError("Key length must be a perfect square (e.g. 4, 9, 16).")
    nums = [ord(c) - A_ORD for c in k]
    return [nums[i*n:(i+1)*n] for i in range(n)]

def encrypt(plaintext, key):
    K = key_to_matrix(key)
    n = len(K)
    nums = _text_to_nums(plaintext)
    chunks = _chunkify(nums, n)
    cipher_nums = []
    for chunk in chunks:
        cipher_nums.extend(_mat_mul_vec(K, chunk, ALPHABET_SIZE))
    return _nums_to_text(cipher_nums)

def decrypt(ciphertext, key):
    K = key_to_matrix(key)
    K_inv = matrix_mod_inv(K, ALPHABET_SIZE)
    n = len(K)
    nums = _text_to_nums(ciphertext)
    chunks = _chunkify(nums, n)
    plain_nums = []
    for chunk in chunks:
        plain_nums.extend(_mat_mul_vec(K_inv, chunk, ALPHABET_SIZE))
    return _nums_to_text(plain_nums)

if __name__ == "__main__":
    key = input("Enter the key (length must be square, e.g. 4, 9, 16 letters): ")
    choice = input("Encrypt or Decrypt? (E/D): ").strip().upper()
    if choice == "E":
        pt = input("Enter plaintext: ")
        print("Ciphertext:", encrypt(pt, key))
    elif choice == "D":
        ct = input("Enter ciphertext: ")
        print("Plaintext:", decrypt(ct, key))
    else:
        print("Invalid choice!")


```

## OUTPUT

<img width="1919" height="969" alt="image" src="https://github.com/user-attachments/assets/d695cc0b-1806-4d78-b3bf-6a22bb3fd1ff" />

<img width="1919" height="972" alt="image" src="https://github.com/user-attachments/assets/93b39a44-1874-4ffc-ab6e-c7f7274b5fdc" />

<img width="1919" height="968" alt="image" src="https://github.com/user-attachments/assets/9a757344-6710-4394-8b37-8bd01823039e" />


## RESULT

Thus the python program to implement the hill cipher substitution techniques is completed and successfully executed
