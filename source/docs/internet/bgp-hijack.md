# BGP hijacking mitigations

* Some proposed mitigations for BGP hijacking mention measuring the Time to Live (TTL) field of incoming IP packets and filtering on that, but TTL is also easy to forge by a MitM. Measuring Round-Trip Time (RTT) would not be so easy to forge.
* A hijacked route is relatively easy to detect and mitigate. Resource Public Key Infrastructure (RPKI) and BGPsec address this issue (to some extent). Global hijacking is rare, local hijacking possible under certain conditions, but also opens the possibility to hijack a prefix in such way that the hijack will not be seen by large ISPs, and RTT for most customers will not change either.
* A route leak is much harder to understand and prevent without a complex network monitoring solution.
* Using data aggregation from global route collection (DNS analytics) to filter prefix announcements coming from peers before passing them on to others (prefix de-aggregation, invalid originations, invalid AS (Autonomous System) adjacencies, and perhaps even improbable AS paths) may work, but none of that makes sense if ISP's do not even secure their networks properly.
* In general, large ISPs often implement some measures to prevent hijacking and leaking, but probably for the same reasons small ISPs often leave their network equipment and even the border routers unpatched and vulnerable, small ISPs seem not to care about [prefix filtering](https://www.noction.com/knowledge-base/bgp-prefix-filtering).

On the lookout for:

* RPKI validating caches
* BGP routers that support origin and path validation extensions

