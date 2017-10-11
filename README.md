# check_dnssec_validation.py
An extremely short Nagios/Icinga script to validate the DNSSEC chain of trust.
It does so by querying for the SOA record of a zone and validating the records.

As this script validates the chain of trust, it is possible it will report DNSSEC failures for domains that are not the domain's operator's fault.
e.g. A DNSSEC issue with the top-level domain will cause an alert.

# Usage
This script is meant to be invoked by Nagios or Icinga and has the following states:

 * OK: DNSSEC chain was validated
 * WARNING: the DNSSEC validation result was insecure (but not bogus)
 * CRITICAL: the DNSSEC validation result was bogus

This script has 2 options:

 - **-z ZONE** -- Specifies the zone to be checked, mandatory option
 - **--insecure-is-ok** -- When the result of the check is insecure, the state is OK and not WARNING

# TODO
This script is very simple and works in most cases to detect errors in the DNSSEC validation chain.
It could use the following:

 - Base the check on python-nagios
 - Configurable location for the root trust anchor
 - Checking the signatures from all nameservers in the final NSSet
 - Checking all signatures from all nameservers (might be slow/expensive and not nice to TLD/root operators)
 - Stop relying on python-unbound (as it is not generally available on e.. Arch) and use dnspython
