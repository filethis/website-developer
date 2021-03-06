# Overview

Having created a FileThis user account and user access token, you can now inject these into the instance of the FileThisConnect web component that is embedded in the development tool's main panel. To turn it on, click the "power" button at the top right of the window. You should see the component render, showing a list of company logos for websites from which we can fetch documents. The component is now "alive" and communicating with the FileThis production server.

If an error occurs during the use of the web component instance, an error dialog will be displayed by the demo app, and the component will be turned off, as indicated by the power button at the top right. The expiration of the user access token is one such error case. When you embed the component into your product's website, you will likely wire up your own handler for error events to deal with things in your own way.

Depending on which variant of the web component you selected, you will see one or more of the "panel" sub-components at different points in the user workflows. We describe each of these, next.

## The Sites Panel

![Thumbnail](assets/ft-source-panel.png)

The first step in any FileThis user workflow is to allow the user to find and select a company website from which they can fetch their documents. The FileThisConnect web component variants all begin by presenting the user with a list of all companies to which we can connect. We have a sub-component named [ft-source-panel](https://filethis.github.io/ft-source-panel/components/ft-source-panel/) which provides this functionality.

The web component instance in your development tool app begins by displaying a list of companies in the ft-source-panel. Note that users can narrow down which companies are displayed by selecting a filter preset from a popup menu, and/or by entering a search string which matches company names. When the user recognizes a company to which they want to connect, they click its logo. When they do so, the component poses a modal dialog that prompts them to enter their username and password for the site's account:

![Thumbnail](assets/ft-create-connection-dialog.png)

 When the user commits the dialog, the component tells the FileThis server to create the connection and kick off the document-fetching operation.

You're welcome to test fetching documents from any website for which you have valid credentials, but for the sake of convenience you might begin by connecting to our test website. Depending on the fake password string you enter when you connect to the test site, you can explore a variety of different user workflows in a deterministic manner. You can find the test site in the company list by typing "test" into the search field. For example, you can choose a "happy path" that always returns a list of fake billing statements, never interrupting the workflow with a challenge question. The credentials that specify this workflow are:

```
Username: user
Password: bills
```

Or, you can cause the site to challenge you with any of several kinds of questions. [This page](https://filethis.github.io/developer-docs/pdf/filethis-test-site-usage.pdf) documents the variety of use cases that the test site can simulate.

## The Connections Panel

![Thumbnail](assets/ft-connection-panel.png)

Having created a connection, either to our test site, or a real-world company site, you will see a representation of the connection appear in a list in the next panel —an instance of our [ft-connection-panel](https://filethis.github.io/ft-connection-panel/components/ft-connection-panel/) subcomponent.

As soon as a connection is created, our server kicks off the first fetching job to retrieve documents from the site. The fetching process takes about as long to complete as it would take a speedy user to log in directly to the company site in their web browser, navigate to the download page, click the appropriate button to start the download, wait for available documents in a certain date range to finish downloading, and then log out. Because this process isn't instantaneous, the web component provides feedback to users to give them a sense of progress and to show them that they're being saved from having to do some rather tedious manual work.

Note that an indeterminate progress indicator spins within the connection item that you created, indicating that work is being done. As soon as each document is downloaded by our service, you will see a thumbnail of the document's first page appear in the web component. After the last document appears, the progress indicator in the connection item stops spinning, and it returns to a resting state, showing the date of the last successful fetch operation.

It's not uncommon for websites to pose challenge questions after users log in successfully with their username and password. These can be questions like "What is your mother's maiden name?", or multiple-choice questions, or a prompt for a PIN code. When our fetching service encounters such a question on a website, it must be conveyed to the user on your website by some means, so that it can be answered by them. The answer they provide must then be sent back to our fetching service so that the operation can resume. The FileThisConnect web component handles this back-and-forth for you, dynamically rendering a modal dialog on your website to pose the question to the user, collecting their answer, and returning it to our service. For example:

![Thumbnail](assets/ft-user-interaction-form.png)

Our test site provides a convenient and deterministic way to test a variety of workflows that include challenge questions of different types. Again, [this document](https://filethis.github.io/developer-docs/pdf/filethis-test-site-usage.pdf) tells you which fake password string to use to determine the workflow to simulate.

If you're curious about the data schema used internally by FileThis to represent challenge questions their and answers, take a look at [this application](https://filethis.github.io/ft-user-interactions-demo/). The left-side column shows the JSON data that represents the question. The center column is a live instance of our [ft-user-interaction-form](https://filethis.github.io/ft-user-interaction-form/components/ft-user-interaction-form/) sub-component which dynamically renders the question data as a modal dialog. The right-side column displays the JSON data that represents the answer that the user enters into the dialog.

## The Documents Panel

![Thumbnail](assets/ft-document-panel.png)

We mentioned above that as documents start to be fetched from a company website for the connections created by a user, we display thumbnails of their first page in our web component. This uses an instance of our [ft-document-panel](https://filethis.github.io/ft-document-panel/components/ft-document-panel/) sub-component.


## The Next Step

In the next section you'll look behind the live web component to see the code that is used to embed it into an HTML page.
