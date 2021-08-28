# Red Team: Summary of Operations
## Table of Contents
## **Target 1**
  - Planning and Reconnaissance and Vulnerability Identification Phase
  - The exploitation phase
  - Privilege Escalation

## Planning and Reconnaissance and Vulnerability Identification Phase

After running `nmap -sS -sC -sV -Pn 192.168.1.0/24` we can identify all the IP's, and what they're doing on the network.

[pictures/nmapscan1.png]
[pictures/nmapscan2.png]
[pictures/nmapscan3.png]
[pictures/nmapscan4.png]
[pictures/nmapscan5.png]

| HOSTNAME       |     IP        | OPEN PORTS |
|----------------|---------------|------------|
| HOST           | 192.168.1.1   |            |
| KALI (root)    | 192.168.1.90  |            |
| ELK            | 1291.68.1.100 |   9200     |
| TARGET1        | 192.168.1.110 |  22, 80    |
| CAPSTONE       | 192.168.1.105 |  22, 80    |

Now that we've identified `192.168.1.110` as the target WordPress server, and we can enumerate with a common wordlist provided by the WPSCAN command, using `wpscan -e --url http://192.168.1.110/wordpress`,

[pictures/wpscan.PNG]

we found the users `michael` and `steven`, and some associated directories

Port 22 is open, lets see if we can `ssh michael@192.168.1.110` and guess the password.

We were able to ssh into the target machine using username: michael and password: michael

[pictures/ssh.png]

## The exploitation phase
Know that we can traverse the directories, we can go to common default directory names to look for *the sensitive information* that we are looking for.

*flag1*
found in `/var/www/html/service.html`

[pictures/flag1.png]

*flag2*
found in `/var/www/`

[pictures/flag2.png]

Someone left the credentials for the SQL database lying around at `/var/www/html/wordpress/wp-config.php`

[pictures/sqldb.png]

Both of these flags just take a little bit of time to find and go through, they're located in similar directories that would normally be of interest, While looking also looking for the SQL database

Going into the SQL database we can see that the credentials are `root:895819057t190-` taking a look with `sql root@192.168.1.110` we can see the following databases, the one of interest seems to be `wordpress`

[pictures/sql_screenshot1.png]

[pictures/sql_screenshot2.png]

*flag3* found found `wordpress/wp_posts` in the SQL databasae

[pictures/flag3.png]

### Privileged Escalation

So, there is an intended way to do a real exploit left for this lab to do Privileged escalation, and it will be demonstrated, but it is important to note that the root password was any first root password guess you can do, `toor`, which saved any real attempt for actual thinking.

*flag4* 
found in `/root`

[pictures/flag4.png]

And that's all for flags
