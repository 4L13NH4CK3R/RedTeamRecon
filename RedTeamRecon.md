# Read Team Recon  
During this "TryHackMe" challenge, we will discover some amazing techniques & tools that we can utilize in order to scan our target and gather as much information as we can about them!  
  
## Introduction  
“Know your enemy, know his sword.” wrote Miyamoto Musashi in his book, [A Book of Five Rings: The Classic Guide to Strategy](https://www.holybooks.com/wp-content/uploads/The-Book-of-Five-Rings-by-Musashi-Miyamoto.pdf).  
He also wrote, “You win battles by knowing the enemy’s timing, and using a timing which the enemy does not expect.” Although this was written when swords and spears won battles, it also applies to cyberspace,  
where attacks are launched via keyboards and crafted packets. The more you know about your target’s infrastructure and personnel, the better you can orchestrate your attacks.  
  
In a red team operation, you might start with no more than a company name, from which you need to start gathering information about the target.  
This is where reconnaissance comes into play. Reconnaissance (recon) can be defined as a preliminary survey or observation of your target (client)  
without alerting them to your activities. If your recon activities create too much noise, the other party would be alerted, which might decrease the likelihood of your success.  
  
The tasks of this room cover the following topics:  
* Types of Reconnaissance Activities  
* WHOIS and DNS-Based Reconnaissance  
* Advanced Searchings  
* Searching by Image  
* Google Hacking  
* Specialized Search Engines  
* Recon-NG  
* Maltego  
![Recon](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/805d899efad693ba745b02a2dd485b74.jpg)  
  
Some specific objectives we'll cover include:  
* Discovering Subdomains related to our target company  
* Gathering publicly available information about a host and IP Address  
* Finding Email Address related to our Target  
* Discovering Login Credentials and Leaked Passwords  
* Locating Leaked Documents & Spreadsheets  
  
Reconnaissance can be broken down into two parts — passive reconnaissance and active reconnaissance, as explained in Task 2. In this room, we will be focusing on passive reconnaissance,  
i.e., techniques that don’t alert the target or create 'noise'. In later rooms, we will use active reconnaissance tools that tend to be noisy by nature.  
  
  
## Taxonomy of Reconnaissance  
Reconnaissance (recon) can be classified into two parts:  
* **Passive Recon:** *Can be carried out by wathing passively!*  
* **Active Recon:** *Requires interacting with the target to provoke it in order to observe its response!*  
  
Passive recon doesn't require interacting with the target. In other words, you aren't sending any packets or requests to the target or the systems your target owns.  
Instead, passive recon relies on publicly available information that is collected and maintained by a third party. Open Source Intelligence (OSINT) is used to collect information  
about the target and can be as simple as viewing a target's publicly available social media profile. Example information that we might collect includes domain names,  
IP address blocks, email addresses, employee names, and job posts. In the upcoming task, we'll see how to query DNS records and expand on the topics  
from the [Passive Reconnaissance](https://tryhackme.com/room/passiverecon) room and introduce advanced tooling to aid in your recon.  
  
Active recon requires interacting with the target by sending requests and packets and observing if and how it responds. The responses collected - or lack of responses -  
would enable us to expand on the picture we started developing using passive recon. An example of active reconnaissance is using Nmap to scan target subnets and live hosts.  
Other examples can be found in the [Active Reconnaissance](https://tryhackme.com/room/activerecon) room. Some information that we would want to discover include live hosts, running servers, listening services, and version numbers.  
  
Active recon can be classified as:  
* **External Recon:** *Conducted outside of the target's network and focuses on externally facing assets assessable from the Internet. One example is running the Nikto from outside of the company network.*  
* **Internal Recon:** *Conducted from within the target company's network. In other words, the pentester or red teamer might be physically located inside of the companies building.  In this scenario, they might be using an exploited host on the target's network. An example would be using Nessus to scan the internal network using one of the target's computers.*
  
  
## Built-In Tools  
This task focuses on:  
* **whois**  
* **Dig**  
* **NSLookUp**  
* **Host**  
* **Traceroute**  
* **Tracert**  
  
Before we start using the whois tool, let's look at WHOIS. WHOIS is a request and response protocol that follows the RFC 3912 specification. A WHOIS server listens on TCP port 43 for incoming requests.  
The domain registrar is responsible for maintaining the WHOIS records for the domain names it is leasing. whois will query the WHOIS server to provide all saved records. In the following example, we can see whois provides us with:  
* **Registrar** *WHOIS Server*  
* **Registrar** *URL*  
* **Record** *Creation Date*  
* **Record** *Updated Date*  
* **Registrant** *Contact Intel*  
* **Admin** *Contact Intel*  
* **Tech** *Contact Intel*  
  
```
$ whois thmredteam.com  
   Domain Name: THMREDTEAM.COM
   Registry Domain ID: 2643258257_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.namecheap.com
   Registrar URL: http://www.namecheap.com
   Updated Date: 2022-09-26T15:22:32Z
   Creation Date: 2021-09-24T14:04:16Z
   Registry Expiry Date: 2023-09-24T14:04:16Z
   Registrar: NameCheap, Inc.
   Registrar IANA ID: 1068
   Registrar Abuse Contact Email: abuse@namecheap.com
   Registrar Abuse Contact Phone: +1.6613102107
   Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
   Name Server: KIP.NS.CLOUDFLARE.COM
   Name Server: UMA.NS.CLOUDFLARE.COM
   DNSSEC: unsigned
   URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
>>> Last update of whois database: 2022-12-29T20:29:55Z <<<

For more information on Whois status codes, please visit https://icann.org/epp

NOTICE: The expiration date displayed in this record is the date the
registrar's sponsorship of the domain name registration in the registry is
currently set to expire. This date does not necessarily reflect the expiration
date of the domain name registrant's agreement with the sponsoring
registrar.  Users may consult the sponsoring registrar's Whois database to
view the registrar's reported date of expiration for this registration.

TERMS OF USE: You are not authorized to access or query our Whois
database through the use of electronic processes that are high-volume and
automated except as reasonably necessary to register domain names or
modify existing registrations; the Data in VeriSign Global Registry
Services' ("VeriSign") Whois database is provided by VeriSign for
information purposes only, and to assist persons in obtaining information
about or related to a domain name registration record. VeriSign does not
guarantee its accuracy. By submitting a Whois query, you agree to abide
by the following terms of use: You agree that you may use this Data only
for lawful purposes and that under no circumstances will you use this Data
to: (1) allow, enable, or otherwise support the transmission of mass
unsolicited, commercial advertising or solicitations via e-mail, telephone,
or facsimile; or (2) enable high volume, automated, electronic processes
that apply to VeriSign (or its computer systems). The compilation,
repackaging, dissemination or other use of this Data is expressly
prohibited without the prior written consent of VeriSign. You agree not to
use electronic processes that are automated and high-volume to access or
query the Whois database except as reasonably necessary to register
domain names or modify existing registrations. VeriSign reserves the right
to restrict your access to the Whois database in its sole discretion to ensure
operational stability.  VeriSign may restrict or terminate your access to the
Whois database for failure to abide by these terms of use. VeriSign
reserves the right to modify these terms at any time.

The Registry database contains ONLY .COM, .NET, .EDU domains and
Registrars.
Domain name: thmredteam.com
Registry Domain ID: 2643258257_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.namecheap.com
Registrar URL: http://www.namecheap.com
Updated Date: 2022-09-26T15:22:32.57Z
Creation Date: 2021-09-24T14:04:16.00Z
Registrar Registration Expiration Date: 2023-09-24T14:04:16.00Z
Registrar: NAMECHEAP INC
Registrar IANA ID: 1068
Registrar Abuse Contact Email: abuse@namecheap.com
Registrar Abuse Contact Phone: +1.9854014545
Reseller: NAMECHEAP INC
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Registry Registrant ID: 
Registrant Name: Redacted for Privacy
Registrant Organization: Privacy service provided by Withheld for Privacy ehf
Registrant Street: Kalkofnsvegur 2 
Registrant City: Reykjavik
Registrant State/Province: Capital Region
Registrant Postal Code: 101
Registrant Country: IS
Registrant Phone: +354.4212434
Registrant Phone Ext: 
Registrant Fax: 
Registrant Fax Ext: 
Registrant Email: e17b7976233e4e72a76b3dadb1d574bd.protect@withheldforprivacy.com
Registry Admin ID: 
Admin Name: Redacted for Privacy
Admin Organization: Privacy service provided by Withheld for Privacy ehf
Admin Street: Kalkofnsvegur 2 
Admin City: Reykjavik
Admin State/Province: Capital Region
Admin Postal Code: 101
Admin Country: IS
Admin Phone: +354.4212434
Admin Phone Ext: 
Admin Fax: 
Admin Fax Ext: 
Admin Email: e17b7976233e4e72a76b3dadb1d574bd.protect@withheldforprivacy.com
Registry Tech ID: 
Tech Name: Redacted for Privacy
Tech Organization: Privacy service provided by Withheld for Privacy ehf
Tech Street: Kalkofnsvegur 2 
Tech City: Reykjavik
Tech State/Province: Capital Region
Tech Postal Code: 101
Tech Country: IS
Tech Phone: +354.4212434
Tech Phone Ext: 
Tech Fax: 
Tech Fax Ext: 
Tech Email: e17b7976233e4e72a76b3dadb1d574bd.protect@withheldforprivacy.com
Name Server: kip.ns.cloudflare.com
Name Server: uma.ns.cloudflare.com
DNSSEC: unsigned
URL of the ICANN WHOIS Data Problem Reporting System: http://wdprs.internic.net/
>>> Last update of WHOIS database: 2022-12-29T17:30:14.59Z <<<
For more information on Whois status codes, please visit https://icann.org/epp
  
```
As we can see above, it is possible to gain a lot of valuable information with only a domain name. After a whois lookup, we might get lucky and find names, email addresses, postal addresses, and phone numbers,  
in addition to other technical information. At the end of the whois query, we find the authoritative name servers for the domain in question.  
  
DNS queries can be executed with many different tools found on our systems, especially Unix-like systems. One common tool found on Unix-like systems, Windows, and macOS is nslookup.  
In the following query, we can see how nslookup uses the default DNS server to get the A and AAAA records related to our domain.  
``` 
$ nslookup cafe.thmredteam.com  
Server:         192.168.1.1
Address:        192.168.1.1#53

Non-authoritative answer:
Name:   cafe.thmredteam.com
Address: 172.67.212.249
Name:   cafe.thmredteam.com
Address: 104.21.93.169
Name:   cafe.thmredteam.com
Address: 2606:4700:3034::ac43:d4f9
Name:   cafe.thmredteam.com
Address: 2606:4700:3034::6815:5da9
```
  
Another tool commonly found on Unix-like systems is dig, short for Domain Information Groper (dig). dig provides a lot of query options and even allows you to specify a different DNS server to use.  
For example, we can use Cloudflare's DNS server: dig @1.1.1.1 tryhackme.com.  
``` 
$ dig cafe.thmredteam.com @1.1.1.1  
; <<>> DiG 9.18.8-1-Debian <<>> cafe.thmredteam.com @1.1.1.1
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 59416
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;cafe.thmredteam.com.           IN      A

;; ANSWER SECTION:
cafe.thmredteam.com.    300     IN      A       172.67.212.249
cafe.thmredteam.com.    300     IN      A       104.21.93.169

;; Query time: 35 msec
;; SERVER: 1.1.1.1#53(1.1.1.1) (UDP)
;; WHEN: Thu Dec 29 15:32:47 EST 2022
;; MSG SIZE  rcvd: 80
```
  
host is another useful alternative for querying DNS servers for DNS records. Consider the following example.  
```
$ host cafe.thmredteam.com  
cafe.thmredteam.com has address 104.21.93.169
cafe.thmredteam.com has address 172.67.212.249
cafe.thmredteam.com has IPv6 address 2606:4700:3034::6815:5da9
cafe.thmredteam.com has IPv6 address 2606:4700:3034::ac43:d4f9
```
  
The final tool that ships with Unix-like systems is traceroute, or on MS Windows systems, tracert. As the name indicates, it traces the route taken by the packets from our system to the target host.  
The console output below shows that traceroute provided us with the routers (hops) connecting us to the target system. It's worth stressing that some routers don’t respond to the packets sent by traceroute,  
and as a result, we don’t see their IP addresses; a * is used to indicate such a case.  
```
$ traceroute cafe.thmredteam.com  
traceroute to cafe.thmredteam.com (172.67.212.249), 30 hops max, 60 byte packets
 1  10.0.0.1 (10.0.0.1)  0.111 ms  0.094 ms  0.086 ms
 2  * * *
 3  * * *
 4  * * *
 5  * * *
 6  * * *
 7  * * *
 8  * * *
 9  * * *
10  * * *
11  * * *
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  * * *
23  * * *
24  * * *
25  * * *
26  * * *
27  * * *
28  * * *
29  * * *
30  * * *
```
  
In summary, we can always rely on:  
* **WHOIS** *to query the WHOIS Database*  
* **NSLookUp** **DIG** **HOST** *to query the DNS Servers*  

WHOIS databases and DNS servers hold publicly available information, and querying either does not generate any suspicious traffic.  
  
Moreover, we can rely on Traceroute (traceroute on Linux and macOS systems and tracert on MS Windows systems) to discover the hops between our system and the target host.  
  
*When was "thmredteam.com" created (registered)? (YYYY-MM-DD)* **2021-09-24**  
*To how many IPv4 Addresses does "clinic.thmredteam.com" resolve?* **2**  
*To how many IPv6 addresses does "clinic.thmredteam.com" resolve?* **2**  
  
  
## Advanced Searching  
Being able to use a search engine efficiently is a crucial skill. The following table shows some popular search modifiers that work with many popular search engines.
Symbol / Syntax 	Function  
"search phrase" 	Find results with exact search phrase  
OSINT filetype:pdf 	Find files of type PDF related to a certain term.  
salary site:blog.tryhackme.com 	Limit search results to a specific site.  
pentest -site:example.com 	Exclude a specific site from results  
walkthrough intitle:TryHackMe 	Find pages with a specific term in the page title.  
challenge inurl:tryhackme 	Find pages with a specific term in the page URL.  
  
Note: In addition to pdf, other filetypes to consider are: doc, docx, ppt, pptx, xls and xlsx.  

Each search engine might have a slightly varied set of rules and syntax. To learn about the specific syntax for the different search engines, you will need to visit their respective help pages.  
Some search engines, such as Google, provide a web interface for advanced searches: [Google Advanced Search](https://www.google.com/advanced_search). Other times, it is best to learn the syntax by heart, such as [Google Refine Web Searches](https://support.google.com/websearch/answer/2466433),  
[DuckDuckGo Search Syntax](https://help.duckduckgo.com/duckduckgo-help-pages/results/syntax/), and [Bing Advanced Search Options](https://help.bing.microsoft.com/apex/index/18/en-US/10002).  
  
Search engines crawl the world wide web day and night to index new web pages and files. Sometimes this can lead to indexing confidential information. Examples of confidential information include:  
* Documents for internal company use  
* Confidential spreadsheets with usernames, email address, and even passwords  
* Files containing usernames  
* Sensitive Directories  
* Service version number (some of which might be vulnerable and unpatched)  
* Error Messages  
  
Combining advanced Google searches with specific terms, documents containing sensitive information or vulnerable web servers can be found. Websites such as [Google Hacking Database](https://www.exploit-db.com/google-hacking-database) (GHDB)  
collect such search terms and are publicly available. Let's take a look at some of the GHDB queries to see if our client has any confidential information exposed via search engines.  
GHDB contains queries under the following categories:  
* **Footholds**  
*Consider [GHDB-ID: 6364](https://www.exploit-db.com/ghdb/6364) as it uses the query intitle:"index of" "nginx.log" to discover Nginx logs and might reveal server misconfigurations that can be exploited.*  
* **Files Containing Usernames**  
*For example, [GHDB-ID: 7047](https://www.exploit-db.com/ghdb/7047) uses the search term intitle:"index of" "contacts.txt" to discover files that leak juicy information.*  
* **Sensitive Directories**  
*For example, consider [GHDB-ID: 6768](https://www.exploit-db.com/ghdb/6768), which uses the search term inurl:/certs/server.key to find out if a private RSA key is exposed.*  
* **Web Server Detection**  
*Consider [GHDB-ID: 6876](https://www.exploit-db.com/ghdb/6876), which detects GlassFish Server information using the query intitle:"GlassFish Server - Server Running".*  
* **Vulnerable Files**  
*For example, we can try to locate PHP files using the query intitle:"index of" "* *.php", as provided by [GHDB-ID: 7786](https://www.exploit-db.com/ghdb/7786).*  
* **Vulnerable Servers**  
*For instance, to discover SolarWinds Orion web consoles, [GHDB-ID: 6728](https://www.exploit-db.com/ghdb/6728) uses the query intext:"user name" intext:"orion core" -solarwinds.com.*  
* **Error Messages**  
*Plenty of useful information can be extracted from error messages. One example is [GHDB-ID: 5963](https://www.exploit-db.com/ghdb/5963), which uses the query intitle:"index of" errors.log to find log files related to errors.*  
  
You might need to adapt these Google queries to fit your needs as the queries will return results from all web servers that fit the criteria and were indexed. To avoid legal issues, it is best to refrain from accessing  
any files outside the scope of your legal agreement.  
  
We recommend you join the Google Dorking room for more in-depth information.  
  
Now we'll explore two additional sources that can provide valuable information without interacting with our target:  
* Social Media  
* Job Ads  
![Social Media & Job Ads](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/30745e7e6b4d48c0f2a7774fd79a908d.jpg)  
  
***Social Media***  
  
Social media websites have become very popular for not only personal use but also for corporate use. Some social media platforms can reveal tons of information about the target.  
This is especially true as many users tend to overshare details about themselves and their work. To name a few, it's worthwhile checking the following:  
* [LinkedIn](https://www.linkedin.com/)  
* [Twitter](https://www.twitter.com/)  
* [Facebook](https://www.facebook.com/)  
* [Instagram](https://www.instagram.com/)  
  
Social media websites make it easy to collect the names of a given company's employees; moreover, in certain instances, you might learn specific pieces of information that  
can reveal answers to password recovery questions or gain ideas to include in a targeted wordlist. Posts from technical staff might reveal details about a company’s systems  
and vendors. For example, a network engineer who was recently issued Juniper certifications may allude to Juniper networking infrastructure being used in their employer’s environment.  
![Vacant Job Ads](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/f24355586bbc8c897b6af23cca84f60e.jpg)  
  
***Job Ads***  
Job advertisements can also tell you a lot about a company. In addition to revealing names and email addresses, job posts for technical positions could give insight into the target company’s  
systems and infrastructure. The popular job posts might vary from one country to another. Make sure to check job listing sites in the countries where your client would post their ads. Moreover,  
it is always worth checking their website for any job opening and seeing if this can leak any interesting information.  
  
Note that the [Wayback Machine](https://archive.org/web/) can be helpful to retrieve previous versions of a job opening page on your client’s site.  
  
*How would you search using Google for "xls" indexed for http://clinic.thmredteam.com?* **filetype:xls site:clinic.thmredteam.com**  
*How would you search using Google for files with the word "passwords" for http://clinic.thmredteam.com?* **passwords site:clinic.thmredteam.com**  
  
  
## Specialized Search Engines  
***WHOIS & DNS Related***  
Beyond the standard WHOIS and DNS query tools that we covered in Task 3, there are third parties that offer paid services for historical WHOIS data. One example is WHOIS history,  
which provides a history of WHOIS data and can come in handy if the domain registrant didn’t use WHOIS privacy when they registered the domain.  
  
There are a handful of websites that offer advanced DNS services that are free to use. Some of these websites offer rich functionality and could have a complete  
room dedicated to exploring one domain. For now, we'll focus on key DNS related aspects. We will consider the following:  
* [ViewDNS.info](https://viewdns.info/)  
* [Threat Intelligence Platform](https://threatintelligenceplatform.com/)  
  
***ViewDNS.info***

[ViewDNS.info](https://viewdns.info/) offers Reverse IP Lookup. Initially, each web server would use one or more IP addresses; however, today, it is common to come across shared hosting servers.  
With shared hosting, one IP address is shared among many different web servers with different domain names. With reverse IP lookup, starting from a domain name or an IP address, you can find the other  
domain names using a specific IP address(es).  
  
In the figure below, we used reverse IP lookup to find other servers sharing the same IP addresses used by cafe.thmredteam.com. Therefore, it is important to note that knowing the IP address  
does not necessarily lead to a single website.  
![ViewDNS.info](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/a6a57e946b742bf9439430c1669382a5.png)  
  
***Threat Intelligence Platform***  
[Threat Intelligence Platform](https://threatintelligenceplatform.com/) requires you to provide a domain name or an IP address, and it will launch a series of tests from malware checks to WHOIS  
and DNS queries. The WHOIS and DNS results are similar to the results we would get using whois and dig, but Threat Intelligence Platform presents them in  
a more readable and visually appealing way. There is extra information that we get with our report. For instance, after we look up thmredteam.com, we see that  
Name Server (NS) records were resolved to their respective IPv4 and IPv6 addresses, as shown in the figure below.  
![Threat Intelligence Platform](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/8ba0fec5aad89cfb7c3d242ed92c9da4.png)  
  
On the other hand, when we searched for cafe.thmredteam.com, we could also get a list of other domains on the same IP address. The result we see in the figure below is similar to the results we obtained using [VidewDNS.info](https://viewdns.info/).  
![Other Domains on IP](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/c920bcf224185c47c4ba54b300079e48.png)  
  
***Specialized Search Engines***  
[Censys Search](https://search.censys.io/) can provide a lot of information about IP addresses and domains. In this example, we look up one of the IP addresses that cafe.thmredteam.com resolves to.  
We can easily infer that the IP address we looked up belongs to Cloudflare. We can see information related to ports 80 and 443, among others; however, it's clear that this IP address is used to server  
websites other than cafe.thmredteam.com. In other words, this IP address belongs to a company other than our client, [Organic Cafe](https://cafe.thmredteam.com/). It's critical to make this distinction so that we  
don’t probe systems outside the scope of our contract.  
![Censys](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/efc8f98cc9e721707d4bad477340e120.png)  
  
***Shodan***  
You might remember using [Shodan](https://www.shodan.io/) in the [Passive Reconnaissance](https://tryhackme.com/room/passiverecon) room. In this section, we will demonstrate how to use Shodan from the command line.  

To use Shodan from the command-line properly, you need to create an account with [Shodan](https://account.shodan.io/register), then configure shodan to use your API key using the command, shodan init API_KEY.

You can use different filters depending on the [type of your Shodan account](https://account.shodan.io/billing). To learn more about what you can do with shodan, we suggest that you check out Shodan CLI.  
Let’s demonstrate a simple example of looking up information about one of the IP addresses we got from nslookup cafe.thmredteam.com. Using shodan host IP_ADDRESS, we can get the  
geographical location of the IP address and the open ports, as shown below.  
```
**On Kali, Add Your API Key Like So** 
$ shodan init YOURAPIKEYHERE  
  
$ shodan host 172.67.212.249  
Hostnames:               toplevelcounseling.com;sni.cloudflaressl.com
City:                    San Francisco
Country:                 United States
Organization:            Cloudflare, Inc.
Updated:                 2022-12-29T19:15:35.677157
Number of open ports:    9

Ports:
     80/tcp  
    443/tcp  
        |-- SSL Versions: -SSLv2, -SSLv3, TLSv1, TLSv1.1, TLSv1.2, TLSv1.3
   2053/tcp  
   2082/tcp  
   2083/tcp  
   2086/tcp  
   2087/tcp  
   8443/tcp CloudFlare 
   8880/tcp  
```
  
*What is the "shodan" command to get your Internet-Facing IP Address?* **shodan myip**  
  
  
## Recon-NG  
[Recon-NG](https://github.com/lanmaster53/recon-ng) is a framework that helps automate the OSINT work. It uses modules from various authors and provides a multitude of functionality.  
Some modules require keys to work; the key allows the module to query the related online API. In this task, we will demonstrate using Recon-ng in the terminal.  
  
From a penetration testing and red team point of view, Recon-ng can be used to find various bits and pieces of information that can aid in an operation or OSINT task. All the data collected  
is automatically saved in the database related to your workspace. For instance, you might discover host addresses to later port-scan or collect contact email addresses for phishing attacks.  
  
You can start Recon-ng by running the command recon-ng. Starting Recon-ng will give you a prompt like [recon-ng][default] >. At this stage, you need to select the installed  
module you want to use. However, if this is the first time you're running recon-ng, you will need to install the module(s) you need.  
  
In this task, we will follow the following workflow:  
* Create a workspace for your project  
* Instert the starting information into the database  
* Search the marketplace for a module and learn about it before installing  
* List the installed modules and load one  
* Run the loaded module  
  
***Creating a Workspace***  
Run workspaces create WORKSPACE_NAME to create a new workspace for your investigation. For example, workspaces create thmredteam will create a workspace named thmredteam.
  
```
$ recon-ng -w CryptoH4ck3r  

```
starts recon-ng with the specific workspace.   
  
***Seeding the Database***  
In reconnaissance, you are starting with one piece of information and transforming it into new pieces of information. For instance, you might start your research with a  
company name and use that to discover the domain name(s), contacts and profiles. Then you would use the new information you obtained to transform it further and learn more about your target.  
  
Let’s consider the case where we know the target's domain name, thmredteam.com, and we would like to feed it into the Recon-ng database related to the active workspace.  
If we want to check the names of the tables in our database, we can run db schema.  
  
We want to insert the domain name thmredteam.com into the domains table. We can do this using the command db insert domains  
  
```
$ recon-ng -w CryptoH4ck3r  
[*] Version check disabled.

    _/_/_/    _/_/_/_/    _/_/_/    _/_/_/    _/      _/            _/      _/    _/_/_/
   _/    _/  _/        _/        _/      _/  _/_/    _/            _/_/    _/  _/       
  _/_/_/    _/_/_/    _/        _/      _/  _/  _/  _/  _/_/_/_/  _/  _/  _/  _/  _/_/_/
 _/    _/  _/        _/        _/      _/  _/    _/_/            _/    _/_/  _/      _/ 
_/    _/  _/_/_/_/    _/_/_/    _/_/_/    _/      _/            _/      _/    _/_/_/    


                                          /\
                                         / \\ /\
    Sponsored by...               /\  /\/  \\V  \/\
                                 / \\/ // \\\\\ \\ \/\
                                // // BLACK HILLS \/ \\
                               www.blackhillsinfosec.com

                  ____   ____   ____   ____ _____ _  ____   ____  ____
                 |____] | ___/ |____| |       |   | |____  |____ |
                 |      |   \_ |    | |____   |   |  ____| |____ |____
                                   www.practisec.com

                      [recon-ng v5.1.2, Tim Tomes (@lanmaster53)]                       

[*] No modules enabled/installed.
  
[recon-ng][CryptoH4ck3r] > db insert domains
domain (TEXT): thmredteam.com
notes (TEXT): 
[*] 1 rows affected.
```
  
***Recon-NG Marketplace***  
We have a domain name, so a logical next step would be to search for a module that transforms domains into other types of information. Assuming we are starting from a fresh  
installation of Recon-ng, we will search for suitable modules from the marketplace.  
  
Before you install modules using the marketplace, these are some useful commands related to marketplace usage:  
* Marketplace search **KEYWORD** ~ This will allow you to search the marketplace for available modules on a specific _Keyword_.  
* Marketplace info **MODULE** ~ This will provide you with the information about the _module_ in question.  
* Marketplace install **MODULE** ~ Here we have the ability to install the desired _module_ in our Recon-NG.  
* Marketplace remove **MODULE** ~ Remove the _module_ from our Recon-NG.  
  
The modules are grouped under multiple categories, such as discovery, import, recon and reporting. Moreover, recon is also divided into  
many subcategories depending on the transform type. Run marketplace search to get a list of all available modules.  
  
In the terminal below, we search for modules containing domains-  
```
[recon-ng][CryptoH4ck3r] > marketplace search domains-  
[*] Searching module index for 'domains-'...

  +---------------------------------------------------------------------------------------------------+
  |                        Path                        | Version |     Status    |  Updated   | D | K |
  +---------------------------------------------------------------------------------------------------+
  | recon/domains-companies/censys_companies           | 2.0     | not installed | 2021-05-10 | * | * |
  | recon/domains-companies/pen                        | 1.1     | not installed | 2019-10-15 |   |   |
  | recon/domains-companies/whoxy_whois                | 1.1     | not installed | 2020-06-24 |   | * |
  | recon/domains-contacts/hunter_io                   | 1.3     | not installed | 2020-04-14 |   | * |
  | recon/domains-contacts/metacrawler                 | 1.1     | not installed | 2019-06-24 | * |   |
  | recon/domains-contacts/pen                         | 1.1     | not installed | 2019-10-15 |   |   |
  | recon/domains-contacts/pgp_search                  | 1.4     | not installed | 2019-10-16 |   |   |
  | recon/domains-contacts/whois_pocs                  | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-contacts/wikileaker                  | 1.0     | not installed | 2020-04-08 |   |   |
  | recon/domains-credentials/pwnedlist/account_creds  | 1.0     | not installed | 2019-06-24 | * | * |
  | recon/domains-credentials/pwnedlist/api_usage      | 1.0     | not installed | 2019-06-24 |   | * |
  | recon/domains-credentials/pwnedlist/domain_creds   | 1.0     | not installed | 2019-06-24 | * | * |
  | recon/domains-credentials/pwnedlist/domain_ispwned | 1.0     | not installed | 2019-06-24 |   | * |
  | recon/domains-credentials/pwnedlist/leak_lookup    | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-credentials/pwnedlist/leaks_dump     | 1.0     | not installed | 2019-06-24 |   | * |
  | recon/domains-domains/brute_suffix                 | 1.1     | not installed | 2020-05-17 |   |   |
  | recon/domains-hosts/binaryedge                     | 1.2     | not installed | 2020-06-18 |   | * |
  | recon/domains-hosts/bing_domain_api                | 1.0     | not installed | 2019-06-24 |   | * |
  | recon/domains-hosts/bing_domain_web                | 1.1     | not installed | 2019-07-04 |   |   |
  | recon/domains-hosts/brute_hosts                    | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-hosts/builtwith                      | 1.1     | not installed | 2021-08-24 |   | * |
  | recon/domains-hosts/censys_domain                  | 2.0     | not installed | 2021-05-10 | * | * |
  | recon/domains-hosts/certificate_transparency       | 1.2     | not installed | 2019-09-16 |   |   |
  | recon/domains-hosts/google_site_web                | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-hosts/hackertarget                   | 1.1     | not installed | 2020-05-17 |   |   |
  | recon/domains-hosts/mx_spf_ip                      | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-hosts/netcraft                       | 1.1     | not installed | 2020-02-05 |   |   |
  | recon/domains-hosts/shodan_hostname                | 1.1     | not installed | 2020-07-01 | * | * |
  | recon/domains-hosts/spyse_subdomains               | 1.1     | not installed | 2021-08-24 |   | * |
  | recon/domains-hosts/ssl_san                        | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-hosts/threatcrowd                    | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-hosts/threatminer                    | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-vulnerabilities/ghdb                 | 1.1     | not installed | 2019-06-26 |   |   |
  | recon/domains-vulnerabilities/xssed                | 1.1     | not installed | 2020-10-18 |   |   |
  +---------------------------------------------------------------------------------------------------+

  D = Has dependencies. See info for details.
  K = Requires keys. See info for details.
```
We notice many subcategories under recon, such as domains-companies, domains-contacts, and domains-hosts. This naming tells us what kind of new information  
we will get from that transformation. For instance, domains-hosts means that the module will find hosts related to the provided domain.  
  
Some modules, like whoxy_whois, require a key, as we can tell from the * under the K column. This requirement indicates that this module is not usable unless we have a key to use the related service.  
  
Other modules have dependencies, indicated by a * under the D column. Dependencies show that third-party Python libraries might be necessary to use the related module.  
  
Let’s say that you are interested in recon/domains-hosts/google_site_web. To learn more about any particular module, you can use the command marketplace info MODULE;  
this is an essential command that explains what the module does. For example, marketplace info google_site_web provides the following description: “Harvests hosts from  
Google.com by using the ‘site’ search operator. Updates the ‘hosts’ table with the results.” In other words, this module will use the Google search engine and the “site” operator.  
  
***Working With Installed Modules***  
We can work with modules using:  
* Modules _Search_ ~ This will get us a list of the installed modules.  
* Modules _load_ **MODULE** ~ We can load a specific module into the memory.  
  
Let’s load the module that we installed earlier from the marketplace, modules load viewdns_reverse_whois. To run it, we need to set the required options.  
  
* options list ~ This will list the options that we have set for the loaded module.  
* options set <option> <value> ~ This will allow us to set the value of our options.  
  
In a previous step, we have installed the module google_site_web, so let’s load it using load google_site_web  
and run it with run. We have already added the domain thmredteam.com to the database, so when the module is run,  
it will read that value from the database, get new kinds of information, and add them to the database in turn.  
The commands and the results are shown in the terminal output below.  
```
[recon-ng][CryptoH4c3r]> marketplace install google_site_web  
[recon-ng][CryptoH4ck3r] > load google_site_web  
[recon-ng][CryptoH4ck3r][google_site_web] > run  
--------------
THMREDTEAM.COM
--------------
[*] Searching Google for: site:thmredteam.com
[*] Country: None
[*] Host: cafe.thmredteam.com
[*] Ip_Address: None
[*] Latitude: None
[*] Longitude: None
[*] Notes: None
[*] Region: None
[*] --------------------------------------------------
[*] Country: None
[*] Host: clinic.thmredteam.com
[*] Ip_Address: None
[*] Latitude: None
[*] Longitude: None
[*] Notes: None
[*] Region: None
[*] --------------------------------------------------
[*] Searching Google for: site:thmredteam.com -site:cafe.thmredteam.com -site:clinic.thmredteam.com
[*] No New Subdomains Found on the Current Page. Jumping to Result 201.
[*] Searching Google for: site:thmredteam.com -site:cafe.thmredteam.com -site:clinic.thmredteam.com
[*] No New Subdomains Found on the Current Page. Jumping to Result 301.
[*] Searching Google for: site:thmredteam.com -site:cafe.thmredteam.com -site:clinic.thmredteam.com
[*] No New Subdomains Found on the Current Page. Jumping to Result 401.
[*] Searching Google for: site:thmredteam.com -site:cafe.thmredteam.com -site:clinic.thmredteam.com
[*] No New Subdomains Found on the Current Page. Jumping to Result 501.
[*] Searching Google for: site:thmredteam.com -site:cafe.thmredteam.com -site:clinic.thmredteam.com
```
  
This module has queried Google and discovered two hosts, cafe.thmredteam.com and clinic.thmredteam.com. It is possible that by the time you run these steps, new hosts will also appear.  
  
***Keys***  
Some modules cannot be used without a key for the respective service API. K indicates that you need to provide the relevant service key to use the module in question.

    keys list lists the keys
    keys add KEY_NAME KEY_VALUE adds a key
    keys remove KEY_NAME removes a key

Once you have the set of modules installed, you can proceed to load and run them.

    modules load MODULE loads an installed module
    CTRL + C unloads the module.
    info to review the loaded module’s info.
    options list lists available options for the chosen module.
    options set NAME VALUE
    run to execute the loaded module.
  
***DEMO***  
To wrap up, use the following demonstration provided below containing all the steps we have explained earlier.  
Use recon-ng to repeat the steps we carried out against thmredteam.com, then answer the following questions.  
  
*How do you start "recon-ng" with the workspace "clinicredteam"?* **recon-ng -w clinicredteam**  
*How many modules with the name "virustotal" exist?* **2**  
*There is a single module under "hosts-domains". What is its name?* **migrate_hosts**  
*"censys_email_address" is a module that "retrieves email addresses from the TLS certificates for a company." Who is the author?* **Censys Team**  
  
  
## Maltego  
[Maltego](https://www.maltego.com/) is an application that blends mind-mapping with OSINT. In general, you would start with a domain name, company name, person’s name, email address, etc.  
Then you can let this piece of information go through various transforms.
  
The information collected in Maltego can be used for later stages. For instance, company information, contact names, and email addresses collected can be used to create very legitimate-looking phishing emails.  
  
Think of each block on a Maltego graph as an entity. An entity can have values to describe it. In Maltego’s terminology, a transform is a piece of code that would query an API to  
retrieve information related to a specific entity. The logic is shown in the figure below. Information related to an entity goes via a transform to return zero or more entities.  
  
![Transform](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/2e8ac9b5c947e1af26b7b9e24c5f8361.png)  
  
It is crucial to mention that some of the transforms available in Maltego might actively connect to the target system. Therefore, it is better to know how the transform works  
before using it if you want to limit yourself to passive reconnaissance.  

Every transform might lead to several new values. For instance, if we start from the “DNS Name” cafe.thmredteam.com, we expect to get new kinds of entities based on the  
transform we use. For instance, “To IP Address” is expected to return IP addresses as shown next.  
![cafe.thmredteam.com](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/948575bd71d1305e8505c854ac9b81e0.png)  
  
One way to achieve this on Maltego is to right-click on the “DNS Name” cafe.thmredteam.com and choose:  
* Standard Transforms  
* Resolve to IP  
* To IP Address (DNS)  
  
After executing this transform, we would get one or more IP addresses, as shown below.  
![Resolve IP Address](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/956d258d9b72d88bb6e088c2f2b4690f.png)  
  
Then we can choose to apply another transform for one of the IP addresses. Consider the following transform:  
* DNS from IP  
* To DNS Name from passive DNS (Robtex)  
  
This transform will populate our graph with new DNS names. With a couple more clicks, you can get the location of the IP address, and so on. The result might be similar to the image below.  
![DNS from IP](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/3d05e45bd6021ca651892542b06f4230.png)  
  
The above two examples should give you an idea of the workflow using Maltego. You can observe that all the work is based on transforms, and Maltego  
will help you keep your graph organized. You would get the same results by querying the different online websites and databases; however, Maltego helps you get all the information you need with a few clicks.

We experimented with whois and nslookup in a previous task. You get plenty of information, from names and email addresses to IP addresses.  
The results of whois and nslookup are shown visually in the following Maltego graph. Interestingly, Maltego transforms were able to extract  
and arrange the information returned from the WHOIS database. Although the returned email addresses are not helpful due to privacy protection,  
it is worth seeing how Maltego can extract such information and how it's presented.  
  
![NSLookup Map](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/c54a869cffca4d657f46dac618cc9135.png)  
  
Now that we have learned how Maltego’s power stems from its transforms, the only logical thing is to make Maltego more powerful by adding new Transforms. Transforms are usually grouped into different categories based on data type, pricing, and target audience. Although many transforms can be used using Maltego Community Edition and free transforms, other transforms require a paid subscription. A screenshot is shown below to give a clearer idea.  
![Maltego Transform Hub](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/adc8ab512edcf6fef5414d434d7577a1.png)  
  
Using Maltego requires activation, even if you opt for Maltego CE (Community Edition). Therefore, the following questions can be answered by visiting Maltego Transform Hub or by installing and activating Maltego CE on your own system (not on the AttackBox).  
  
*What is the name of the transform that queries NIST's National Vulnerability Database?* **NIST NVD**  
*What is the name of the project that offers a transform based on ATT&CK?* **MISP Project**  
  
  
## Summary  


Sun Tzu once said, “If you know the enemy and know yourself, you need not fear the result of a hundred battles. If you know yourself but not the enemy, for every victory gained you will also suffer a defeat. If you know neither the enemy nor yourself, you will succumb in every battle.” Fast forward to the cyber warfare era; in addition to knowing our red team skillset and capabilities, we need to gain as much information about the target as possible. The terrain is constantly evolving, and new ways to collect data are becoming possible.

We have reviewed essential built-in tools such as whois, dig, and tracert. Moreover, we explored the power of search engines to aid in our passive reconnaissance activities. Finally, we demonstrated two tools, Recon-ng and Maltego, that allow us to collect information from various sources and present them in one place.

The purpose is to expand our knowledge about the target and collect various information that can be leveraged in the subsequent attack phases. For instance, hosts that are discovered can be scanned and probed for vulnerabilities, while contact information and email addresses can be used to launch phishing campaigns efficiently. In brief, the more information we gather about the target, the more we can refine our attacks and increase our chances of success.
  
