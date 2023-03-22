# HAW-Design
Created: 2023-03-21 18:06
Tags: 
____

facts
1. 80% of notifications would send by email
2. ~120 email templates
3. 700,000 email notification per month
4. will grow 40% in 2023 ( ~1M email per month)
5. shared service or interface that product engineering teams can use to send notifications
6. 90 days saving messages


1. functional requirements
	1. notification system
		2. `Email`
		3. `SMS`
		4. `Whatsapp` 
		5. `Slack`
	2. Shared interface / API for product engineering 
	3. __Unsubscribe__ or opt out in case of `email`, `SMS` and `Whatsapp`
	4. in case of emails __Bulk__ notifications in specefic scenarios



5. start making proposal
	1. understand problem and establish design scope
	2. propose high-level design and get buy-in
	4. design deep dive
	5. wrap up

Constraints
1. we define our services using `Domain Driven Design`.
2. We greatly emphasize __continuous integration, continuous delivery __ and automation
3. __Security__ is of utmost importance here
4. design solutions that scale with the business

Discuss the following
	1. your proposed design for this shared service
	2. your plan to develop and roll out this new service


Requirements
	1. Product engineering team should be able to audit the notification sent in the past 90 days for troubleshooting purposes
		1. undeliverable notifications
		2. bounced `emails`
	2. in the case of `emails`, `SMS`, and `Whatsapp`, end-users should be able to unsubscribe or opt out of receiving a particular notification.
	3. in the case of emails, we would like to use them to send bulk notification



Databse
APIS

SendNofitication

```json
{
	"user_id": "314342132",
	"priority": "high", // ("high", "low") for transactional and promotional
	"type": ["SMS", "EMAIL"], // array of enum (SMS, EMAIL, WHATSAPP, SLACK),
	"content" :{
		"template":"12",
		"values": {
			"text":"s",
			"time":"daas"
		}
	}
}
```

SendBulkNotfication 
```json

{
	"user_id": "314342132",
	"priority": "high", // ("high", "low") for transactional and promotional
	"type": ["SMS", "EMAIL"], // array of enum (SMS, EMAIL, WHATSAPP, SLACK),
	"content" :{
		"template":"12",
		"values": {
			"text":"s",
			"time":"daas"
		}
	}
}
```



_____
##### References
1.

