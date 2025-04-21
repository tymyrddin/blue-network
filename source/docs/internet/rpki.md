# Deploying Resource Public Key Infrastructure (RPKI)

```{contents} Table of Contents
:depth: 3
```

## Prerequisites

* Access to the RIR account (ARIN, RIPE, APNIC, etc.).
* Admin access to the BGP routers (Cisco, Juniper, BIRD, FRR).
* Public ASN and IP prefixes (must be registered in the RIR).

## Create ROAs (Route Origin Authorizations)

ROAs cryptographically authorize your ASN to announce specific prefixes.

### Log in to Your RIR Portal

* RIPE NCC: https://my.ripe.net
* ARIN: https://arin.net
* APNIC: https://myapnic.net

### Create a ROA

1. Navigate to RPKI → ROAs.
2. Add a new ROA:
   * Prefix: 192.0.2.0/24 (your IP range)
   * Max Length: 24 (or longer if sub-delegating)
   * ASN: AS12345 (your AS number)
3. Sign and publish the ROA (takes ~5 mins to propagate).

## Configure RPKI Validation on Routers

### Cisco IOS-XR

```
router bgp 65000
 rpki server tcp 203.0.113.1  # RPKI validator (e.g., Cloudflare/RIPE)
  refresh-time 30
  shutdown
 !
 address-family ipv4 unicast
  rpki origin-as validation enable
 !
!
```

Test validation:

```
show bgp ipv4 unicast 192.0.2.0/24    # Look for rpki: Valid in the output.
```

### Juniper JunOS

```
set routing-options validation group RPKI-VALIDATOR
  session 203.0.113.1 port 323
set protocols bgp group EBGP import VALIDATE_ROUTES
set policy-options policy-statement VALIDATE_ROUTES
  term RPKI-VALID from protocol bgp
  term RPKI-VALID then validation state valid accept
  term RPKI-INVALID then validation state invalid reject
```

### BIRD (Linux)

```
roa4 table r4;
protocol rpki {
  roa4 { table r4; };
  remote "203.0.113.1" port 323;
}
protocol bgp {
  ipv4 {
    import where roa_check(r4, net, bgp_path.last) = ROA_VALID;
  };
}
```

## Set Up an RPKI Validator (Optional)

If you want your own validator (instead of public ones like Cloudflare/RIPE):

```
# Install Routinator (Linux)
curl -sSL https://github.com/NLnetLabs/routinator/releases/latest/download/routinator-install.sh | bash
routinator init --accept-arin-rpa
routinator server --rtr 203.0.113.1:323 --http 8080
```

Point routers to your validator (`203.0.113.1:323`).

## Monitor RPKI status

### Check ROA coverage

* RIPE NCC RPKI Dashboard: https://rpki.groot.app
* Cloudflare RPKI Explorer: https://rpki.cloudflare.com

### Test Prefix validation

```
# Linux (RPKI-client)
rpki-client -n -j | jq '.roas[] | select(.prefix == "192.0.2.0/24")'
```

Should show your ASN as valid.

## Enforce RPKI in BGP Policies

Reject invalid routes from peers:

Cisco IOS-XR:

```
route-policy RPKI-FILTER
  if validation-state is invalid then
    drop
  endif
end-policy
router bgp 65000
 neighbor 203.0.113.2
  address-family ipv4 unicast
   route-policy RPKI-FILTER in
```

FRRouting (Linux):

```
bgp community-list standard RPKI-INVALID seq 5 permit 0x4300:0xFFFFFFFF
route-map RM-PEER-IN deny 10
  match community RPKI-INVALID
route-map RM-PEER-IN permit 20
router bgp 65000
 neighbor 203.0.113.2 route-map RM-PEER-IN in
```

## Verify Deployment

1. Announce a test prefix (198.51.100.0/24) without a ROA.
2. Check if neighbors reject it:

```
show bgp ipv4 unicast 198.51.100.0/24
```

Should show `rpki: Invalid` and be dropped.

