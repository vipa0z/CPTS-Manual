## Why DNS Matters for Web Recon

DNS is not merely a technical protocol for translating domain names; it's a critical component of a target's infrastructure that can be leveraged to uncover vulnerabilities and gain access during a penetration test:

- `Uncovering Assets`: DNS records can reveal a wealth of information, including subdomains, mail servers, and name server records. For instance, a `CNAME` record pointing to an outdated server (`dev.example.com` CNAME `oldserver.example.net`) could lead to a vulnerable system.
- `Mapping the Network Infrastructure`: You can create a comprehensive map of the target's network infrastructure by analysing DNS data. For example, identifying the name servers (`NS` records) for a domain can reveal the hosting provider used, while an `A` record for `loadbalancer.example.com` can pinpoint a load balancer. This helps you understand how different systems are connected, identify traffic flow, and pinpoint potential choke points or weaknesses that could be exploited during a penetration test.
- `Monitoring for Changes`: Continuously monitoring DNS records can reveal changes in the target's infrastructure over time. For example, the sudden appearance of a new subdomain (`vpn.example.com`) might indicate a new entry point into the network, while a `TXT` record containing a value like `_1password=...` strongly suggests the organization is using 1Password, which could be leveraged for social engineering attacks or targeted phishing campaigns.
# SERVERS IN DNS RESOLUTION

|DNS Concept|Description|Example|
|---|---|---|
|`Domain Name`|A human-readable label for a website or other internet resource.|`www.example.com`|
|`IP Address`|A unique numerical identifier assigned to each device connected to the internet.|`192.0.2.1`|
|`DNS Resolver`|A server that translates domain names into IP addresses.|Your ISP's DNS server or public resolvers like Google DNS (`8.8.8.8`)|
|`Root Name Server`|The top-level servers in the DNS hierarchy.|There are 13 root servers worldwide, named A-M: `a.root-servers.net`|
|`TLD Name Server`|Servers responsible for specific top-level domains (e.g., .com, .org).|[Verisign](https://en.wikipedia.org/wiki/Verisign) for `.com`, [PIR](https://en.wikipedia.org/wiki/Public_Interest_Registry) for `.org`|
|`Authoritative Name Server`|The server that holds the actual IP address for a domain.|Often managed by hosting providers or domain registrars.|
|`DNS Record Types`|Different types of information stored in DNS.|A, AAAA, CNAME, MX, NS, TXT, etc.|

# DNS RECORD TYPES

|Record Type|Full Name|Description|Zone File Example|
|---|---|---|---|
|`A`|Address Record|Maps a hostname to its IPv4 address.|`www.example.com.` IN A `192.0.2.1`|
|`AAAA`|IPv6 Address Record|Maps a hostname to its IPv6 address.|`www.example.com.` IN AAAA `2001:db8:85a3::8a2e:370:7334`|
|`CNAME`|Canonical Name Record|Creates an alias for a hostname, pointing it to another hostname.|`blog.example.com.` IN CNAME `webserver.example.net.`|
|`MX`|Mail Exchange Record|Specifies the mail server(s) responsible for handling email for the domain.|`example.com.` IN MX 10 `mail.example.com.`|
|`NS`|Name Server Record|Delegates a DNS zone to a specific authoritative name server.|`example.com.` IN NS `ns1.example.com.`|
|`TXT`|Text Record|Stores arbitrary text information, often used for domain verification or security policies.|`example.com.` IN TXT `"v=spf1 mx -all"` (SPF record)|
|`SOA`|Start of Authority Record|Specifies administrative information about a DNS zone, including the primary name server, responsible person's email, and other parameters.|`example.com.` IN SOA `ns1.example.com. admin.example.com. 2024060301 10800 3600 604800 86400`|
|`SRV`|Service Record|Defines the hostname and port number for specific services.|`_sip._udp.example.com.` IN SRV 10 5 5060 `sipserver.example.com.`|
|`PTR`|Pointer Record|Used for reverse DNS lookups, mapping an IP address to a hostname.|`1.2.0.192.in-addr.arpa.` IN PTR `www.example.com.`|

# examples
### Who does the resolver ask for `www.example.com`?

1. **First, your computer asks the resolver (usually your ISP’s DNS server):**  
    “What’s the IP address of `www.example.com`?”
    
2. **The resolver doesn’t know immediately, so it starts the lookup:**
    
    - It asks a **root name server**:  
        “Who handles `.com` domains?”
        
    - The root server replies with the **names of the `.com` TLD name servers** (via their NS records), for example:  
        `a.gtld-servers.net`, `b.gtld-servers.net`, etc.
        
3. **The resolver then asks one of the `.com` TLD servers:**  
    “Who handles `example.com`?”
    
    - The `.com` TLD server responds with the **authoritative name servers for `example.com`** — that is, it sends back the NS records for `example.com`, such as:  
        `ns1.exampledns.com`  
        `ns2.exampledns.com`
        
4. **Finally, the resolver asks one of those authoritative name servers:**  
    “What’s the IP address of `www.example.com`?”
    
    - The authoritative server responds with the **A or CNAME record** for `www.example.com`.


## DNSENUM
#DNSEnum

`dnsenum` is a versatile and widely-used command-line tool written in Perl. It is a comprehensive toolkit for DNS reconnaissance, providing various functionalities to gather information about a target domain's DNS infrastructure and potential subdomains. The tool offers several key functions:

- `DNS Record Enumeration`: `dnsenum` can retrieve various DNS records, including A, AAAA, NS, MX, and TXT records, providing a comprehensive overview of the target's DNS configuration.
- `Zone Transfer Attempts`: The tool automatically attempts zone transfers from discovered name servers. While most servers are configured to prevent unauthorised zone transfers, a successful attempt can reveal a treasure trove of DNS information.
- `Subdomain Brute-Forcing`: `dnsenum` supports brute-force enumeration of subdomains using a wordlist. This involves systematically testing potential subdomain names against the target domain to identify valid ones.
- `Google Scraping`: The tool can scrape Google search results to find additional subdomains that might not be listed in DNS records directly.
- `Reverse Lookup`: `dnsenum` can perform reverse DNS lookups to identify domains associated with a given IP address, potentially revealing other websites hosted on the same server.
- `WHOIS Lookups`: The tool can also perform WHOIS queries to gather information about domain ownership and registration details.