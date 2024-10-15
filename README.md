# p5-kriptografy

<hr>

Nama     : Muhamad David Ali

NIM      : 312210291

Kelas    : TI.22.A.1

<hr>

Latihan :
Lakukan enkripsi Playfair Cihper pada plaintext:

GOOD BROOM SWEEP CLEAN <hr>
REDWOOD NATIONAL STATE PARK <hr>
JUNK FOOD AND HEALTH PROBLEMS <hr>

Dengan kunci “TEKNIK INFORMATIKA”

Kumpulkan : Capture hasil Enkrip dan Dekripnya, beserta Link Githubnya![image](https://github.com/user-attachments/assets/1aa6cf0a-dd4b-4d1f-bb2e-a9937e7e9d68)

~~~
# Membuat Matriks 5x5 dari Kunci
def create_matrix(key):
    key = key.upper().replace('J', 'I')
    alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"
    matrix = []
    used = []
    
    for char in key:
        if char not in used:
            matrix.append(char)
            used.append(char)
    
    for char in alphabet:
        if char not in used:
            matrix.append(char)
    
    matrix_5x5 = [matrix[i:i+5] for i in range(0, 25, 5)]
    return matrix_5x5

# Mendapatkan posisi huruf dalam matriks
def get_position(char, matrix):
    for row in range(5):
        for col in range(5):
            if matrix[row][col] == char:
                return row, col
    return None

# Memisahkan Plaintext menjadi bigram dan menambahkan 'X' jika diperlukan
def prepare_text(text, is_decryption=False):
    text = text.upper().replace('J', 'I').replace(" ", "")
    bigrams = []
    i = 0
    while i < len(text):
        # Saat enkripsi: tambahkan 'X' di antara dua huruf yang sama atau jika teks ganjil
        if i + 1 < len(text) and text[i] != text[i + 1]:
            bigrams.append(text[i] + text[i + 1])
            i += 2
        else:
            if is_decryption:
                bigrams.append(text[i:i + 2])  # Dekripsi tidak menambahkan 'X'
            else:
                bigrams.append(text[i] + 'X')
            i += 1
    # Jika panjang teks ganjil dan enkripsi, tambahkan 'X' di akhir
    if len(bigrams[-1]) == 1 and not is_decryption:
        bigrams[-1] += 'X'
    return bigrams

# Fungsi Enkripsi
def encrypt(plaintext, matrix):
    plaintext_pairs = prepare_text(plaintext)
    ciphertext = ''
    
    for pair in plaintext_pairs:
        row1, col1 = get_position(pair[0], matrix)
        row2, col2 = get_position(pair[1], matrix)
        
        # Jika kedua huruf ada di baris yang sama
        if row1 == row2:
            ciphertext += matrix[row1][(col1 + 1) % 5]
            ciphertext += matrix[row2][(col2 + 1) % 5]
        # Jika kedua huruf ada di kolom yang sama
        elif col1 == col2:
            ciphertext += matrix[(row1 + 1) % 5][col1]
            ciphertext += matrix[(row2 + 1) % 5][col2]
        # Huruf tidak di baris atau kolom yang sama
        else:
            ciphertext += matrix[row1][col2]
            ciphertext += matrix[row2][col1]
    
    return ciphertext

# Fungsi Dekripsi
def decrypt(ciphertext, matrix):
    ciphertext_pairs = prepare_text(ciphertext, is_decryption=True)
    plaintext = ''
    
    for pair in ciphertext_pairs:
        row1, col1 = get_position(pair[0], matrix)
        row2, col2 = get_position(pair[1], matrix)
        
        # Jika kedua huruf ada di baris yang sama
        if row1 == row2:
            plaintext += matrix[row1][(col1 - 1) % 5]
            plaintext += matrix[row2][(col2 - 1) % 5]
        # Jika kedua huruf ada di kolom yang sama
        elif col1 == col2:
            plaintext += matrix[(row1 - 1) % 5][col1]
            plaintext += matrix[(row2 - 1) % 5][col2]
        # Huruf tidak di baris atau kolom yang sama
        else:
            plaintext += matrix[row1][col2]
            plaintext += matrix[row2][col1]
    
    # Menghapus huruf 'X' yang ditambahkan sebagai padding, kecuali jika itu bagian dari teks asli
    cleaned_plaintext = ""
    i = 0
    while i < len(plaintext):
        # Jika 'X' adalah padding yang ditambahkan di antara huruf yang sama, kita abaikan
        if i + 1 < len(plaintext) and plaintext[i + 1] == 'X' and i + 2 < len(plaintext) and plaintext[i] == plaintext[i + 2]:
            cleaned_plaintext += plaintext[i]
            i += 2  # Skip the 'X'
        else:
            cleaned_plaintext += plaintext[i]
            i += 1
    return cleaned_plaintext.rstrip('X')  # Menghapus padding 'X' di akhir teks

# Main program with loop for multiple tests
while True:
    print("\n=== Playfair Cipher ===")
    
    # Input dari pengguna
    key = input("Masukkan kunci (tanpa spasi, huruf non-duplikat): ")
    matrix = create_matrix(key)

    plaintext = input("Masukkan plaintext yang ingin dienkripsi: ")
    
    # Enkripsi dan Dekripsi
    ciphertext = encrypt(plaintext, matrix)
    print(f"Ciphertext: {ciphertext}")

    decrypted_text = decrypt(ciphertext, matrix)
    print(f"Decrypted Text: {decrypted_text}")

    # Tanya apakah ingin mencoba lagi
    repeat = input("\nApakah Anda ingin mengulangi (Y/N)? ").upper()
    if repeat != 'Y':
        print("Program selesai.")
        break
~~~

![image](https://github.com/user-attachments/assets/20572e56-f893-43d4-a387-f3ac56ab6951)
