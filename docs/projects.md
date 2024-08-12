# Projects

There are a couple of personal projects I have produced.

## Fellsman Entrant Management

[![PyPI - Implementation](https://img.shields.io/pypi/implementation/Django?logo=Django&label=Django)
](https://www.djangoproject.com/)

I have been involved with the [Fellsman](https://fellsman.org.uk) since 1984, first as a volunteer, then for a number of years on the organising committee. Recently I went back to being an volunteer acting as Safety Officer and then running a checkpoint.

Since 1995 I have produced an entrant management system. Used to keep track of finisher and drop out statuses of the entrants, as well as producing the results lists, certificates and address labels for the mailshot. It has taken many forms over the years, from a VB6 application with a MS Access database to its current incarnation which is a Django web based application. From a maintenance and management perspective this latest version may be almost perfect!

I am not ready to open the repository to public access, but the documentation will soon be online. Keep checking back for updates.

## Mantis Email Service

[![github repo](https://img.shields.io/badge/github-repository-blue?style=plastic&style=for-the-badge&logo=Github&logoColor=white)](https://github.com/katterfelto/MantisBTMailTask) [![github repo](https://img.shields.io/badge/.NET_core-v3.1-green?style=plastic)](https://dotnet.microsoft.com/)

At work we use the [Mantis Bug Tracking](https://mantisbt.org/) system, self hosted on a windows PC. One of the main issues we encountered when initially setting it up was mail delivery. This was due to our lack of experience setting up php io work with IIS, we initially solved the problem with this windows service. When Microsoft removed the ability to easily configure an `SMTP` server in Exchange 365, this service was adjusted to use the GraphQL interface to send the emails.

Mantis has a database table which buffers the outgoing emails until they are sent. If no email delivery client is configured the contents of this table grows unless the email subsystem is disabled. If not processed this table also swallows important emails such as password reset requests.

## Timesheet Updater Application

[![github repo](https://img.shields.io/badge/github-repository-blue?style=plastic&style=for-the-badge&logo=Github&logoColor=white)](https://github.com/katterfelto/TimesheetUpdate) [![github repo](https://img.shields.io/badge/.NET-v6.0-green?style=plastic)](https://dotnet.microsoft.com/) [![github repo](https://img.shields.io/badge/Avalonia-v11.0.6-green?style=plastic)](https://avaloniaui.net/)

This is rather specific, my company uses Excel spreadsheets for our time recording. Excel is very flexible but sometimes it ends up being a square peg forced into a round hole and for timesheets can be very inflexible and annoying. I started to use an online service called [Clockify](https://clockify.me/) which better suits my needs and work flow.

I just needed a way to synchronise the data in my Clockify account with my spreadsheet. Luckily Clockify provides a web API which helped solve the problem. I ended up writing this [Avalonia UI](https://avaloniaui.net/) base app to download a specific weeks time entries, then transform them into a format I could use to record the time on my spreadsheet.

This is my first foray into WPF project development.