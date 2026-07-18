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







