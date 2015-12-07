App Engine application for the Udacity training course.

## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool


# TASK 1 Explanation of Design Choices

In structuring the back-end for this application, the Session is created as a child of Conference, since a conference is composed of various conferences and it is important to have all sessions associated with a particular conference.

For the Session kind, the following attributes were created as basic information needed for any potential conference attendee: name (of session), hightlights, speaker, duration (in minutes), type of session (workshop, lecture, seminar, etc.), date, and the start time. Two additional attributes included the websafeConferenceKey of the Conference ancestor of the sessions and the [session] key. You although you can get the ancestor key (websafeConferenceKey) if you know the entity key, I included a specific field for so that you could always get reference the associated conference without having an server-side code manipulation to get there. This might be useful if they application was expanded. It would minimize future lines of code written to translate the session entity key into the conference key. That said, it is not completely necessary and could be removed. The key field is for the session entity key.

For the typeOfsession attribute, I left it as a regular string field which leaves flexibility. If this were a real work application, it would give me a chance to collect information from users on what types of sessions are actually associated with conferences rather than locking a set of pre-defined choices. I did comment-out the potential to use pre-defined choices once it becomes clearer what types of sessions are actually used.

The speaker attribute was designed simply as a attribute to Session and not it's own unique kind (and object). There could be some major advantages to creating the speaker as a kind (and object). You could include fields associated with each speaker such as occupation, university, degrees, company, email address (and other contact methods), etc. I chose not to go this route for the sake of simplicity. I created a speaker as a string entity.



# TASK 3 Explanation of Additonal Queries

getConferencesCreated() is a query that returns all the conferences created by a particular user. If someone was interested in attending all conferences created by a person. For example, a person in the admisions office at a university might create several conferences each year. A potential attendee might be interested in see all conferences created by that conference organizer. 

A more specific query that follows is getConferencesCreatedByCity(). This query returns the conferences created by a particular conference organizer and filters them by a particular city. Following the example above, you might only be interested in univesity admissions conferences organized by a particular organizer that are in your hometown of Philadelphia. 



# TASK 3 Query Problem explanation

The problem with the query problem is that the query has two parameters: (1) must not be a workshop session type AND (2) must be before 7pm. In the datastore, inequality filters are limited to one property. While it is obvious that the before-7pm-reqirement is an inequality filter on the start_time property, it should be recognized that the not-workshop filter would also utilize inequality filters. That is because the DOES NOT EQUAL != filter actually works by employing greater than > and less than < filters. In effect, by using a != filter, you would be using inequality filters on two different properties in teh same query which is not allowed.

To solve this problem, I would break the query into two parts. The first query would return all non-workshop sessions. A second query would be performed by using a for loop to select only sessions with a start time less than 7PM.




