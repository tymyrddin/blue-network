# Adopting Post-Quantum Cryptography (PQC)

```{contents} Table of Contents
:depth: 3
```

## For Developers: Integrate PQC Libraries

Choose a Library: 

* AWS Libcrypto (PQ-enabled OpenSSL fork).
* OpenQuantumSafe (OQS): Supports NIST-standardized algorithms (Kyber, Dilithium).

```
git clone https://github.com/open-quantum-safe/liboqs
cd liboqs && mkdir build && cd build
cmake .. && make
```

Hybrid Key Exchange Example (OQS + TLS 1.3):

```python
# Python (using OQS-OpenSSL)
from socket import socket, AF_INET, SOCK_STREAM
from ssl import PROTOCOL_TLS_CLIENT, SSLContext
context = SSLContext(PROTOCOL_TLS_CLIENT)
context.set_ciphers('KYBER512:RSA')  # Hybrid PQ + classical
with socket(AF_INET, SOCK_STREAM) as sock:
    with context.wrap_socket(sock, server_hostname='example.com') as ssock:
        ssock.sendall(b"GET / HTTP/1.1\r\nHost: example.com\r\n\r\n")
```

Test with Cloudflare’s PQ-enabled Services:

```
curl --curves X25519:Kyber512 https://pq.cloudflare.com
```

## For Sysadmins: Prepare for PQ migration

Monitor NIST’s PQC Standardization (2024 finalists: Kyber, Dilithium).

Audit systems for PQ readiness using:

```
nmap --script ssl-enum-ciphers -p 443 example.com | grep "PQ"
```

