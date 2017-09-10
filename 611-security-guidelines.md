# 6.6.1 Security Guidelines

Anyone using MySQL on a computer connected to the Internet should read this section to avoid the most common security mistakes.

In discussing security, it is necessary to consider full protecting the entire server host\(not just the MySQL server\)against all types of applicable attacks: eavesdropping, altering, and denial of service. We do not cover all aspects of availability and tolerance here.

MySQL uses security based on Access Control Lists\(ACLs\) for all connections, queries, and other operations that users can attempt to perform. There is also support for SSL-encrypted connections between MySQL clients and servers. Many of the concepts discussed here are not specific to MySQL at all; the sam general ideas apply to almost all application.

When running MySQL, follow these guidelines:

* Do not ever give anyone\(except MySQL **root** accounts\) access to the **user** table in the **mysql** database! This is critical.

* Learn how the MySQL access privilege system works \(see [Section 6.2, "The MySQL Access Privilege System"](/62-the-mysql-access-privilege-system.md)\). Use the **GRANT** and **REVOKE** STATEMENTS to control access to MySQL.Do not grant more privileges than necessary. Never grant privileges to all hosts.

  Checklist:

  * Try **mysql -u root**. If you are able to connect successfully to the server without being asked for a password, anyone can connect to your MySQL server as the MySQL **root** user with full privileges! Review the MySQL installation instructions, paying particular attention to the information about setting a **root** password. See Section 2.10.4, "Securing the Initial MySQL Accounts".

  * Use the **SHOW GRANTS** statement to check which accounts have access to what. Then use the **REVOKE** statement to remove those privileges that are not necessary.

* Do not store cleartext passwords in you database. If you computer compromised, the intruder can take the full list of passwords and use them. Instead, use **SHA2\(\)** or some other one-way hashing function and store the hash value.

To prevent password recovery using rainbow tables, do not use these functions on a plain password; instead, choose some string to be used as a salt, and use hash\(hash\(password\) + salt\) values.

* Do not choose passwords from dictionaries. Special programs exist to break passwords. Even passwords like "xfish98" are very bad. Much better is "duag98" which contains the same word "finsh" but typed one key to the left on a standard QWERTY keyboard. Another method is to use a password that is taken from the first characters of each word in a sentence\(for example, "Four score and seven years ago" results in a password of "Fsasya"\). The password is easy to remember and type, but diffcult to guess from someone who does not know the sentence. In this case substitute digits for the number words to obtain the phrase "4 score and 7 years ago", yielding the password "4sa7ya" which is even more difficult to guess.

* Inverst in a firewall. This protects you from at least 50% of all types of exploits in any software. Put MySQL behind the firewall or in a demilitarized zone\(DMZ\).

  Checklist:

  * Try to scan your ports from the Internet using a tool such as **nmap**. MySQL users port **3306** by default. This port should not be accessible from untrusted hosts.As a simple way to check whether your MySQL port is open, try the following command from some remote machine, where **server\_host** is the host name or IP address of the host on which your MySQL server runs:
  
   ```
    shell> telnet server\_host 3306
    ```
 If **telnet** hangs or the connection is refused, the port is blocked, which is how you want it to be. If you get a connection and some garbage characters, the port is open, and should be closed on your firewall or router, unless you really have a good reason to keep it open.

* Applications that access MySQL should not trust any data entered by users, and should be written using proper defensive programming techniques. See Section 6.1.7, "Client Programming Security Guidelines".

* Do not transmit plain(unencrypted) data over the Internet. This information is accessible to everyone who has the time and ability to intercept it and use it for their own purposes. Instead, use an encrypted protocol such as SSL or SSH. MySQL supports internal SSL connections. Another technique is to use SSH port-forwarding to create an encrypted(and compressed) tunnel for the communication.

* Learn to use the **tcpdump** and **strings** utilities. In most cases, you can check whether MySQL data streams are unencrypted by issuing a command like the following:


```
shell> tcpdump -l -i eth0 -w -src or dst prot 3306 | strings
```
This works under Linux and should work with small modifications under other systems.

> Warning
>
> If you do not see cleartext data, this does not always mean that the information actually is encrypted. If you need high security, consult with a security expert.




