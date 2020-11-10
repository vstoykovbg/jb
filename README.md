# How Jason Bourne Stores His Bitcoin in 2020

WarpWalelt is so 2014. Now, Jason Bourne is using Doubleslow and Steghide.

Shortcomings of WarpWallet:
- JavaScript in the browser is inefficient compared with Python
- It requires too little CPU/RAM resources (about 2 seconds when implemented in Python on i3-2100)
- The result is uncompressed Bitcoin address (it would be better to get BIP39 seed at the output instead, or at least compressed address)

The storage system should should:

(1) Prevent others from stealing your coin
(2) Avoid accidental loss
(3) Be low-cost
(4) Be globally accessible

## Obscurity with steganography

Using programs like Steghide is optional.

First Jason hides the key stretching parameters and multiple seeds (to be used in the future to derive different keys/seeds) with a program like Steghide in a randomly chosen image (img1.jpg).

He uploads that image along with other images to his cloud hosting account and other online systems (email account) just to be sure that the data is stored safely and globally accessible (4).

The key stretching scripts can be stored separately (not inside img1.jpg) to minimize the changes to the image (it can be publicly accessible in Github).

Instead of using Steghide (and remembering another passphrase - the one Steghide uses to encrypt the secrets inside the image) a simple checksum of the image can be used (converted to BIP39, RFC1751, or another format with mnemonic-sha256.py or another script from mnemonic-hashes collection).

One salt/seed can produce multiple wallets (when using different passphrases and key stretching settings).

Jason uses obscurity only as an additional layer, not as a primary method of achieving security (security through obscurity is a bad practice when obscurity is used alone).

## Seed/salt generation

Jason uses secure randomness mixers (Doublerandom) to create a truly random seed/salt and passphrases.

## Strong key stretching

The most important element of the system is the key stretching. It must be strong so Jason needs to remember a shorter passphrase (to achieve the same level of security). Jason does not want to risk losing his Bitcoin (or keys for his VeraCrypt container) in case another brain trauma occurs (it would be less likely to forget the shorter passphrase).

Also, Jason may use Shamir's secret-sharing scheme to give keys to his trusted people so in case he forgets the password he can ask them "Did I gave you some codes for keeping?" (in case he remember who they are).

Jason is choosing a large number of iterations that take half an hour or even several hours on his slightly old computer he uses as an air-gapped computer dedicated to cryptographic stuff. He runs the OS ([Cryptopup](https://github.com/vstoykovbg/cryptopup/)) from a DVD (or a CD) inserted in a read-only optical device to reduce the risk of hypothetical malware writing the secrets on the optical disk. For the same reason, the computer does not have non-volatile memory devices (hard drive, USB flash drive).

## Air-gapped computer

Unfortunately, it's not possible for a computer to not have non-volatile memory at all (inside the CPU, on the "BIOS/UEFI" memory). But removing some of the devices is protecting at least against the non-sophisticated malware relying on an easily accessible memory (HDD, optical disks in a read-write optical device, USB flash drives).

Jason does not connect the air-gapped computer to other computers after he uses it for cryptographic stuff (in case there is hidden malware inside the BIOS/UEFI, the optical drive's firmware, the CPU).

Jason is keeping his computer in a Faraday cage to reduce the risk of hypothetical malware transmitting data via modulating the signal on the USB cables.

The computer is kept in a room without windows because the hypothetical malware can modulate the light emitted by the HDD LEDs. Or HDD LEDs are disconnected.

The PC speaker is disconnected to reduce the risk of hypothetical malware transmitting secrets via ultrasound.

## Virtual keyboard

Jason is using a virtual keyboard when typing passwords to reduce the risk of a hardware keylogger embedded on his keyboard or in the BIOS/UEFI level.

## More obscurity

It's possible to feed the output from Doubleslow to another stage of Doubleslow with the same (or other) key stretching settings and another passphrase. For the next Doubleslow stage Jason may use a mnemonic checksum as a passphrase (generated with a script from the mnemonic-hashes collection from another file - img2.jpg).

But adding more obscurity increases the risk to forget how to repeat the whole procedure.

The salt for the Doubleslow can be in different formats, not only formats protected with a checksum:

- RFC1751 mnemonic
- BIP39 mnemonic
- Decimal (big decimal integer)
- Base58 with a checksum
- Hexadecimal
- Base64
- Unicode string (normalized with NFKC by the script)

![Parsing the salt](https://raw.githubusercontent.com/vstoykovbg/cryptopup/main/images/horse_1.png)

First, the script tries to parse the salt in the checksum protected formats or as a decimal integer. If no success Doubleslow tries the other formats with priority to parse it as the smallest salt. If the string can be parsed as hexadecimal and base64 the hexadecimal have a priority because the resulting salt is the smallest). If no other format matches Doubleslow is parsing the salt as a Unicode string and then normalizes it with NFKC.

Doubleslow computes the mnemonic hash of the salt, so it's safer to remember it (at least the first two of the words) or write it down, especially when you decide to use a salt in a format not protected with a checksum (arbitrary Unicode string, a decimal integer, base64, hexadecimal). If you notice the checksum does not match - this means you did not enter the salt correctly.

You may use a checksum for the password. However, it is not implemented in Doubleslow because it's a security risk: the attacker can use the checksum to brute-force your password easily (the user may be tempted to write the checksum somewhere - what could go wrong?). It's safe to write (store in your backup) a checksum of your salt, but not the checksum of your password.

Alternatively, you may remember the first two words from the output of Doubleslow. If the words you remember are different, this means you wrote the passphrase wrong, or something other happened (cosmic rays caused bit flip, wrong salt, wrong settings).

![Doubleslow output](https://raw.githubusercontent.com/vstoykovbg/cryptopup/main/images/horse_2.png)

For better security, Doubleslow should be modified not to print the derived key (or seed) on the screen. Instead, it would be better to import the seed directly to a wallet like Electrum (by creating a wallet file). Or shield the monitor against surveillance (optical and radio).


