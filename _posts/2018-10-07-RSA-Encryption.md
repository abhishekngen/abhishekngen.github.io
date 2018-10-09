---
layout: post
title:  "Encryption#1 - RSA"
date:   2018-10-07
excerpt: "One of the many keepers of modern privacy"
image: "/images/pic14.png"
---
Encryption provides the primary layer of defence in modern security systems, and affects the privacy of our entire population – yet its specifics remain a topic that is seldom prominent within computer science theory. Hence I believe it is fitting to go through these various encryption algorithms, starting off with one of the more notable ones – RSA encryption.

RSA encryption is a type of **public-key encryption**, which is a broader category for encryption techniques that utilise two keys -  a public and private key. A “key” in this context is a mathematical function that can be used to disguise a message, or in computing jargon, convert cleartext into ciphertext. As a mathematical function clearly cannot be used on a string of characters, the characters that make up a message are converted into a number using a character encoding standard (numbers in this manner are still treated as characters). From here the key can then be used to convert the “cleartext” into ciphertext. 

<img src = "/images/pic15.png" width = "600" height = "450" />

*Character encoding table*

Now one may notice at this stage that even just one key would suffice – the key could be used to encrypt the message, then decrypt it on the receiving end (provided the key is a one-to-one or one-to-many function – in other words a function with an inverse). In fact, this technique is used in another type of encryption called symmetric encryption, in which there is one singular key (called the “symmetric” key) that is used to encrypt and decrypt the message. However, the noticeable disadvantage here is that if a third party manages to get the singular key, they can decrypt any message sent. And if a computer is sending a message to another computer, the computer obviously has to send the symmetric key as part of the transmission in order for the other computer to decrypt the message, hence making it even easier to gain access to the symmetric key. This is where the concept of public-key encryption comes in to cleverly solve this problem.

In public-key encryption, as aforementioned there are two keys, a public and private key. If we take an example of two computers communicating with each other, computer A and computer B, then in the instance of computer A sending a message to B, if public-key encryption is in use, then computer B sends its **public** key to A. A will then use this public key to encrypt the message it wishes to send to B, and then proceed to send it to B. But here’s the main difference – to decrypt the message encrypted by this public key, the public key cannot be reused; only the unique private key can be used to decrypt the encrypted message. This private key is stored in the computer that the message is being sent to, in this case B; and hence B is able to freely share its public key with any computer wishing to communicate with it. The only way to decrypt messages being sent to it is by gaining access to the private key, which is securely stored on B, and thus public-key encryption can be seen to be significantly more effective than symmetric encryption. All that needs to be done is to then implement a method in which these public and private keys are generated – and RSA is a popular example of this.

RSA utilises the NP problem that I mentioned in one of my previous articles – factorising a number into its primes. This cannot be performed in polynomial time, but verifying if a number’s prime factors are correct can be done in polynomial time. Hence the first step of RSA encryption is to multiply two very large and random prime numbers – let us call them **x** and **y**, and their product **z**.  Another integer is then calculated by multiplying **(x-1)** and  **(y-1)**, and then an integer denoted as **e** is calculated as a number *relatively* prime to the integer just calculated, which we will call **n**. For two numbers to be relatively prime, they must contain no common factors other than 1 and themselves. An algorithm is not actually needed to calculate a relatively prime number, but rather a library of numbers that would generally be relatively prime to the number calculated – for example, even a number like 3 will be relatively prime to any prime calculated excluding 3. The numbers **z** and **e** are then combined together to produce the public key. All that is left to calculate is then the private key, which is done by finding the modular inverse **d** of **e modulo(n)**. I won’t go too much into how the modular inverse is calculated, but I will briefly cover the basics – if we have integers **a, b, m**, then if **m** is a factor of **a-b**, we can write the integers like this:

**a = b(mod(m))**

Now using a variety of proofs, we have found we can also arrive at a similar formation if we have two integers with a greatest common factor of 1 – for example **a** and **b** - then we can express the following:

**ax = 1(mod(b))**

There always exists here an integer **x** that satisfies this condition, and this integer is known as the modular inverse of **a modulo(b)**. To find **x**, a special Euclidean algorithm is used – and this is also used to find **d**, the modular inverse of **e modulo(n)**, which is then used as the private key.

To actually encrypt and decrypt messages using these keys, the message is first translated into a number **m** using a character encoding standard, generally ASCII. The ciphertext is then calculated by performing the calculation **m<sup>e</sup>(mod(n))**. Once the ciphertext is received, the computer calculates **c<sup>d</sup> = m(mod(n))** using the private key to obtain m in cleartext. Using the character encoding standard, the number is then translated back into letters.

As you can see the RSA encryption is extremely secure – but with the advent of quantum computation, even it is starting to look significantly less safe. Algorithms such as Shor’s algorithm are capable of prime factorisation on a powerful enough quantum computer – all that needs to happen is for a powerful enough quantum computer to be developed.


