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

#Design
Each Conference has an owner(ancestor) of kind Profile.
Each Session has an ancestor Conference and a speaker.
Adiitionally each Profile stores a list of conferences to attend and wishlist of sessions for a user.


#Additional queries
1)getSessionsToAttend - returns all Sessions that are a part of this conference that this user is registered to attend.
query would be something like:
Session.query(ndb.OR( Session.parentConference in Conference.query(ndb.OR( Conference.key in prof.conferenceKeysToAttend))))

2) addSpeakerToWishList - whenever this speaker is giving a session notify users who've added him/her to their wishlist.
   Similar logic to addSessionsToWishList, in Profile keep a list of wishlist for speakers 
   speakersWishToAttend = ndb.StringProperty(repeated=True)
in createSession :
    q = Profile.query()
    q = q.filter(Profile.speakersWishToAttend == data['speaker'])
    send mail via task to the following list:
    [prof.mainEmail for prof in q]

