# Projects

There are a couple of personal projects I have produced.

## Fellsman Entrant Management

I have been involved with the [Fellsman](https://fellsman.org.uk) since 1984 first as a volunteer, then for a number of years on the organising committee. Recently I went back to being an volunteer acting as Safety Officer and then running a checkpoint.

Since 1995 I have produced an entrant management system. Used to keep track of finisher and drop out statuses of the entrants, as well as producing the results lists, certificates and address labels for the mailshot. It has taken many forms over the years, from a VB6 application with a MS Access database to its current incarnation which is a Django web based application. From a maintenance and management perspective this latest version may be almost perfect!

I am not ready to open the repository to public access, but the documentation will soon be online. Keep checking back for updates.

## Mantis Email Service

At work we user the Mantis Bug Tracking system, self hosted on a windows PC. One of the main issues we encountered was mail delivery, which we initially solved with this windows service. When Microsoft removed the ability to easily configure an `SMTP` server this service was adjusted to use the Graph interface to send the emails.
