# StegoRSA
| **Platform** | picoCTF 2026 |
| **Category** | Cryptography |
| **Difficulty** | Easy |
| **Author** | Yahaya Meddy |
| **Topic** | RSA / Metadata / Private Key Recovery |

## Challenge

A message has been encrypted using RSA. The public key is gone, but someone may have been careless with the private key.

The challenge provides an encrypted flag and an image.

Hints:

- Metadata can tell you more than you expect.
- Hex can be turned back into a key file.

## Step 1 – Download the Files

Download the provided flag and image files.

Check the files:

ls -la

## Step 2 – Inspect the Image Metadata

Use ExifTool to inspect the image:

exiftool image

Look through the metadata for suspicious hexadecimal data.

The hexadecimal data contains the RSA private key.

## Step 3 – Convert Hex to a Key File

Convert the hexadecimal data back into binary:

echo '<hex-data>' | xxd -r -p > private.key

Check the recovered file:

file private.key

## Step 4 – Decrypt the Flag

Use the recovered private key with OpenSSL:

openssl pkeyutl -decrypt -inkey private.key -in flag -out decrypted_flag.txt

If the encrypted flag has a different filename, replace flag with the correct filename.

## Step 5 – Read the Flag

cat decrypted_flag.txt

Output:

picoCTF{rs4_k3y_1n_1mg_4eedd678}

## Why This Works

The private RSA key was accidentally exposed through the image metadata.

The solution path is:

Image
↓
exiftool
↓
Find hexadecimal key data
↓
Convert hex with xxd
↓
Recover private.key
↓
Decrypt with OpenSSL
↓
Read decrypted_flag.txt
↓
Flag

## Commands Used

exiftool image
echo '<hex-data>' | xxd -r -p > private.key
file private.key
openssl pkeyutl -decrypt -inkey private.key -in flag -out decrypted_flag.txt
cat decrypted_flag.txt

## One-Line Solution

openssl pkeyutl -decrypt -inkey private.key -in flag -out decrypted_flag.txt && cat decrypted_flag.txt

## Complete Solution

Download the image and encrypted flag
↓
exiftool image
↓
Find the hexadecimal private-key data
↓
echo '<hex-data>' | xxd -r -p > private.key
↓
file private.key
↓
openssl pkeyutl -decrypt -inkey private.key -in flag -out decrypted_flag.txt
↓
cat decrypted_flag.txt
↓
picoCTF{rs4_k3y_1n_1mg_4eedd678}

## Flag

picoCTF{rs4_k3y_1n_1mg_4eedd678}

## Tools Used

- Linux Terminal
- ExifTool
- xxd
- file
- OpenSSL
- RSA

## Key Learning

- Image metadata can contain hidden sensitive information.
- ExifTool is useful for forensic metadata analysis.
- Hexadecimal data can be converted back into binary using xxd.
- RSA private keys must be kept secret.
- OpenSSL can decrypt data using a recovered RSA private key.

## Final Solution

exiftool image
↓
Find hexadecimal private-key data
↓
echo '<hex-data>' | xxd -r -p > private.key
↓
openssl pkeyutl -decrypt -inkey private.key -in flag -out decrypted_flag.txt
↓
cat decrypted_flag.txt
↓
picoCTF{rs4_k3y_1n_1mg_4eedd678}
