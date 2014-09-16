[![Build Status](http://img.shields.io/travis/ArthurGuy/BBMembershipSystem.svg?style=flat-square)](https://travis-ci.org/ArthurGuy/BBMembershipSystem)
[![Code Climate](http://img.shields.io/codeclimate/github/ArthurGuy/BBMembershipSystem.svg?style=flat-square)](https://codeclimate.com/github/ArthurGuy/BBMembershipSystem)

BB Membership System
====================

The Build Brighton membership management system

New members can join and create accounts, payments are tracked and managed through the system and GoCardless


###Features
* Member signup form which collects full name and address, emergency contact and profile photo.
* Direct Debit setup and payment collection through GoCardless
* Notifications of monthly Direct Debit payments are received and used to extend the users subscription
* PayPal IPN notifications are also received and used to extend member subscriptions.
* The ability for the user to edit all their details allowing for self management
* Various user statuses to cater for active members, members who have left or been banned as well as tracking founders and honorary members
* Handling of the induction/equipment training procedures and collection of payments.
* Tracking of who trains who
* Member grid to see who belongs to Build Brighton
* The ability for members to cancel their subscription and leave Build Brighton (not that they would want to!)
* Collect deposit payments for door keys and member storage boxes
* Door entry control and tracking

###Upcomming Features
* Handle member storage box assignments
* Equipment control and usage logging



###Member Statuses
There are a variety of member statuses which are used for various scenarios.
* Setting Up - just signed up, no subscription setup, no access to space
* Active
* Payment Warning - subscription payment had been cancelled - will move to leaving the next day
* Leaving - The user has said they are leaving or they were in a payment warning state
* Left - Leaving users move here once their last payment expires.


###Other Makerspaces
This system can be used with only minor modifications by other spaces.<br />
The Build Brighton naming is hardcoded into the pages and pieces of text will need to be altered.<br />
It has been design to work primarily with GoCardless but the PayPal integration is OK and would be good enough on its own.<br />
The system also has limited support for scanning and processing payments from bank statements


###Seting It Up
The system is build on the Laravel framework so familiarity with that would help.

The config options below need to be setup in an .env config file or passed in to php can access them through $_SERVER

* AWS_KEY
* AWS_SECRET
* DB_HOST
* DB_NAME
* DB_USER
* DB_PASS
* GOCARDLESS_APP_ID
* GOCARDLESS_APP_SECRET
* GOCARDLESS_MERCHANT_ID
* GOCARDLESS_ACCESS_TOKEN
* MAILGUN_USERNAME
* MAILGUN_PASSWORD
* ENCRYPTION_KEY

Some of these wont be needed.<br />
AWS is used for avatar storage.<br />
The system is built for a MySQL DB but a similar system will work<br />
GoCardless as above<br />
MailGun is completely optional<br />
The encryption key is essential<br />