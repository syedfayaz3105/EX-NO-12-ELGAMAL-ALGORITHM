## EX-NO:12 ELGAMAL-ALGORITHM
## AIM:
To encrypt and decrypt a message using the ElGamal encryption algorithm.
## ALGORITHM:
•	Key Generation: Bob computes his public key y using p, g, and his private key x.
•	Encryption: Alice encrypts the message M using p, g, y, and her random key k to produce ciphertext (c1, c2).
•	Decryption: Bob decrypts the message using his private key x to recover M.
This algorithm provides secure communication between Alice and Bob using ElGamal encryption.
## PROGRAM:
```
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

int main() {
    int p, g, bob_private_key, message, k;

    printf("Enter a prime number (p): ");
    scanf("%d", &p);
    
    printf("Enter a primitive root modulo p (g): ");
    scanf("%d", &g);

    printf("Bob's private key (x): ");
    scanf("%d", &bob_private_key);

    int bob_public_key = 1;
    int base = g;
    int exp = bob_private_key;
    int mod = p;
    
    while (exp > 0) {
        if (exp % 2 == 1) {
            bob_public_key = (bob_public_key * base) % mod;
        }
        base = (base * base) % mod;
        exp = exp / 2;
    }
    
    printf("Bob's public key (y): %d\n", bob_public_key);

    printf("Alice, enter the message (M) you want to send: ");
    scanf("%d", &message);

    printf("Alice's private key (k): ");
    scanf("%d", &k);

    int c1 = 1;  // c1 = g^k % p
    int c2 = 0;  // c2 = M * y^k % p

    // Compute c1 = g^k % p
    base = g;
    exp = k;
    mod = p;
    
    while (exp > 0) {
        if (exp % 2 == 1) {
            c1 = (c1 * base) % mod;
        }
        base = (base * base) % mod;
        exp = exp / 2;
    }

    int yk = 1;
    base = bob_public_key;
    exp = k;
    mod = p;
    
    while (exp > 0) {
        if (exp % 2 == 1) {
            yk = (yk * base) % mod;
        }
        base = (base * base) % mod;
        exp = exp / 2;
    }

    // Compute c2 = M * y^k % p
    c2 = (message * yk) % p;
    
    printf("Alice sends encrypted message (c1, c2): (%d, %d)\n", c1, c2);

    int s = 1;  // s = c1^x % p

    // Compute s = c1^x % p
    base = c1;
    exp = bob_private_key;
    mod = p;
    
    while (exp > 0) {
        if (exp % 2 == 1) {
            s = (s * base) % mod;
        }
        base = (base * base) % mod;
        exp = exp / 2;
    }

    int s_inv = 1;
    int s_temp = s;
    int p_temp = p;
    int t, q;
    int x0 = 0, x1 = 1;

    while (s_temp > 1) {
        q = s_temp / p_temp;
        t = p_temp;
        p_temp = s_temp % p_temp, s_temp = t;
        t = x0;
        x0 = x1 - q * x0;
        x1 = t;
    }

    if (x1 < 0) {
        x1 += p;
    }
    s_inv = x1;

    // Bob decrypts the message: M = c2 * s_inv % p
    int decrypted_message = (c2 * s_inv) % p;
    printf("Bob decrypts the message: %d\n", decrypted_message);
    
    return 0;
}
```

## OUTPUT:
 ![image](https://github.com/user-attachments/assets/82df41aa-bf4b-4935-9577-81666fb5eb2f)

## RESULT:
The program for ElGamal encryption and decryption was executed successfully.

