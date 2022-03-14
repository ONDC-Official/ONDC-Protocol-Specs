# Auth Header - Signing & Verification

## 1. Pre-requisites

Key pairs, for signing & encryption, can be generated using [libsodium](https://libsodium.gitbook.io/doc/bindings_for_other_languages).

## 2. Creating Key Pairs

   - Create key pairs, for signing & encryption, in hex encoded form, compliant with [RFC7748](https://datatracker.ietf.org/doc/html/rfc7748);

   - Update public key in registry;

   - For ONDC live registry, ONDC will also provide an utility to generate the self-signed DSC with key pairs;

## 3. Auth Header Signing

   - Create the signature string as documented [here](https://docs.google.com/document/d/1Iw_x-6mtfoMh0KJwL4sqQYM0kD17MLxiMCUOZDBerBo/edit#), including digest of the response body;
   - Sign the request using ed25519 algorithm;
   - Encode the signature generated in above step using base64 encoding algorithm;
   - Add this to authorization header as in the document

## 4. Auth Header Verification

   - Extract the encoded signature from the request;

   - Get the signing_public_key from registry using lookup (by verifying the ukid in the authorization header);

   - Verify signature by decoding and verifying the signature using the public key;
