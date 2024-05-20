
# auto attendants
* allow you to set up menu options to route calls based on caller input. Menu options for an Auto attendant--such as "For Sales, press 1--For Services press 2"

* direct a caller to:
	* specific people within your organization
	* Call queues
	* external numbers
	* other Auto attendants
	* voicemail

* call routing options can be specified for:
	* business hours
	* off hours
	* holidays

* each Auto attendant has a specific language and time zone.
* you can create as many different Auto attendants as you need to accommodate your callers with different languages

**Operator**
* can be configured for each AA
* designed to allow callers to talk to a specific person in your organization who can help them.

**Dial scope**
* specify who is available for the directory search by choosing groups of users to include or exclude
# call queue
* waiting areas for callers; analogous to a waiting room in a physical building

* calls can be routed to:
	* specific people
	* voicemail
	* other Call queues
	* Auto attendants

* also have language settings and just like AA, can create multiple CQs

* *Internal callers*
	- can reach CQ by calling the RA assigned to the CQ

- *External callers*
	- can reach CQ by dialing the phone number assigned to the RA
	- via **web** if click-to-call is configured

* When agents receive a call, they will see the RA name. 



# [prerequisites]([Plan for Teams Auto attendants and Call queues - Microsoft Teams | Microsoft Learn](https://learn.microsoft.com/en-us/microsoftteams/plan-auto-attendant-call-queue#prerequisites)

1. RA for each AA and CQ
	- several RA can be assigned to AA & CQ
2. a free MS Teams phone resource account license for each RA
3. external phone calls:
	- phone number (toll or toll-free)
4. agents receiving calls from CQ must be Enterprise Voice enabled
5. if agents are using MS Teams app for CQ calls, they need to be in TeamsOnly mode
6. if AA or CQ is transferring calls to an external number:
	- one of the following:
		- a **calling plan license** and a **phone number** assigned
		- an online voice routing policy (for Direct routing)
	- license the RA on the first AA receiving the call when that AA transfer to other AA or CQ that transfer calls externally
	- license the RA of the AA or CQ performing the external transfer

7. if RA is used for calling line ID, one of the following must be assigned:
	- a **calling plan** and a **phone number** assigned
	- an online voice routing policy (for Direct routing)

### How agents are added to CQs?

- Individual users
- Distribution lists
- Security groups, including mail-enabled security groups
- Microsoft 365 Groups or Teams

>Groups that have an email address can be used for voicemail.

