# Auth Header - signing and verification



## 1. Pre-requisites

Key pairs, for signing & encryption, can be generated using different publicly available libraries. Some of these libraries include:

Java - [BouncyCastle](https://github.com/venkatramanm/beckn-sdk-java/blob/master/src/test/java/in/succinct/beckn/SampleUseCase.java)

Python - [ed25519](https://cryptobook.nakov.com/digital-signatures/eddsa-sign-verify-examples) 

C, C#, C++ - [libsodium](https://libsodium.gitbook.io/doc/), [NaCl](https://nacl.cr.yp.to/features.html)

## 2. Creating Key Pairs

- Create key pairs, for signing & encryption, in binary form (e.g. using b64encode() in Python);
- Update public key in registry;
- For ONDC live registry, ONDC will also provide an utility to generate the self-signed DSC with key pairs;

## 3. Auth Header Signing

- Create digest of the response body (e.g. using hashlib.blake2b in Python);
- Create the signature string as documented [here](https://docs.google.com/document/d/1Iw_x-6mtfoMh0KJwL4sqQYM0kD17MLxiMCUOZDBerBo/edit#);
- Sign the request using ed25519 algorithm (private_key.sign(use string generated in above step) in Python);
- Encode the signature generated in above step using base64 encoding algorithm;
- Add this to authorization header as in the document

## 4. Auth Header Verification

- Extract the encoded signature from the request;
- Get the signing_public_key from registry using lookup (by verifying the ukid in the authorization header);
- Verify signature by decoding and verifying the signature using the public key;
