# jira-agile-holiday

Holiday removes some of the tedium of updating the non-working days on your JIRA Agile boards.

##	Requirements

Holiday Bot is a Perl script.
The following Perl modules are required:

	Getopt::Long
	REST::Client
	JSON

## Command Line Usage

Usage: holidaybot [options]

Options:

	--config=file    Specify a rules file (default=./config.json).
	--cred=file      Specify a credentials file (default=./holiday.cred).
	--mkcred=creds   Converts credentials to base64 format for credentials file.
	                 Example: --mkcred=username:password
	                 (Note: This is NOT encrypted. Protect your cred file.
	--help           Output this message.


## Setup Instructions

### 1. Familiarize yourself with the Command Line Usage (above)


### 2. Create a credentials file

The Jira API requires the username and password of a Jira user
be formatted in a certain way. This is done by taking the
username and password, sparated by a colon, and converted
into base64.

Holiday Bot will perform this formatting for you with the 
--mkcred option. Run:

```
./holidaybot --mkcred=username:password > holidaybot.cred
```

By default, holidaybot looks for these credentials in the
./holidaybot.cred file, but can be pointed elsewhere with the
--cred option.

Note: The contents of this file are saved in base64, but this
is *NOT* encrypted. The contents can easily be converted back
to obtain the cleartext username and password of your Jira
user. Set appropriate permissions on your credentials file.

### 3. Create your config file

A sample config file has been included, called 
./config.json.example. The config file is in JSON (JavaScript
Object Notation) format, and can easily be edited by hand.
You should copy the sample file to ./config.json to get started.

Example:
```
	{
		"jiraurl": "https://jira.yourcompany.com",
		"board_ids": ["1", "2", "3"],
		"holidays": ["2015-12-24", "2015-12-25"]
	}
```

The config file has several properties:

* jiraurl
* board_ids
* holidays

The jiraurl property is the full base URL to your Jira
instance.

Example:
```
  "jiraurl": "https://jira.yourcompany.com",
```

The board_ids property is an array of the numerical IDs assigned to the boards 
you want to update. This can be determined by navigating to the board 
in JIRA and looking at the "rapidView" property in the URL.

https://jira.yourcompany.com/secure/RapidBoard.jspa?rapidView=<b>6</b>&quickFilter=108

Example:
```
	"board_ids": ["6", "12", "35"],
```

The holidays property is an array of the dates you want to flag as non-working days 
on the configured JIRA boards. These should be in the ISO 8601 date format.

Example:
```
	"holidays": ["2015-12-24", "2015-12-25"]
```



### 4. Running Holiday Bot

Once you have holiday.cred and config.json in place, you're ready
to run Holiday Bot. Holiday Bot will output some details about what it's
doing with each date. You'll likely want to redirect this output
to a log file. Example:

```
./holidaybot > logs/holidaybot.log
```

### 5. Caveats

Holiday Bot is intented to be a one and done solution that you run whenever 
you need to add new dates to your boards, for example annually. If you wanted to 
automate this in a cron job, you could do so easily, however the dates would 
still need to be added to the config file.

In its current form, Holiday Bot only adds new dates. It does not "sync" dates 
with the config file. Meaning, if you add a date by mistake and run Holiday Bot,
you will need to manually remove it from your boards. If there is enough demand
I will see if I can support a "sync" like feature through the API. If anyone knows
of an API call to clear all non-working days, that would make it much simpler.

### Credit

Holiday Bot was inspired by and based on Label Bot https://github.com/burnettk/jira-labelbot

## Maintainers

* Mike Enright (original author)

## Contributing

Send a pull request.
