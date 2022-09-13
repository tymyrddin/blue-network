# Certificate validation

* There are proposals for being able to "pin" domains to a specific certificate authority (for a fee of course), in which a certificate authority is to do extra diligence before considering issuing a certificate for a "pinned" domains.
* A new "pinning" header is being implemented by servers and browsers to protect against fraudulent certificates, but are not widely used yet.
* Some certificate authorities use multiple clients around the world to do their domain control validation. That will not stop a local BGP hijacking of a target domain, but does stop hijacking of the certificate authority validation test traffic itself.
* The Extended Validation (EV) process for certificates requires additional validation, including based on contact details provided in a qualified information source. An adversary would have to compromise that third-party source in addition to doing the BGP hijacking.
* The Certificate Transparency (CT) project is working on a global repository of domains, certificates, and associated certificate authorities. CT encourages domain owners to register with a monitoring service that will notify them if another certificate is ever issued. At the moment, only EV certificates are required to be registered with CT Logs.
