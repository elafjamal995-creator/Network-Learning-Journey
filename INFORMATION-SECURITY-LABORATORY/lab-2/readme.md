# 2 Lab Environment
cd ~/Desktop
cd Labsetup
$docker-composebuild           #Buildthecontainerimage
$docker-composeup           #Startthecontainer
$docker-composedown        #Shutdownthecontainer
$dockps
$docksh <id>

* Note:If a docker command requires a container ID ,you do not need to type the entire ID string .Typing the first few characters will be sufficient ,as long as they are unique among all the containers.


# 3 Task1:Frequency Analysis

cd ~/Desktop/Labsetup/Files
./freq.py
tr 'vgapnbrtmwsicuxeohqyzflkdj' 'abcdefghijklmnopqrstuvwxyz' < ciphertext.txt > output.txt
cat output.txt

# Task2:  Encryption using Different Ciphers and Modes
cd ..
mkdir Task2
cd Task2
echo "Hello, this is my first encrypted file" > plain.txt
## AES-CBC
openssl enc -aes-128-cbc -e -in plain.txt -out cipher1.bin -K 00112233445566778889aabbccddeeff -iv 0102030405060708
cat cipher1.bin
## Blowfish
openssl enc -bf-cbc -e -in plain.txt -out cipher2.bin -K 00112233445566778889aabbccddeeff -iv 0102030405060708
cat cipher2.bin
## AES-CFB
openssl enc -aes-128-cfb -e -in plain.txt -out cipher3.bin -K 00112233445566778889aabbccddeeff -iv 0102030405060708
cat cipher3.bin


### Decryption
openssl enc -aes-128-cbc -d -in cipher1.bin -out decrypted1.txt -K 00112233445566778889aabbccddeeff -iv 0102030405060708

# Lab Commands - Task 3: Encryption Mode (ECB vs CBC)
seed@VM:~/.../Labsetup$ cd Files
## Mode– ECB
seed@VM:~/.../Files$ openssl enc -aes-128-ecb -e -in pic_original.bmp -out encrypt_ecb.bmp -K 00112233445566778889aabbccddeeff
## Mode– CBC
openssl enc -aes-128-cbc -e -in pic_original.bmp -out encrypt_cbc.bmp -K 00112233445566778889aabbccddeeff -iv 0102030405060708

### 1. Header and Body Extraction (ECB Mode)
```bash
head -c 54 pic_original.bmp > header
tail -c +55 encrypt_ecb.bmp > body
cat header body > new.bmp
```
### 2. Header and Body Extraction (CBC Mode)
```text
head -c 54 pic_original.bmp > header_cbc
tail -c +55 encrypt_cbc.bmp > body_cbc
cat header_cbc body_cbc > new_cbc.bmp
```
### 3. Displaying Results
```bash
eog new.bmp
```
![test](test-1.png)
```bash
eog new_cbc.bmp
```
![test](test-2.png)

# SEED Labs: Task 3 Encryption Report

This task explores how different encryption modes affect data security. We compare ECB (Electronic Code Book) and CBC (Cipher Block Chaining) by encrypting an image file and analyzing the visual output.

---

## 1. Why do we perform this experiment?
When we encrypt a file, the output becomes binary data that most programs cannot read. However, image files (specifically .bmp) contain a 54-byte "Header" at the beginning of the file that stores information like image dimensions and color depth. If we encrypt this header along with the image data, the computer will not recognize it as an image anymore.

To see the effect of encryption, we perform a "Header Replacement" technique:
1. We keep the original, unencrypted 54-byte Header.
2. We encrypt the rest of the image (the Body).
3. We join the original Header with the encrypted Body.

This allows us to open the encrypted file in a standard image viewer to see how the encryption mode handled the original image patterns.

---

## 2. Encryption Modes Explained

### Electronic Code Book (ECB)
In ECB mode, the encryption algorithm processes each block of data independently. Because the algorithm is deterministic, identical blocks of input data produce identical blocks of encrypted output. In an image, areas with the same color or pattern result in identical encrypted patterns, which allows the original shape of the picture to remain visible after encryption. This is considered insecure for data with repeating patterns.

### Cipher Block Chaining (CBC)
In CBC mode, each block of data is XORed with the previous encrypted block before being encrypted itself. This creates a dependency between blocks. As a result, even if the original image has repeating patterns, the encrypted output will be completely randomized. This effectively hides the structure of the original data, making it a much more secure choice.

---

## 3. Our Observations
By viewing the resulting files:
* **ECB Results:** The image is scrambled but still shows clear outlines and patterns of the original picture. This confirms that ECB does not sufficiently hide data structure.
* **CBC Results:** The image appears as random, incoherent visual noise. This confirms that CBC successfully hides the data patterns and is significantly more secure.

---

## 4. Summary of Methodology
The experiment proves that the choice of encryption mode is as critical as the encryption algorithm itself. While AES is a strong algorithm, using it with the wrong mode (like ECB) can lead to information leakage in structured data such as images. By isolating the file header from the encrypted body, we can visualize these security properties and verify that CBC provides the expected protection against pattern analysis.


# SEED Labs: Task 4 - Padding Analysis & Experimental Steps

This report documents the step-by-step experiment performed to analyze padding in block ciphers, reflecting the actual commands executed in the lab environment.

---

## 1. Experimental Setup
We started by creating a dedicated workspace on the Desktop and generating a test file to be encrypted.

```bash
# Navigate to Desktop and create a folder
cd Desktop
mkdir Task3
cd Task3

# Create a 4-byte test file (using -n to prevent newline characters)
echo -n '1234' > file.txt
```
## 2. Pre-Encryption Verification
Before encrypting, we check the exact size of our input file to establish a baseline for comparison.
```text
# Verify the file size
1234[07/18/26]seed@VM:~/.../Task3$ wc -c file.txt
4 file.txt
```
## 3. Encryption Process
We encrypted the file using the AES-128-CBC algorithm. Note that because our file is smaller than the 16-byte block size, the system automatically applies padding.
```text
# Encrypt the file
openssl enc -aes-128-cbc -e -in file.txt -out cipher1.bin -K 00112233445566778889aabbccddeeff -iv 0102030405060708
```
* Note: If you view the encrypted file content using cat cipher1.bin, you will see unreadable, scrambled binary data, confirming the encryption was successful.
```text
[07/18/26]seed@VM:~/.../Task3$ cat cipher1.bin
�Z�����K�
```
## 4. Decryption & Padding Analysis
To see the padding added by the encryption process, we perform decryption while disabling the automatic removal of padding using the `-nopad` flag.
```text
# Decrypt without removing padding
openssl enc -aes-128-cbc -d -in cipher1.bin -out decrypted1_file.txt -nopad

# Check the size of the decrypted file (should be 16 bytes due to padding)

[07/18/26]seed@VM:~/.../Task3$ wc -c decrypted1_file.txt
16 decrypted1_file.txt

```
| Mode | Input Size (bytes) | Output Size (bytes) | Padding Applied? |
| :--- | :--- | :--- | :--- |
| **ECB** | 4 | 16 | Yes |
| **CBC** | 4 | 16 | Yes |
| **CFB** | 4 | 4 | No |
| **OFB** | 4 | 4 | No |

## 5. Key Findings
* Block Ciphers (ECB/CBC): These modes require data to be a multiple of the block size (16 bytes). Therefore, they apply padding automatically.

* Stream Ciphers (CFB/OFB): These modes encrypt data byte-by-byte, making padding unnecessary, which is why the output size matches the input size exactly.

# Tool Documentation: Analyzing Data with hexdump

In this lab, we use `hexdump -C` to inspect the internal structure of our files. Since padding data often contains non-printable characters, standard text editors cannot display them correctly. This tool serves as our "microscope."

---

# Tool Documentation: Analyzing Data with hexdump and xxd

In this lab, we use `hexdump -C` or `xxd` to inspect the internal structure of our files. Since padding data often contains non-printable characters, standard text editors cannot display them correctly. These tools serve as our "microscopes."

---

## 1. Available Tools
*   **hexdump -C:** A powerful utility that displays file content in a structured hexadecimal format, with a side-by-side ASCII view.
*   **xxd:** A highly popular alternative that performs the same function, converting any file into a readable hexadecimal representation[cite: 1].

## 2. Why do we use them?
*   **Revealing Hidden Data:** Encryption padding is often hidden from normal view. These tools allow us to see specific bytes, which are crucial for our analysis[cite: 1].
*   **Verification:** They provide objective proof that padding was added or removed during the encryption/decryption process[cite: 1].

## 3. How to use them
You can use either command to inspect your decrypted files:

```bash
# Option 1: Using hexdump
hexdump -C decrypted_file.txt

# Option 2: Using xxd
xxd decrypted_file.txt
```























