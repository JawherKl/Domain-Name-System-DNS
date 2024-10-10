# How DNS Works

DNS lookup involves the following eight steps:

1. A client types `example.com` into a web browser; the query travels to the internet and is received by a DNS resolver.
2. The resolver then recursively queries a DNS root nameserver.
3. The root server responds to the resolver with the address of a Top Level Domain (TLD).
4. The resolver then makes a request to the `.com` TLD.
5. The TLD server responds with the IP address of the domain's nameserver, `example.com`.
6. The recursive resolver sends a query to the domain's nameserver.
7. The IP address for `example.com` is returned to the resolver from the nameserver.
8. Finally, the DNS resolver responds to the web browser with the IP address of the initially requested domain.

Once the IP address has been resolved, the client can request content from the resolved IP address, such as a webpage to be rendered in the browser.

## Server Types

Now, let's look at the four key groups of servers that make up the DNS infrastructure:

### DNS Resolver

A DNS resolver (also known as a DNS recursive resolver) is the first stop in a DNS query. The recursive resolver acts as a middleman between a client and a DNS nameserver. After receiving a DNS query from a web client, a recursive resolver will either respond with cached data or send a request to a root nameserver, followed by another request to a TLD nameserver, and then one last request to an authoritative nameserver. After receiving a response from the authoritative nameserver containing the requested IP address, the recursive resolver sends a response to the client.

### DNS Root Server

A root server accepts a recursive resolver's query that includes a domain name. The root nameserver responds by directing the recursive resolver to a TLD nameserver based on the extension of that domain (.com, .net, .org, etc.). The root nameservers are overseen by a nonprofit organization called the Internet Corporation for Assigned Names and Numbers (ICANN). 

There are 13 DNS root nameservers known to every recursive resolver. Note that while there are 13 root nameservers, it doesn't mean that there are only 13 machines in the root nameserver system. There are 13 types of root nameservers, but multiple copies of each exist around the world, using Anycast routing to provide speedy responses.

### TLD Nameserver

A TLD nameserver maintains information for all the domain names that share a common domain extension, such as `.com`, `.net`, or any other extension after the last dot in a URL. 

Management of TLD nameservers is handled by the Internet Assigned Numbers Authority (IANA), a branch of ICANN. The IANA categorizes TLD servers into two main groups:

- **Generic top-level domains**: Domains like `.com`, `.org`, `.net`, `.edu`, and `.gov`.
- **Country code top-level domains**: Domains specific to a country or state, such as `.uk`, `.us`, `.ru`, and `.jp`.

### Authoritative DNS Server

The authoritative nameserver is usually the resolver's last step in the journey for an IP address. The authoritative nameserver contains information specific to the domain name it serves (e.g., google.com) and can provide the recursive resolver with the IP address found in the DNS A record. If the domain has a CNAME record (alias), it will provide the recursive resolver with an alias domain, requiring the resolver to perform a new DNS lookup to procure a record from an authoritative nameserver (often an A record containing an IP address). If the domain cannot be found, it returns an NXDOMAIN message.

## Query Types

There are three types of queries in a DNS system:

### Recursive

In a recursive query, a DNS client requires that a DNS server (typically a DNS recursive resolver) respond with either the requested resource record or an error message if the resolver can't find the record.

### Iterative

In an iterative query, a DNS client provides a hostname, and the DNS Resolver returns the best answer it can. If the DNS resolver has the relevant DNS records in its cache, it returns them. If not, it refers the DNS client to the Root Server or another Authoritative Name Server nearest to the required DNS zone. The DNS client must then repeat the query directly against the DNS server it was referred to.

### Non-recursive

A non-recursive query is one in which the DNS Resolver already knows the answer. It either immediately returns a DNS record because it stores it in a local cache or queries a DNS Name Server that is authoritative for the record. In both cases, there is no need for additional rounds of queries, as a response is returned immediately to the client.

## Record Types

DNS records (also known as zone files) are instructions that live in authoritative DNS servers, providing information about a domain, including its associated IP address and how to handle requests for that domain.

These records consist of a series of text files written in DNS syntax. DNS syntax is simply a string of characters used as commands that tell the DNS server what to do. All DNS records also have a "TTL" (Time-To-Live), which indicates how often a DNS server will refresh that record.

Here are some commonly used record types:

- **A (Address record)**: Holds the IP address of a domain.
- **AAAA (IPv6 Address record)**: Contains the IPv6 address for a domain.
- **CNAME (Canonical Name record)**: Forwards one domain or subdomain to another domain; does NOT provide an IP address.
- **MX (Mail exchanger record)**: Directs mail to an email server.
- **TXT (Text Record)**: Allows an admin to store text notes in the record; often used for email security.
- **NS (Name Server records)**: Stores the name server for a DNS entry.
- **SOA (Start of Authority)**: Stores admin information about a domain.
- **SRV (Service Location record)**: Specifies a port for specific services.
- **PTR (Reverse-lookup Pointer records)**: Provides a domain name in reverse lookups.
- **CERT (Certificate record)**: Stores public key certificates.

## Subdomains

A subdomain is an additional part of the main domain name, commonly used to logically separate a website into sections. Multiple subdomains or child domains can be created on the main domain.

For example, `blog.example.com` where `blog` is the subdomain, `example` is the primary domain, and `.com` is the top-level domain (TLD). Other examples can include `support.example.com` or `careers.example.com`.

## DNS Zones

A DNS zone is a distinct part of the domain namespace that is delegated to a legal entity (like a person, organization, or company) responsible for maintaining the DNS zone. A DNS zone also serves as an administrative function, allowing for granular control of DNS components, such as authoritative name servers.

## DNS Caching

A DNS cache (sometimes called a DNS resolver cache) is a temporary database maintained by a computer's operating system that contains records of all recent visits and attempted visits to websites and other internet domains. In essence, a DNS cache is a memory of recent DNS lookups that the computer can quickly reference when determining how to load a website.

The Domain Name System implements a time-to-live (TTL) on every DNS record. TTL specifies the number of seconds the record can be cached by a DNS client or server. When a record is stored in a cache, the TTL value is also stored. The server updates the TTL of the cached record, counting down every second. When it reaches zero, the record is deleted from the cache. If a query for that record is received at that point, the DNS server must start the resolution process anew.

## Reverse DNS

A reverse DNS lookup is a DNS query for the domain name associated with a given IP address. This process accomplishes the opposite of the more commonly used forward DNS lookup, where the DNS system is queried to return an IP address. Reverse resolving an IP address uses PTR records. If the server does not have a PTR record, it cannot resolve a reverse lookup.

Reverse lookups are commonly used by email servers to check if an email message originated from a valid server before accepting it. Many email servers will reject messages from servers that do not support reverse lookups or from those deemed unlikely to be legitimate.

> **Note:** Reverse DNS lookups are not universally adopted, as they are not critical to the normal functioning of the internet.

## Examples

Here are some widely used managed DNS solutions:

- Route53
- Cloudflare DNS
- Google Cloud DNS
- Azure DNS
- NS1
