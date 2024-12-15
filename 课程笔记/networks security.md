DES is comprised of three phases:

initial permutaion

16 rounds of processing

final permutation



DES uses two important components:

a key generator

an expansion function



the key generator generates an inital 64-bit key and 8-bits are removed from the inital key

then the  56-bit key is divided into two 28-bits keys. Bit shifting is performed on the left and right 28-bit keys to ensure that an unique subkey is generated during each processing round.

the expansion function receives a 48-bit subkey and XORs it with a plaintext block to produce a 48-bit block

then this 48-bit block is divided into 8 6-bit blocks and each 6-bit block is compressed into a 4-bit block using a substitution method

finally all the 8 4-bit blocks are concatenated into a 32-bit block





the following subkey is generated in round 3, 1101010

what is the subkey for round 4 (bit shifting)



given B1 = 110110 find C1



diffi helman exchange process



RSA

given p=3 q=5, calculate the public and private key pairs

s1: n = p x q = 3 x 5 = 15

s2:  $\phi(n) = (p-1) x (q-1) = 8$

s3:  choose public key e such that e is relatively prime to $\phi(n)$ and less than $\phi(n)$

s4: finding d such that de = 1 mod $\phi(n)$  and d less than 160

$\frac{1 + \phi(n) k}{e}$



加密与解密： $c = m^e mod \ \ n,   m = c^d mod \ \ n$





security management, firewall,  malware











