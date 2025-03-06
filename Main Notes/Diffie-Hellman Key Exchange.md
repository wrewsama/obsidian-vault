Tags:
- [[Computer Networking]]
- [[Cybersecurity]]
---
Cryptographic technique that enables 2 parties to securely generate a shared secret key over an insecure communication channel

**Formula**
Party A has a private key (integer) a
Party B has a private key b
Both parties agree on 2 large prime numbers g and p

A computes a public key
A = `g^a mod p`

B does the same
B = `g^b mod p`

they then exchange their public keys. The shared, symmetric key can be calculated by both parties by taking

in A: `B^a mod p`
in B: `A^b mod p`

(both are equal to `g^ab mod p`)

since the private keys a and b are never sent over the network, hackers cannot feasibly compute the shared secret key.

---
## References
- 