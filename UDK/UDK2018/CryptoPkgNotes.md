# [UDK2018]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2018) CryptoPkg Notes

##                            NEW FEATURES AND CHANGES

1. OpenSSL version was updated to the new 1.1.0g (released at 02-Nov-2017).

2. OpenSSL git repository was added into EDK II project as one submodule.    Please refer to CryptoPkg/Library/OpensslLib/OpenSSL-HOWTO.txt on how to    clone / pull main EDK II repo and `openssl` submodule.

3. The `Cryptest` application was removed from `CryptoPkg`, which was only for unit   test.

##                            PACKAGE INTERFACE CHANGES

1. The following API was added into `BaseCryptLib.h`

    1) One new `X509GetCommonName()` API to retrieve the subject common name (CN)       string from one` X.509` certificate.
