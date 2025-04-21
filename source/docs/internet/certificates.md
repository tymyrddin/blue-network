# Certificate validation

Certificate validation has evolved significantly to combat BGP hijacking, phishing, and fraudulent certificate issuance. 
Below are techniques and how to implement them.

| Technique	Purpose	How to Deploy
| CAA Records	| Restrict CA issuance	| Add DNS CAA entries |
| Expect-CT Header	| Enforce CT logging	| Nginx/Apache header |
| Multi-Perspective Validation	| Stop BGP attacks	| Use Let’s Encrypt/Google |
| EV Certificates	| High-trust sites	| Buy from DigiCert/Sectigo |
| Short-Lived Certs	| Reduce exposure	| Use Let’s Encrypt |
| CT Monitoring	Detect rogue certs	| Cert Spotter/Octo |

```{contents} Table of Contents
:depth: 3
```

## Certificate Pinning (HPKP Replacement)

HTTP Public Key Pinning (HPKP) was deprecated due to risks of accidental DoS. 

### Expect-CT Header (Certificate Transparency Enforcement)

Forces browsers to check CT logs before accepting certificates.

How to Implement:

```
add_header Expect-CT "enforce, max-age=86400, report-uri='https://example.com/report-ct'";
```

Browser Support: Chrome, Firefox, Edge.

### Certificate Authority Authorization (CAA)

Restricts which CAs can issue certs for your domain.

DNS Record Example:

```
example.com. CAA 0 issue "letsencrypt.org"
example.com. CAA 0 issuewild "digicert.com"
example.com. CAA 0 iodef "mailto:security@example.com"
```

Validation: All CAs must check CAA records before issuance.

### Trusted Types (Browser-Level pinning)

Uses WebTrust-certified CAs with strict audits.

Deployment:

* Choose CAs with Extended Validation (EV) or Organization Validation (OV).
* Avoid Domain Validation (DV) for critical services.

## Extended Validation (EV) certificates

Still relevant for high-security sites (banks, governments).

Requirements:

* Legal entity verification (via government databases).
* Physical address confirmation.
* Manual review by the CA.

How to Get an EV Certificate:

* Purchase from a WebTrust-accredited CA (DigiCert, Sectigo, GlobalSign).
* Submit legal documents (e.g., business license).
* Wait 3–7 days for manual approval.

## Certificate Transparency (CT)

Now mandatory for all certificates (not just EV).

How to Monitor:

Google’s CT Dashboard: https://transparencyreport.google.com/https/certificates

Open Source Tools:

* Cert Spotter (SSLMate): Alerts for new certs.
* Facebook’s Octo (CT log monitor).

Enforce CT via Headers:

```
add_header Expect-CT "enforce, max-age=86400";
```

## Multi-Perspective Domain Validation

Defeats local BGP hijacks by validating domains from multiple global locations.

Used by: Let’s Encrypt, Google, Cloudflare.

How It Works:

* The CA checks your domain from AWS, Azure, and GCP regions.
* If all locations resolve correctly, validation passes.

## ACME DNS-01 Challenges (Not HTTP-01)

* HTTP-01 is vulnerable to BGP hijacking.
* DNS-01 is safer (requires hijacking both BGP and DNS).

How to Use (Certbot example):

```
certbot certonly --manual --preferred-challenges dns -d example.com
```

## Short-Lived Certificates (Automated Rotation)

Reduces exposure time for stolen certs.

Tools:

* Let’s Encrypt: 90-day certs (auto-renewed).
* Google’s Certificate Manager: 7-day certs.

## Post-Quantum certificates (Future-proofing)

* NIST-approved algorithms (e.g., CRYSTALS-Dilithium).
* Early adopters: Cloudflare, Google.

