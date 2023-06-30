- #### Three Tier Architecture
	- ![Three-Tier Architecture Approach for Custom Applications - Zirous](https://www.zirous.com/wp-content/uploads/2022/10/Untitled-5-01.png)
	- Oraganizes applications into three logical and physical computing tiers:
		- **Presentation tier** - *user interface*
		  logseq.order-list-type:: number
		- **Application tier** - *business logic*
		  logseq.order-list-type:: number
		- **Data tier** - *persisting and managing application data*
		  logseq.order-list-type:: number
	- Each tier runs on its own infrastructure
	- ##### Benefits
		- Each tier can be developed simultaneously by a separate development team
		  logseq.order-list-type:: number
		- Outage in one tier is less likely to to impact the availability or performance of the other tiers
		  logseq.order-list-type:: number
- #### DMZ (demilitarised zone)
	- Protects and adds layer of security to an organisation's internal local-area network from **untrusted traffic**
	- DMZ allows an orgranization to access untrusted network,  such as the internet, *while* ensuring its private **network or LAN** remain secure
	- DMZ store *external-facing services*:
		- DNS
		  logseq.order-list-type:: number
		- FTP
		  logseq.order-list-type:: number
		- mail
		  logseq.order-list-type:: number
		- proxy
		  logseq.order-list-type:: number
		- VoIP
		  logseq.order-list-type:: number
		- Web server
		  logseq.order-list-type:: number
	- These servers and resources are isolated and given limited access to the *LAN*
	- ![What is Demiltarized Zone? - GeeksforGeeks](https://media.geeksforgeeks.org/wp-content/uploads/20220808192523/DemiltarizedZoneDMZ.png)
		-
	- Secured between two firewalls, *filter* exchanged data, between internal network and DMZ, traffic flowing from external network to DMZ
		- screen data packets before they get through the servers in the DMZ
		  logseq.order-list-type:: number
	- ==Variant==
		- Single Firewall
		- ![No alt text provided for this image](https://media.licdn.com/dms/image/D4E12AQEkE2nZf5Bweg/article-inline_image-shrink_1500_2232/0/1666183153904?e=1693440000&v=beta&t=FI3oVnQT6RgwnQswO6MR_8OvpZeksQDghoGYxjapLF4)
		- Dual Firewall
		- ![No alt text provided for this image](https://media.licdn.com/dms/image/D4E12AQFreO-gnRk5Sw/article-inline_image-shrink_1500_2232/0/1666183170168?e=1693440000&v=beta&t=hksIB0LdcLL7ZEpnwST6V-Fn4Fuw0C475bX4B-CcHqI)
	- ![Do we know the names of the other 3 districts from Wall Maria? I read  something about the western district being called Quinta, but I don't know  if it's true. I'm an](https://i.redd.it/0523qdwxxol61.jpg)