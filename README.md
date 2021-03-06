OAuth 2.0
====

![abc](http://i.stack.imgur.com/3oDJt.png)


OAuth is protocol or specification focuses on client developer simplicity while providing specific authorization flows for web applications, desktop applications, mobile phones, and living room devices.


     +----------+         Client Identifier       +---------------+
     |         -+----(A)-- & Redirect URI ------->|               |
     | End User |                                 | Authorization |
     |    at    |<---(B)-- User authenticates --->|     Server    |
     | Browser  |                                 |               |
     |         -+----(C)-- Verification Code ----<|               |
     +-|----|---+                                 +---------------+
       |    |                                         ^      v
      (A)  (C)                                        |      |
       |    |                                         |      |
       ^    v                                         |      |
     +---------+                                      |      |
     |         |>---(D)-- Client Credentials, --------'      |
     |   Web   |           Verification Code,                |
     |  Client |            & Redirect URI                   |
     |         |                                             |
     |         |<---(E)------- Access Token -----------------'
     +---------+        (w/ Optional Refresh Token)


                                 Figure 4

   The web server flow illustrated in Figure 4 includes the following
   steps:

   (A)  The web client initiates the flow by redirecting the end user's
        user-agent to the authorization endpoint with its client
        identifier and a redirect URI to which the authorization server
        will send the end user back once authorization is received (or
        denied).

   (B)  The authorization server authenticates the end user (via the
        user-agent) and establishes whether the end user grants or
        denies the client's access request.

   (C)  Assuming the end user granted access, the authorization server
        redirects the user-agent back to the client to the redirection
        URI provided earlier.  The authorization includes a verification
        code for the client to use to obtain an access token.

   (D)  The client requests an access token from the authorization
        server by including its client credentials (identifier and
        secret), as well as the verification code received in the
        previous step.






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

```ruby
  def authorize_endpoint(allow_approval = false)
    Rack::OAuth2::Server::Authorize.new do |req, res|
      @client = Client.find_by_identifier(req.client_id) || req.bad_request!
      res.redirect_uri = @redirect_uri = req.verify_redirect_uri!(@client.redirect_uri)
      if allow_approval
        if params[:approve]
          case req.response_type
          when :code
            authorization_code = current_account.authorization_codes.create(:client_id => @client, :redirect_uri => res.redirect_uri)
            res.code = authorization_code.token
          when :token
            res.access_token = current_account.access_tokens.create(:client_id => @client).to_bearer_token
          end
          res.approve!
        else
          req.access_denied!
        end
      else
        @response_type = req.response_type
      end
    end
  end
```


### User grant

![user](http://asciicasts.com/system/photos/1195/original/E353I04.png)


```ruby
require 'oauth2'
client = OAuth2::Client.new('client_id', 'client_secret', :site => 'https://example.org')

client.auth_code.authorize_url(:redirect_uri => 'http://localhost:8080/oauth2/callback')
# => "https://example.org/oauth/authorization?response_type=code&client_id=client_id&redirect_uri=http://localhost:8080/oauth2/callback"

token = client.auth_code.get_token('authorization_code_value', :redirect_uri => 'http://localhost:8080/oauth2/callback', :headers => {'Authorization' => 'Basic some_password'})
response = token.get('/api/resource', :params => { 'query_foo' => 'bar' })
response.class.name
# => OAuth2::Response
```



outh provider:

1. [doorkeeper](https://github.com/applicake/doorkeeper)
2. [rack oauth2](https://github.com/nov/rack-oauth2)
3. [oauth2](https://github.com/intridea/oauth2)
