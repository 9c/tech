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


outh provider:

1. [doorkeeper](https://github.com/applicake/doorkeeper)
2. [rack oauth2](https://github.com/nov/rack-oauth2)
