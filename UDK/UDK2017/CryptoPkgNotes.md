# [UDK2017]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2017) CryptoPkg Notes
##                            NEW FEATURES AND CHANGES
1. OpenSSL version was updated to the new 1.1.0 series of release. The current
   supported release is OpenSSL-1.1.0e (released at 2017-Feb-16).
   With this update, no extra openssl source patch is needed for EDKII build.
   Please refer to CryptoPkg/Library/OpensslLib/OpenSSL-HOWTO.txt on how to
   download & install OpenSSL for EDKII-CryptoPkg build.
2. One new TlsLib library was added to provide TLS support functions for EFI TLS
   Protocol and EFI TLS Configuration Protocol.
3. The header files for openssl and CRT wrappers were moved to the private include
   section. The external consumer modules should only use the interfaces defined
   in BaseCryptLib.h and TlsLib.h to access cryptographic and TLS functions.

##                            PACKAGE INTERFACE CHANGES
1. The following APIs were added into BaseCryptLib.h
   1) New xxxxHashAll APIs to facilitate the digest computation of blob data,
      including: Md4HashAll(), Md5HashAll(), Sha1HashAll(), Sha256HashAll(),
      Sha384HashAll(), and Sha512HashAll();
   2) HmacSha256GetContextSize(), HmacSha256Init(), HmacSha256Duplicate(),
      HmacSha256Update(), HmacSha256Final() for HMAC-SHA256 cipher support;
   3) Pkcs5HashPassword() API to provide PKCS#5 v2.0 PBKDF2 support (Password
      based encryption key derivation function, specified in RFC 2898);
2. New TLS library APIs defined in TlsLib.h for TLS context and TLS connection
   handling.

