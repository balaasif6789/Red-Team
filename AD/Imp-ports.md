Below Ports which needs to be opened for Active directory to function properly
UDP Port 88 for Kerberos authentication
UDP and TCP Port 135 for domain controllers-to-domain controller and client to domain controller operations.
TCP Port 139 and UDP 138 for File Replication Service between domain controllers.
UDP Port 389 for LDAP to handle normal queries from client computers to the domain controllers.
TCP and UDP Port 445 for File Replication Service
TCP and UDP Port 464 for Kerberos Password Change
TCP Port 3268 and 3269 for Global Catalog from client to domain controller.
TCP and UDP Port 53 for DNS from client to domain controller and domain controller to domain controller.
Also refer below link which will explains this in more detail.

Port used in Active directory Replication.

http://blogs.technet.com/b/janelewis/archive/2006/11/13/ports-used-in-active-directory-replication.aspx

Active directroy and Firewall ports,

http://geekswithblogs.net/TSCustomiser/archive/2007/05/09/112357.aspx

Active Directory Replication over Firewall

http://social.technet.microsoft.com/wiki/contents/articles/584.active-directory-replication-over-firewalls.aspx
