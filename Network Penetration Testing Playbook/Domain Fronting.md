Domain fronting is a technique to mask C2 traffic, mask dns hostname, mask c2 location. And most companies trust legitimate traffic from cdns like amazon and google and that helps with masking traffic. Domain fronting was originally used to bypass and circumvent internet censorship. This technique allows us to fetch arbitrary website content from a CDN even though the initial TLS session is targeting a different domain. This works because TLS and HTTP sessions are handled independently. For example we can initiate a tls session to site1 and get the contents of site 2. 

To explain how this works we have to look at HTTP host headers. With the advent of virtual hosting multiple web sites with different domains could be hosted on one server from one IP address. 

After the dns lookup the information about the domain is lost 
