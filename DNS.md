# DNS ( Domain Name System ) 

   Basically, If we have used the internet, it means we have used the DNS. DNS is used to convert human readable names(i.e https://google.com) to Internet Protocol(IP) (such as http://172.217.166.110).
   
   Ip Addresses are used by computers to identify each other on the network. And there are two types of IP addressess
   
   * [IPv4](https://en.wikipedia.org/wiki/IPv4) (mostly Used)   
   * [IPv6](https://en.wikipedia.org/wiki/IPv6)
   
  
   A Domain Name System is also a Distributed Database, Partitioned into zones which are managed in a distributed, decenteralized way which are available globally, accross the internet.
   
   Database typically have indexes,DNS indexes are those funny dotted names, which are properly called domain names. Domain Names are actually paths in a inverted tree.
   
   ![DNS Tree](https://github.com/DevOpsStuff/Networking/blob/master/dns-hierarchy.gif)
   
   (Source: Google images)
   
 # NameServers,Zones and Authority
 
   DNS is a client-Server System.
   * The Servers are called name servers or DNS servers.
   * The clients are called resolvers, and they query name servers.
   
   ![DNS Resolver to NS](https://github.com/DevOpsStuff/Networking/blob/master/ns.jpg)
   
   (Source: Google images)
   
   Name Servers Load data in zones, the administrative units in DNS.
   * Name servers may load thousands of zones, but no name server loads the entire namespace.
   * A name server that loads an entire zone is `authoritative` for that zone.
   
   Name Servers are reponsible for answering the queries sent by the resolvers. But the Name servers also query other Name Servers.
   
# Resolvers
  
  All the OS have the DNS resolvers,Such as nslookup,dig. It is a software application. Resolvers takes applications requests for data in  DNS and translates them into DNS queries.
  
  Resolvers then awaits a response,
    * It may retransmit the query after a timeout.
    * It may fall back to querying a different name servers, or a differnt naming server entirely.
    
 when it receives a response, the resolver unmarshals it into data structure that passes it back to application.
 
## DNS Resolutions

   The below image shows whenever a client hits the request, How DNS resolves the query.
   
   ![DNSResolution](https://github.com/DevOpsStuff/Networking/blob/master/dnsresolution.png)
   
   (Source: google images)
   
## DNS Recursion

   Queries are either recursive or non-recursive, also called as iterative.
   
   * Accepting a recursive query obliges a name server to do all the work necessary to find an answer
   
      * which may inculde following serveral referrals
      * the name servers may not send a referral in response
      
   * A nameserver reponds to a non-recursive query with the best answer it already knows, often a refferal.
   
## DNS Caching

   All the DNS enteries are cached. Meaning if we hit the google.com
   
     * Client -> NS -> (Root Domain) (1 Referel)
     * (Root domain) -> NS -> (Top level domain) ( 2 Referal)
     * (Top level domain) -> NS -> google.com
     
  When we first hit the google.com it takes two referals to finally reach the required results. Next time when we hit the same google.com. It will take zero referrals. Now when we hit he "facebook.com 
  
    * Client -> NS -> (top level domain) ( 1 referal).
    * (TLD) -> NS -> facebook.com
    
  **Time To Live**
  
  Cached data must eventually time out in order to force name servers to re-query autoritative name servers and see changes. Each resource record has a value associated with it called time to live, or TTL, which governs how long another name server may cache that record
     
  * TTL range from seconds to hours to days
  * The administrator of the zone that contains the record decides on the TTL.
  * A name server that has a record cached decrements its TTL over time. when the TTL reaches zero, the name server deletes the record.
  * TTL has no effect on a name server authoritative for the zone that contains the resource record.

## Server selection and Retransmissions

* Most zones have more than one name server
    * Recursive name servers need to choose which one(s) to query.
  
* Queries and responses are generally sent over UDP 
    * So they can get lost
    * Both resolvers and name server must handle retansmissions.
    
* Retransmission algorithms also depends on implementation

* However, all name servers send queries and await responses
   * if they don't receive a response within a timeout, they retransmit the query - to another name server, if possible.
 
    
     
