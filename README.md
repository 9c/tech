OAuth 2.0
====

OAuth is protocol or specification focuses on client developer simplicity while providing specific authorization flows for web applications, desktop applications, mobile phones, and living room devices.


![abc](http://i.stack.imgur.com/3oDJt.png)

[reference](http://stackoverflow.com/questions/11631928/authenticating-with-oauth2-for-an-app-and-a-website)


### Twitter application

![twitter app](http://www.webdevdoor.com/wp-content/uploads/2013/02/twitter-feed-authentication-step2.jpg)



### build Oauth provider

```ruby


GET       /oauth/authorize
POST      /oauth/authorize
DELETE    /oauth/authorize
POST      /oauth/token
resources /oauth/applications
GET       /oauth/authorized_applications
DELETE    /oauth/authorized_applications/:id
GET       /oauth/token/info

```

### admin panel

![admin](http://asciicasts.com/system/photos/1194/original/E353I03.png)


### User grant

![user](http://asciicasts.com/system/photos/1195/original/E353I04.png)


outh provider:

1. [doorkeeper](https://github.com/applicake/doorkeeper)
2. [rack oauth2](https://github.com/nov/rack-oauth2)
