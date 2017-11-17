I've been observing unusual behavior from the ubuntu repository archives (*.archive.ubuntu.com). The requests being made for package downloads are getting redirected to IP addresses not obviously associated with Cononical. The redirection occurs randomly, but is very reproducible and occurs regardless of software or user-agent -- tested using apt-get, curl, wget, and web browser. 

So far I've tested this behaviour against the following domains, with decreasing reproducibilty :
    archive.ubuntu.com
    us.archive.ubuntu.com
    uk.archive.ubuntu.com
    cz.archive.ubuntu.com
    ru.archive.ubuntu.com
    cn.archive.ubuntu.com

NOTE: This behavior has only been reproducible on Cox Cable networks, for reference I'm in east.cox.net on the New Orleans/Baton Rouge trunk.

I've included two packet capture logs showing both the expected behavior and the effects of the redirection. The redirection appears to be coming from the ubuntu servers (302 response) at which point the requested file is proxied by the redirected server along with numerous duplicate tcp packets from the original server.

Tests of the SHA-256 hashes from the redirected data match with the files received directly from us.archive.ubuntu.com and pass key-ring signing checks -- ie: apt-get will install the packages without any warnings.

The redirect servers are running on Cox Cable IPs in the 68.106.66.0/24 address range and there doesn't appear to be any DNS records attached to the IP addresses. See: redirector.txt for a more detailed explanation and list of IP addresses


