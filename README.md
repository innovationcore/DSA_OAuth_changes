# DSA_OAuth_changes

In digital_slide_archive/devops/dsa/provision.yaml, set the following:

```
pip:
  - girder-oauth==3.2.3.dev12
rebuild-client: True
```

File          	Girder Container File Path

`user.py`   =>  `/opt/girder/girder/api/v1/user.py`

`App.js`    =>  `/opt/girder/girder/web_client/src/views/App.js`

`rest.js`   =>  `/opt/girder/girder/web_client/src/rest.js`


Explanation of Changes:<br>
`user.py`: <br>
&nbsp;&nbsp;&nbsp;&nbsp;Changed the getMe() function to redirect to OAuth Sign-In link if user is not already signed in<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- This would need to change for general Girder implementations, but for our use case where we <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;want the user to always be logged in with their credentials, this works.\n<br>
&nbsp;&nbsp;&nbsp;&nbsp;Added a default First and Last name value to the user in the case that it is null. <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- For OAuth it seems that some users (particularly external users in the org) can have no set<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;first or last name, which causes Girder to break. This was addressed by adding default values<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for those fields.<br>
`App.js`:<br>
&nbsp;&nbsp;&nbsp;&nbsp;Added handling for the redirect in user.py to circumvent CORS issues.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- This would need to be adjusted for any changes to the edited behavior in user.py.<br>
`rest.js`:<br>
&nbsp;&nbsp;&nbsp;&nbsp;Added handling to set girderToken cookie if it is not set but the girderToken is<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Girder OAuth checks for the girderToken cookie, but that cookie is not set when signing in <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;with OAuth. This breaks lots of endpoints that check for the cookie to authenticate, <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;including EventStream routes.<br>
