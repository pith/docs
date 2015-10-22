---
title: "Overview"
type: "manual"
zones:
    - "Seed"
sections:
    - "SeedCrypto"
tags:
    - "security"
    - "cryptography"
menu:
    SeedCrypto:
        weight: 10
---

Seed provides helpers for Java cryptography:

 * Private key encryption
 * Secure hashing
 * KeyStore configuration
 * SSL configuration

Enable this features in your project by adding the following dependency.

```xml
<dependency>
    <groupId>org.seedstack.seed</groupId>
    <artifactId>seed-crypto</artifactId>
</dependency>
```

# Encryption

Private key encryption allows you to securely store or exchange data. It is based on a Public Key Infrastructure (PKI),
e.g. a public key crypts the data and a private key decrypts it. Java provides key stores in order to secure the keys.
Seed facilitates their usage by allowing you to configure one or more key stores in your props files and then
to inject them in your code. Here is the expected configuration format

```ini
[org.seedstack.seed.crypto.keystore.keystoreName]
path=src/main/resources/app.keystore
password=changeit

# Optional configuration
type=JKS
provider=...
```

The key store configuration is prefixed by `org.seedstack.seed.crypto.keystore.keystoreName`. The prefix ends with a
logical key store name. Java key stores protect keys using passwords and associate public/private key pairs to aliases.

```ini
[org.seedstack.seed.crypto.keystore.keystoreName.alias]
client.password=21B06221FC9EC83BAAD
ssl.password=70E65711ACFEF03F59A
```

When a key store is configured with a key pair. It is then possible to use inject an `EncryptionService`.

```java
@Inject
@Named("client") // named with the key pair alias
private EncryptionService encryptionService;
```

And use it to crypt or decrypt data as follows:

```java
final String stringToCrypt = "essai crypting";
byte[] encryptedString = encryptionService.encrypt(chaine.getBytes());
byte[] decryptedString = encryptionService.decrypt(encrypt);
```

Sometimes you will only have a certificate file. It is recommended to store certificate in a key store. But if you don't,
you can still use this certificate as follows:

```ini
[org.seedstack.seed.crypto.cert]
client.resource=client.cer
```

or

```ini
[org.seedstack.seed.crypto.cert]
client.file=src/main/resources/seed.crt
```

# KeyStores

The java cryptographic architecture provides many features. The `EncryptionService` provided by Seed crypto is not
intended to address them all. If you want to use more advanced features, you can directly access a configured key store
through injection as follows:

```java
@Inject
@Named("keystoreName") // key store name
private KeyStore keyStore;
```

# Secure hashing

Seed crypto also comes with an `HashService` which provides the current best default hashing algorithms.

```java
@Inject
private HashingService hashingService;
...
Hash hash = hashingService.createHash("string to hash");
```

It also provides a **secure password validation**. It takes a password provided by a user, hash it and check
it against the previously hashed password.

```java
Hash hash = hashingService.validatePassword(passwordToCheck, verifiedHash);
```

# Configuration files protection

Sometimes, you need to have **sensitive data in your configuration files**. Using the Seed cryto it is possible to have
encrypted values in your props files. This values are decrypted at runtime. For instance, you can encrypt a password
and specify it in the props file with the `${password:XXX}` syntax.

```ini
[org.seedstack.seed.crypto.keystore.keystoreName.alias]
client.password=${password:70E65711ACFEF03F59AFCED...F96563A19B18954B49DD59}
```

The password decrypting is done using an `EncryptionService`. This service expect a key store named `master` with a
key alias named `seed`.

```ini
[org.seedstack.seed.crypto.keystore.master]
path=src/main/resources/masterkeystore.jck
password=${env:KS_PASSWD}
alias.seed.password=${env:KEY_PASSWD}
```

{{% callout info %}}
Describing **how to create a Java key store** or how to use it is not a goal of this documentation.
If you want more information about key store creation, see this [oracle documentation](https://docs.oracle.com/cd/E19509-01/820-3503/ggfen/index.html).
{{% /callout %}}