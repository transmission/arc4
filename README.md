# ARC4: Alleged RC4 implementation

This is a tiny and reusable implementation of [alleged RC4](https://en.wikipedia.org/wiki/RC4) cipher.

The use of RC4 is declining due to various security concerns.
A number of popular cryptographic libraries have started to mark it as deprecated or remove it completely:
* **OpenSSL**: not built by default in 1.1, moved to "legacy" provider in 3.0
* **WolfSSL (CyaSSL)**: not built by default in 3.4.6
* **MbedTLS (PolarSSL)**: removed in 3.0

Nonetheless, it's still used in e.g. BitTorrent software, and forcing people to do some more work to make RC4 available in the aforementioned libraries is not always possible or desirable.
