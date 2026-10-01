# Problem 1: Understanding the One-Time Pad

## Definition

The one-time pad (OTP) encrypts a message m of n bits using a key k of the same length by bitwise XOR:

    c = m XOR k

Decryption is the same operation, because XORing with k twice cancels it:

    m = c XOR k = (m XOR k) XOR k

## Conditions required for perfect secrecy

All four must hold, and breaking any one of them removes the guarantee:

1. **The key is truly random.** Every bit is uniform and independent. It must come from a cryptographically secure
   or hardware source, not a predictable generator or a typed passphrase.
2. **The key is at least as long as the message.** It is never repeated or stretched.
3. **The key is used only once.** Every message needs fresh key material.
4. **The key is kept secret.** It is shared securely in advance between the two parties and destroyed after use.

## Operating principle: why it is perfectly secret

For a fixed ciphertext c and any candidate plaintext m of the same length, exactly one key maps m to c, namely
k = m XOR c. Because every key is equally likely (probability 2^-n), every plaintext is equally likely given c:

    Pr[M = m | C = c] = Pr[M = m]

So the ciphertext reveals nothing about the message except its length, even to an attacker with unlimited computing
power. This is Shannon's definition of perfect secrecy.

## Limitations

- **Key distribution and storage:** the key is as long as all the traffic, and must be delivered securely ahead of time.
- **Length leaks:** the ciphertext is the same length as the message.
- **No integrity:** flipping a ciphertext bit flips the same plaintext bit (malleability), so an attacker can tamper
  with a message without knowing it.

## What goes wrong when the key is reused

If c1 = m1 XOR k and c2 = m2 XOR k, then

    c1 XOR c2 = m1 XOR m2

The key cancels, leaving the XOR of two plaintexts. English has enough redundancy that this is usually enough to
recover both messages. This is what the Venona project exploited, where reused Soviet pad pages let cryptanalysts
read traffic. Problem 4 carries out this attack.
