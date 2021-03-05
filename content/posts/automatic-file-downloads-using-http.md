---
title: "Triggering File Downloads Using HTML and HTTP"
date: 2021-02-20T08:58:37-05:00
author: Brian Penguin
draft: false
---

Here's a neat little problem I've recently run across. I've got a web-server which takes various data and serves a Report at a url that looks like **https://my-server.com/reports/1** where 1 is the id of the report. All the reports are generated on the fly based on the latest data (No Caching here). Users have been asking if it's possible to generate the report as a PDF they might send to customers and add a link to the app to download the PDF directly. No Problem at all we'll allow **.pdf** as a response type and generate it on request! These files are small and generally can be created from the existing HTML in a reasonable response time.

There's already an HTML attribute for specifying that a link should trigger the browser download so we can use that to tell our app this is something to save.
```html
<a href="/reports/1.pdf" download> Download Report </a>
```

Voila! We've done it. Only there's a few things we could do to make this a little nicer for our users. When you click on the link, the file that's downloaded is always named **1.pdf**. This isn't the best. The second time a user clicks that download link a new download will be attempted with the same **1.pdf** but potentially different data. Depending on your operating system's name incrementing scheme usually you'll get a file that looks like **1(1).pdf**. Instead, lets generate a new scheme for the file name something like **REPORT-NAME_YEAR-MONTH-DAY.pdf**. Our reports are rarely generated more than once a week so we won't need to go any more granular with our date-time.

To tell the browser what to name the file we can use the download attribute again, this time assigning it our generated file name value. Our HTML will now look like this:
```html
<a href="/reports/1.pdf" download="My-Awesome-Report_2021-02-20.pdf"> Download Report </a>
```

Awesome work! But we've got one more little wrinkle. In some of the use cases our users are hitting the link from some external location, maybe it's an email or some CMS that's generating the link for reports on it's own. Those users are still downloading the **1.pdf** name files from those locations! We want to make sure that every link gets the same naming scheme.
