### The flow
When the user enters a url like `www.blog.example.com`
#### 1. **Browser → Operating System**
##### ❓ _Browser asks OS:_
“Do you know the IP of `www.blog.example.com`?”
##### 🧠 OS checks:
- Browser cache
- OS DNS cache
- `/etc/hosts` file
If found → OS returns IP to browser → DONE.
If NOT found → OS must ask the DNS resolver.
#### 2. **OS → Recursive Resolver**
This resolver is usually:
- ISP resolver
- 8.8.8.8 (Google)
- 1.1.1.1 (Cloudflare)
##### ❓ OS sends a request:
“Please get me the IP of `www.blog.example.com`.”
##### 🧠 If resolver already has it cached:
- It responds immediately with the IP
- DONE
If NOT cached, resolver must **go out to the internet** and resolve it.
#### 3. **Resolver → Root Servers**
The resolver sends:
> “Where can I find information about `.com` domains?”
##### 🧭 Root server responds:
> “I don’t know the IP of `www.blog.example.com`.  
> But I DO know the servers responsible for `.com`.  
> Here is the list of `.com` TLD name servers.”

This gives the resolver the addresses of the **TLD (.com) servers**.
#### 4. **Resolver → TLD (.com) Servers**
Resolver asks a `.com` server:
> “Where can I find information about `example.com`?”
##### 🧭 `.com` TLD server responds:
> “`example.com` is managed by these name servers:
- `ns1.example.com`
- `ns2.example.com`  
    Also, here are their IP addresses.”
_(These IPs are called **glue records** so the resolver can reach them.)_
Now the resolver knows the **authoritative name servers** for `example.com`.
#### 5. **Resolver → Authoritative Server (ns1.example.com)**
Resolver asks one of them (say ns1):
> “What is the IP of `www.blog.example.com`?”
##### Two possible outcomes:
##### **Case 1: Direct Answer**
Server responds:
> “`www.blog.example.com` = 93.184.216.34  
> TTL = 3600 seconds.”
##### **Case 2: CNAME Chain**
Server responds:
> “I don’t store that name directly.  
> `www.blog.example.com` is actually an alias (CNAME) for `blog.example.com`.”

In this case, resolver will ask AGAIN:
**Resolver → same server:**
> “Okay, then what is the IP of `blog.example.com`?”

Server replies:
> “`blog.example.com` = 93.184.216.34”

Either way, resolver finally gets the IP.
#### 6. **Resolver → OS**
Resolver returns the final answer:
> “Here is the IP: 93.184.216.34  
> Cache it for 3600 seconds.”

OS caches it.
#### 7. **OS → Browser**
OS tells the browser:
> “The IP for `www.blog.example.com` is 93.184.216.34.”
#### 8. **Browser connects to the server**
Browser now does:
> “Make TCP/HTTPS connection → 93.184.216.34”

Done. Page loads.


### Zone files
- Zone files are stored in authoritative server for the domain
##### What do they contain
- **SOA (Start of Authority)** → domain metadata
- **NS records** → authoritative name servers
- **A/AAAA** → IPv4/IPv6 addresses
- **CNAME** → aliases
- **MX** → mail servers
- **TXT** → SPF, DKIM, verification
- **SRV** → service endpoints
- **PTR** → reverse lookup (in reverse zone files only)
Zone files also define:
- TTL (how long records can be cached)
- Basic domain settings (admin email, serial number, refresh times)

```dns
$TTL 3600
@   IN  SOA ns1.example.com. admin.example.com. (
        2025011701 ; Serial
        7200       ; Refresh
        3600       ; Retry
        1209600    ; Expire
        3600       ; Negative TTL
)

; Name servers
    IN  NS  ns1.example.com.
    IN  NS  ns2.example.com.

; A records
@   IN  A    93.184.216.34
www IN  A    93.184.216.34
api IN  A    93.184.216.50

; AAAA record (IPv6)
@   IN  AAAA 2606:2800:220:1:248:1893:25c8:1946

; MX record (mail server)
@    IN  MX   10 mail.example.com.
mail IN  A    93.184.216.40

; CNAME record
blog IN  CNAME www.example.com.

; TXT record (SPF)
@    IN  TXT  "v=spf1 mx -all"

```
#### Explanation
##### Line 1
```dns
$TTL 3600
```
- Default TTL for all records in the file = 1 hour
- Resolver can cache any record for 3600 seconds unless a specific TTL overrides it
##### SOA Block
```dns
@   IN  SOA ns1.example.com. admin.example.com. (
```

`@` 
- represents the current zone = `example.com`

`SOA`
- Identifies this as the Start of Authority Record
- Every zone MUST have exactly one SOA

`ns1.example.com.`
- Primary authoritative DNS server

`admin.example.com.`
- Email of administrator -> `admin@example.com`
- DNS uses dot instead of @

##### SOA Timers
```dns
        2025011701 ; Serial
        7200       ; Refresh
        3600       ; Retry
        1209600    ; Expire
        3600       ; Negative TTL
```

`Serial`
- serial number of this zone file
- changes every time you edit the file

`Refresh`
- Secondary DNS servers check every 2 hours for update

`Retry`
- If refresh query fails, retry after 1 hour

`Expire`
- If secondary can't reach primary for 14 days, it stops serving the zone

`Negative TTL`
- How long on NXDOMAIN response can be cached

##### NS Records
```dns
    IN  NS  ns1.example.com.
    IN  NS  ns2.example.com.
```
- This means, the authoritative name servers for example.com are ns1 and ns2

##### A Records
`@   IN  A    93.184.216.34`
- example.com → IPv4 address

`www IN  A    93.184.216.34`
- www.example.com → same server

`api IN  A    93.184.216.50`
- api.example.com → different IP

##### AAAA Records
`@   IN  AAAA 2606:2800:220:1:248:1893:25c8:1946`
- IPv6 address for `example.com`

##### MX Record
`@    IN  MX   10 mail.example.com.`
- Mail for `example.com` should be delivered to `mail.example.com`
- lower number = higher priority

`mail IN A   93.184.216.40`
- IP Address of the mail server

##### CNAME Record
`blog IN  CNAME www.example.com.`
- `blog.example.com` is an alias of `www.example.com`

##### TXT Record
`@    IN  TXT  "v=spf1 mx -all"`
- Only the MX server is allowed to send email for this domain

### DNS Cache Poisoning
##### **Step 1: Victim user triggers the DNS query**
User types:
`bank.com`

Their device asks the **DNS resolver** (e.g., the ISP DNS):
`What is the IP of bank.com?`

Resolver doesn't have the answer → must query the authoritative server.
##### **Step 2: Attacker sends a flood of fake responses**
Before the resolver gets the _real_ response from the authoritative server, attacker tries to guess:
- **Transaction ID**
- **Port number**    
- **Question asked**

and sends fake replies like:
`bank.com → 6.6.6.6  (attacker IP) TTL = 86400 (cache for 24 hrs)`

The attacker may send thousands of these **spoofed DNS responses**.
##### **Step 3: Resolver receives a fake answer first**
If **one** of the attacker’s forged replies matches the correct transaction ID + port number (in modern DNS, harder but still possible with weaknesses), the resolver **accepts** it.

Resolver stores in its cache:
`bank.com → 6.6.6.6 TTL: 24 hours`

The real DNS answer may arrive milliseconds later…
But it's **ignored** because the query is already considered resolved.
##### **Step 4: The cached fake answer is given to users**
Now **every user** asking the resolver for `bank.com` will get:
`6.6.6.6`
They are silently redirected to:
- Phishing login page
- Malware hosting page
- Fake payment portal
- Data harvesting site

#### How to prevent DNS Cache Poisoning
##### ✔ Randomized Transaction IDs
Harder to guess
##### ✔ Randomized UDP source ports
Doubles randomness
##### ✔ DNSSEC
Adds **digital signatures** to DNS records  
Resolver rejects any unsigned or tampered answers.
##### ✔ Query minimization & rate limiting
Reduce attacker’s chances to trigger many queries
##### ✔ Disable recursion for outsiders
Only internal users can ask the resolver
##### ✔ Split-horizon DNS
Different DNS for inside/outside
