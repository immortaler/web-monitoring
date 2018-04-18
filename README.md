# Web Monitoring


This repository is for EDGI [Web Monitoring Project](https://github.com/edgi-govdata-archiving/web-monitoring) documentation and project-wide issue management.

EDGI is already monitoring tens of thousands of pages and will eventually be monitoring tens of millions (or even as many as ~1 billion). Currently, there is a lot of manual labor that goes into reviewing all changes, regardless of whether they are meaningful or not. Any system will need to emphasize usability of the UI and efficiency of computational resources.

- [Project Goals](#project-goals)
- [How to Help](#how-to-help)
- [Project Overview](#project-overview)
- [License & Copyright](#license--copyright)

You can **track upcoming releases** by exploring our [milestones](https://github.com/edgi-govdata-archiving/web-monitoring/milestones) within the GitHub issue queue.

## :hammer: Technologies Used

* [**Heroku.**][heroku] A platform for easily deploying applications.
* [**Review Apps.**][review-apps] A Heroku feature that deploys code
  from GitHub pull requests as a disposable app on the web.
* **Javascript.** Notable packages include:
  * [**Webpack.**][webpack] A _static module bundler_ for modern JavaScript
    applications.
  * [**ReactJS.**][reactjs] A JavaScript library for building user
    interfaces.
* [**Redis.**][redis] An open source, in-memory data structure store,
  used as a database, cache and message broker.
* **HTML & CSS.**
* **SASS.**
* **Python.** Notable packages include:
  * [**Beautiful Soup.**][beautifulsoup] A library for pulling data out
    of HTML and XML files.
  * [**Tornado.**][tornado] A web server and application framework.
* [**Sentry.**][sentry] A hosted error-tracking service, that happens to
  be open source.
* [**Ruby on Rails.**][rails] A web application development framework used by our
  backend API server.
* [**Swagger.**][swagger] A spec and framework for API developer tools.
* [**Docker.**][docker] Runs "containers images" to help make running
  software simpler for developers.
* [**Postgresql.**][postgres] A powerful, open source
  object-relational database system.
* [**Amazon Web Services. (AWS)**][aws] A hosted cloud services platform
  for servers, databases, file storage, etc.
* [**Ansible.**][ansible] An open source automation platform for
  software configuration.
* [**Versionista.**][versionista] Enterprise tool for webpage change
  detection and alerts.
* [**Internet Archive.**][ia] A nonprofit-led digital library of
  Internet website history going back 20+ years.

   [heroku]: https://www.heroku.com/what
   [review-apps]: https://devcenter.heroku.com/articles/github-integration-review-apps
   [reactjs]: https://thinkster.io/tutorials/what-exactly-is-react
   [sentry]: https://sentry.io/about/
   [redis]: https://redis.io/topics/introduction
   [rails]: http://guides.rubyonrails.org/getting_started.html
   [swagger]: https://swagger.io/
   [docker]: https://www.docker.com/what-container
   [postgres]: https://www.postgresql.org/about/
   [aws]: https://aws.amazon.com/what-is-aws/
   [ansible]: https://networklore.com/ansible/
   [versionista]: https://versionista.com/about
   [ia]: https://archive.org/about/
   [webpack]: https://webpack.js.org/concepts/
   [tornado]: http://www.tornadoweb.org/en/stable/
   [beautifulsoup]: https://medium.freecodecamp.org/how-to-scrape-websites-with-python-and-beautifulsoup-5946935d93fe

## Project Goals

The purpose of the system is to enable analysts to quickly review monitored government websites in order to report on [__meaningful changes__](#identifying-meaningful-changes). The Website Monitoring automated system aims to make these changes easy to track, review, and report on.

For more, see [the Version Tracking page on the EDGI website](https://envirodatagov.org/version-tracking/) and watch this
[50-minute Analyst training video](https://www.youtube.com/watch?v=iFz9xhD41G8).

## How to Help

The best way to get involved is to take a run through [our onboarding
process][onboarding], for which we rely on Trello. It's designed to be self-directed,
so you can run through it at your own pace. But don't worry -- along the
way, it will introduce you to the humans of EDGI's Web Monitoring
project! Yay humans!

[![Onboarding screenshot](http://i.imgur.com/JmFiMue.png)][onboarding]

### Where we work

* Say hi on our chat!
   * [Create](https://archivers-slack.herokuapp.com/) an account on Slack team.
   * Join us in the `#webmonitoring` channel.
* Attend a software development call.
   * [Join](https://edgi-video-call-landing-page.herokuapp.com/https://zoom.us/j/187414228) our call, every Wednesday at 2pm ET.
   * Zoom Meeting account is optional. See above link for details.

## Project Overview

### Use Case

1. Access captured data (starting with HTML, later encompassing more types) from
   multiple archival sources including Versionista, PageFreezer, and the
   Internet Archive.
2. Compare version of the same page over time --- potentially using multiple
   different strategies.
3. Automatically filter out "nonmeaningful" or repetitive changes: for example,
   the "Page Last Viewed" timestamp updated or the same news article was added
   to 100 pages from the same website.
4. Prioritize the changes most likely to be "meaningful," meaning that some
   item of importance to fact-based governance was deleted or changed in a
   harmful way.
5. Present changes to human analysts with useful visualizations and statistics
   to help them differentiate meaningful changes. Each user will have been
   assigned a "subdomain", a full or partial government domain that has been
   identified as relevant to fact-based governance.
6. Collect annotations from the analysts. Use this to flag changes for special
   attention from EDGI administrators. Also, use it to feed back into the
   filtering and prioritization process. (That is, use it to train models.)

### Definition of Terms

* Page: a web page crawled over time by one or more services like Internet
  Archive, Versionista, or PageFreezer.
* Version: a snapshot of a Page at a specific time (saved as HTML, for now).
* Change: two different Versions of the same Page.
* Diff: a representation of a Change: this could be a plain text `diff` (as in
  the UNIX command line utility) or a richer representation (as in the JSON blobs
  returned by PageFreezer) considers HTML semantics.
* Annotation: a set of key-value pairs characterizing a given Change, submitted
  by a human analyst or generated by an automated process. A given Change might
  be annotated by multiple analysts, thus creating multiple Annotations per
  Change.

For more detail, see the Schema section below.

### Architecture

The project is currently divided into several repositories handling complementary aspects of the task. They can be developed and upgraded semi-independently, communicating via agreed-upon interfaces. For additional information, you can contact the active maintainers listed alongside each repo or our Project Manager, [@weatherpattern](https://github.com/weatherpattern):
* [**web-monitoring-db**](https://github.com/edgi-govdata-archiving/web-monitoring-db) ([@Mr0grog](https://github.com/Mr0grog))
  A Ruby on Rails app that serves database data via a REST API, serves diffs and collects human-entered annotations.
* [**web-monitoring-ui**](https://github.com/edgi-govdata-archiving/web-monitoring-ui) ([@lightandluck](https://github.com/lightandluck))
  React front-end that provides useful views of the diffs. It
  communicates with the Rails app via JSON.
* [**web-monitoring-processing**](https://github.com/edgi-govdata-archiving/web-monitoring-processing) ([@danielballan](https://github.com/danielballan))
  A Python backend ingests new captured HTML, computes diffs (for now, by
  querying PageFreezer), performs prioritization/filtering, and populates
  databases for Rails app.
* [**web-monitoring-versionista-scraper**](https://github.com/edgi-govdata-archiving/web-monitoring-versionista-scraper) ([@Mr0grog](https://github.com/Mr0grog))
  A set of Node.js scripts used to extract data from Versionista and load it into the database. It also generates the CSV files that analysts currently use in Google Spreadsheets to review changes. This project runs on its own, but in the future may be managed by or merged into `web-monitoring-processing`.

### Deployment Plan

The software will be deployed on Google Cloud, with each component running in a
separate Docker container.

### Identifying "Meaningful Changes"

The vast majority of changes to web pages are not relevant to analysts and we want to avoid presenting those irrelevant changes to analysts at all. It is, of course, not trivial to identify "meaningful" changes immediately, and we expect that analysts will always be involved in a decision about whether some changes are "important" or not. However, as we expand from 10<sup>4</sup> to 10<sup>7</sup> web pages, we need to drastically reduce the number of pages that analysts look at. 

Some examples of **meaningless** changes: 
- it's not unusual for a page to have a view counter on the bottom. In this case, the page changes **by definition** every time you view it.
- many sites have "content sliders" or news feeds that update periodically. This change may be "meaningful", in that it's interesting to see news updates. But it's only interesting once, not (as is sometimes seen) 1000 or 10000 times.

An example of a **meaningful** change: 
- In February, we noticed a systematic replacement of the word "impact" with the word "effect" on one website. This change is very interesting because while "impact" and "effect" have similar meanings, "impact" is a **stronger** word. So, there is an effort being made to **weaken** the language on existing sites. Our question is in part: what tools would we need in order to have this change **flagged** by our tools and presented to the analyst as **potentially interesting**?

### Sample Data

[`example-data`](./example-data) contains examples of website changes:

- `falsepos-...` files are cases any filter should catch
- `truepos...` files are cases of changes we care about

This is a small but illustrative sample. Many more samples will be made
available as soon as possible.

### Schema

This describes the schema of the SQL databases shared by the Rails app in
[**web-monitoring-db**](https://github.com/edgi-govdata-archiving/web-monitoring-db) and the Python processing backend in web-monitoring-processing.
Review the Definition of Terms section above, which corresponds to these tables.

Every table includes:

* uuid: UUID4 unique identifier
* created_at: internal detail of the database
* updated_at: internal detail of the database

in addition to the table-specific fields listed below.

#### Pages

* url: URL, which may be updated over time if a page is moved
* title: `<title>` tag
* agency: Government agency
* site: A category used to organizing Pages, loosely but not always the
  subdomain of the URL.

#### Versions

* page_uuid: reference to a Page
* capture_time: when this snapshot of the Page was acquired
* uri: path to stored (HTML) data; could be a file path, S3 bucket, etc.
* version_hash: sha256 hash of stored data
* source_type: name of source (such as 'Internet Archive')
* source_metadata: JSON blob of extra info particular to the source.
  This field is free-form, but we generally expect the following content for a
  given `source_type`:
  * `source_type: 'versionista'`
    * `account`: A string identifying which Versionista account the data came from. This will generally be `versionista1` or `versionista2`.
    * `site_id`: ID of the site in Versionista
    * `page_id`: ID of the page in Versionista
    * `version_id`: ID of the version in Versionista
    * `url`: The full URL to view this version in Versionista. You’ll need to be logged into the appropriate Versionista account to make use of it.
    * `diff_with_previous_url`: URL to diff view in Versionista (comparing with previous version)
    * `diff_length`: Length (in characters) of the diff identified by the above `diff_with_previous_url`.
    * `diff_hash`: SHA 256 hash of the above diff identified by `diff_with_previous_url`.
    * `diff_with_first_url`: URL to diff view in Versionista (comparing with the first recorded version)
    * `has_content`: Boolean indicating whether Versionista had raw content for this version. If this is true, the version’s `uri` should have a value (and vice-versa).
    * `error_code`: If HTTP status code returned to Versionista when it originally scraped the page was a non-200 (OK) status, this property will be present. Its value is the status code of the response, e.g. `403`, `500`, etc.

#### Changes

* uuid_from: reference to the "before" Version
* uuid_to: reference to the "after" Version
* priority: a number between 0 and 1 where 1 is high priority
* current_annotation: a JSON blob production a materialized reduction of one or
  more submitted Annotations, resolving conflicts in some way yet to be
  determined

####  Diffs

* change_uuid: reference to a Change that this Diff represents
* uri: path to stored diff data; could be a file path, S3 bucket, etc.
* diff_hash: sha256 has of stored diff data
* source_type: name of diffing utility (such as 'PageFreezer')
* source_metadata: JSON blob of extra info particular to the source

#### Annotations

* change_uuid: reference to a Change that this Annotation characterizes
* annotation: JSON blob
* author: user id

(This summary omits Users and Invitations, which are implemented in the Rails
app.)

For more details see the [Python implementation](https://github.com/edgi-govdata-archiving/web-monitoring-processing/blob/master/web_monitoring/db.py) and the [Ruby implementation (currently in progress)](https://github.com/edgi-govdata-archiving/web-monitoring-db/pull/15).

## Contributing

Don't forget to check out the "How To Help" section above.

See our [contributor guidelines](https://github.com/edgi-govdata-archiving/web-monitoring/blob/master/CONTRIBUTING.md).

This project wouldn’t exist without a lot of amazing people’s help. Thanks to the following for their work reviewing URL's, monitoring changes, writing [reports](https://envirodatagov.org/website-monitoring/), and a slew of so many other things!

<!-- ALL-CONTRIBUTORS-LIST:START -->
| Contributions | Name |
| ----: | :---- |
| [🔢](# "Content") | Chris Amoss |
| [🔢](# "Content") [📋](# "Organizer") [🤔](# "Ideas and Planning") | Maya Anjur-Dietrich |
| [🔢](# "Content") | Marcy Beck |
| [🔢](# "Content") [📋](# "Organizer") [🤔](# "Ideas and Planning") | Andrew Bergman |
| [🔢](# "Content") | Madelaine Britt |
| [🔢](# "Content") | Ed Byrne |
| [🔢](# "Content") | Morgan Currie |
| [🔢](# "Content") | Justin Derry |
| [🔢](# "Content") [📋](# "Organizer") [🤔](# "Ideas and Planning") | Gretchen Gehrke |
| [🔢](# "Content") | Jon Gobeil |
| [🔢](# "Content") | Pamela Jao |
| [🔢](# "Content") | Sara Johns |
| [🔢](# "Content") | Abby Klionski |
| [🔢](# "Content") | Katherine Kulik|
| [🔢](# "Content") | Aaron Lamelin |
| [🔢](# "Content") [📋](# "Organizer") [🤔](# "Ideas and Planning") | Rebecca Lave |
| [🔢](# "Content") | Eric Nost |
| [📖](# "Documentation") | Karna Patel |
| [🔢](# "Content") | Lindsay Poirier |
| [🔢](# "Content") [📋](# "Organizer") [🤔](# "Ideas and Planning") | Toly Rinberg|
| [🔢](# "Content") | Justin Schell |
| [🔢](# "Content") | Lauren Scott |
| [🤔](# "Ideas and Planning") [🔍](# "Funding/Grant Finders")| Nick Shapiro |
| [🔢](# "Content") | Miranda Sinnott-Armstrong |
| [🔢](# "Content") | Julia Upfal |
| [🔢](# "Content") | Tyler Wedrosky |
| [🔢](# "Content") | Adam Wizon |
| [🔢](# "Content") | Jacob Wylie |

<!-- ALL-CONTRIBUTORS-LIST:END -->

(For a key to the contribution emoji or more info on this format, check out [“All Contributors.”](https://github.com/kentcdodds/all-contributors))


## Sponsors & Partners

Finally, we want to give a huge thanks to partner organizations that have helped to support this project with their tools and services:

- [Amazon Web Services](https://aws.amazon.com/)
- [Sentry.io](https://sentry.io)
- [PageFreezer](https://www.pagefreezer.com)
- [Google Cloud Platform](https://cloud.google.com)
- [Google Summer of Code](https://summerofcode.withgoogle.com)
- [DataKind](http://www.datakind.org/)


## License & Copyright

Copyright (C) 2017 Environmental Data and Governance Initiative (EDGI) <br /> <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/80x15.png" /></a> Web Monitoring documentation is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>. See the [`LICENSE`](https://github.com/edgi-govdata-archiving/web-monitoring/blob/master/LICENSE) file for details.

   [onboarding]: https://trello.com/b/FCGGEaQq/edgi-web-monitoring-project-onboarding

