---
layout: post
title: "Quantum Supremacy and Key Pairs"
date: 2022-12-26 00:00:00
description: Securing a communication link
tags: Networks Security
---

As the research in [quantum computing](https://www.ibm.com/topics/quantum-computing) advances, [quantum supremacy](https://en.wikipedia.org/wiki/Quantum_supremacy) in the field of cybersecurity becomes more of a concern.

The fear that once quantum computing becomes feasible for threat actors the security of the information transmitted through computer networks are no longer guaranteed is definitely a legit one. As explained in [this paper](https://arxiv.org/abs/1907.10454) by Zhang et al., the [asymmetric encryption](https://en.wikipedia.org/wiki/Public-key_cryptography) schemes widely used to secure our communication over the Internet which rely on integer factorization and discrete logarithm approaches are extremely weak against attacks from quantum computers.

As also mentioned in the paper by Zhang et al., the [symmetric encryption](https://en.wikipedia.org/wiki/Symmetric-key_algorithm) algorithms, on the other hand, do not face similar threat from the quantum computing technology due to the nature of its encryption scheme: only brute-force attacks can break it. While the symmetric encryption algorithms are also weaker against quantum computers if compared to the attacks performed with classical computers, it's just weaker in terms of the time complexity required to brute-force the key. In the classical computer setup, the expected time complexity is $$O(n)$$ where $$n$$ is the length of the encryption key. In the quantum computer setup, the expected time complexity is $$O(\sqrt{n})$$. Which means we can use the existing algorithms relatively safely in a quantum computing setup, but just with bigger value for $$n$$.

The main difference between asymmetric and symmetric encryption algorithms is the availability of public keys that can be used to guess the private key. The private key in asymmetric encryption system is usually used for signing messages as the proof of identity of the parties involved in the communication, aside for being used to decrypt messages encrypted by the public key associated with the said private key. The process of guessing the private key can be made feasible by quantum computers, which makes it concerning as asymmetric encryption systems are widely used for identity validation and communication security in the modern Internet infrastructure.

# Post-Quantum Technology Adoption

Nowadays, organizations have started to adopt [post-quantum cryptography](https://en.wikipedia.org/wiki/Post-quantum_cryptography) technologies. Providers of secure communication technologies have started to implement quantum safety into their products, such as [Tectia](https://www.ssh.com/products/tectia-ssh/quantum-safe).

The full adoption, however, should take some time, given how many layers of components have already been built into our networking infrastructure. Zhang et al., in the paper mentioned in the previous section, saw a parallel between post-quantum technology adoption and fixing the [Y2K bug](https://education.nationalgeographic.org/resource/Y2K-bug) in regards that both need fixes implemented in computer systems worldwide in order to avoid wordwide information systems disasters. The difference being that the Y2K bug has a clear deadline for its implementation and rollout: 31 December 1999. The post-quantum technology doesn't have a clear deadline for its adoption by now, as we still have no idea when quantum supremacy will be finally reached and our asymmetric encryption scehemes are finally broken.

Nevertheless, I have seen organizations adopting post-quantum cryptography technologies in their IT infrastructure setup which shows that they have invested in securing their infrastructure against future attacks enabled by quantum computers.

# Asymmetric Key Cryptography and Key Pairs

In an asymmetric key cryptography setup, the communicating parties are going to have their own key pairs containing a public key and a private key.

An example of asymmetric key cryptography algorithm is the [RSA algorithm](https://en.wikipedia.org/wiki/RSA_(cryptosystem)), which also involves a public key and a private key for each party involved. We will not be covering the RSA key generation algorithm in this post, but the RSA algorithm Wikipedia article has [a section](https://en.wikipedia.org/wiki/RSA_(cryptosystem)#Key_generation) for explaining the key generation steps.

As the names suggest, the public key is safe to share with other parties involved in the communication while the private key must be kept secret and should be stored securely. This has been known for decades by now, and is expected to be common knowledge for software developers and system/network admins.

# Technology vs People Investments

While it's very nice to see organizations has started adopting post-quantum cryptography technologies for their IT infrastructure, there was this case I encountered where an organization that has invested in post-quantum communication technology adoption still have their IT infrastructure admins weren't aware about how the public and private keys are supposed to be used and were careless with how they were handling their private key.

This was quite surprising for me, because while the organization has made the effort to invest in the cutting-edge security technologies, they were still lagging in the people and process side of things.

I say the issue might not be just about the people, but also the process, because if a proper process is put in place, ideally the private key should never leave the hands of a select few people who're well-trained in the topic and can be trusted to handle the private key with utmost care. Another point is that the asymmetric encryption technology has been around since the 1970s, which makes it a mature enough technology and has been included in various computer science-related curricula and cybersecurity training materials I've encountered so far.

I've only managed to come in contact with very few organizations that I noticed to have started to adopt post-quantum security technologies, but among those few I managed to notice one whose team might not get enough training on how to correctly handle their encryption keys. This brings me some concerns about how the other organizations are handling their cybersecurity investments, as it would be a waste to have such great technology available for them but still exposed to risks that should be avoidable if their personnel is well-trained on the fundamentals.

# References

[What is Quantum Computing?](https://www.ibm.com/topics/quantum-computing)

[Quantum supremacy](https://en.wikipedia.org/wiki/Quantum_supremacy)

[Quantum Advantage and Y2K Bug: Comparison](https://arxiv.org/abs/1907.10454)

[Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography)

[Symmetric-key algorithm](https://en.wikipedia.org/wiki/Symmetric-key_algorithm)

[Post-quantum cryptography](https://en.wikipedia.org/wiki/Post-quantum_cryptography)

[Tectia® SSH Client/Server Quantum-Safe Edition](https://www.ssh.com/products/tectia-ssh/quantum-safe)

[Y2K bug](https://education.nationalgeographic.org/resource/Y2K-bug)

[RSA (cryptosystem)](https://en.wikipedia.org/wiki/RSA_(cryptosystem))
