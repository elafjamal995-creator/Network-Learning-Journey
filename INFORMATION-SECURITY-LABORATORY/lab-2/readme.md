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









