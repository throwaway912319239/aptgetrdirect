I've been observing unusual behavior from the ubuntu repository archives (*.archive.ubuntu.com). The requests being made for package downloads are getting redirected to IP addresses not obviously associated with Canonical. The redirection occurs randomly, but is very reproducible and occurs regardless of software or user-agent -- tested using apt-get, curl, wget, and web browser. 

So far I've tested this behaviour against the following domains, with decreasing frequency of reproducibility:

    archive.ubuntu.com
    us.archive.ubuntu.com
    uk.archive.ubuntu.com
    cz.archive.ubuntu.com
    ru.archive.ubuntu.com
    cn.archive.ubuntu.com

NOTE: This behavior has only been reproducible on Cox Cable networks, for reference I'm in east.cox.net on the New Orleans/Baton Rouge trunk.

I've included two packet capture logs showing both the expected behavior and the effects of the redirection. The redirection appears to be coming from the ubuntu servers (302 response) at which point the requested file is proxied by the redirected server along with numerous duplicate tcp packets from the original server.

Tests of the SHA-256 hashes from the redirected data match with the files received directly from us.archive.ubuntu.com and pass key-ring signing checks -- ie: apt-get will install the packages without any warnings.

The redirect servers are running on Cox Cable IPs in the 68.106.66.0/24 address range and there doesn't appear to be any DNS records attached to the IP addresses. 

See: redirector.txt for a more detailed explanation of the behaviour of the redirection servers and list of IP addresses (110 total)

See: curl.txt for curl/wget logs showing expected, redirected, and double redirected transactions.

ERRATA: 
Redirection only seems to occur when resolving the ubuntu servers by name -- direct connections using the IP address do not seem to be affected.

STEPS FOR REPRODUCING:
    # run the following command multiple times and look for a 302 response
    wget http://us.archive.ubuntu.com/ubuntu/pool/main/r/resolvconf/resolvconf_1.78ubuntu5_all.deb

