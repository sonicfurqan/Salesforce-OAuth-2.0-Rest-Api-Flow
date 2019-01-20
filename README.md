# Salesforce-OAuth-2.0-Rest-Api-Flow
Step by step process of authentication with salesforce using OAUTH2.0 


Step 1 : Creating Connected App
     a. select
        Full access (full)
        Perform requests on your behalf at any time (refresh_token, offline_access)
        in Selected OAuth Scopes 
     b.Create a VF page and add its url to Callback URL
     
Step 2 : (URL is to opend by user ) 
        Calling end point 'https://login.salesforce.com/services/oauth2/authorize' to ask user to authorize access 
         Paramters
         a.response_type : it should be 'code'
         b.client_id : connected app's customer key
         c.redirect_uri : remeber vf page created in first step, addedd its url to callback url of connected app, Add same url to this parameter
         d.login_hint: user name 
         e.prompt= it should be 'login'
         f.state = Pass value that you want to be returned back after user authorize the access
         
 Step 3.  (POST method)(Header > 'Authorization' : Base64 encoded (ConsumerKey+':'+ConsumerSecret))
          After user authorize then he is redireced to url sepecifed in redirect_uri paramter od step 2
          Parameters return in url
          a.code : Contans request token using which access token is to be requested
          b.state : value you passed in step 2 is return back
          c.error : if any error is encounterd then that is returned
          
          Calling end point 'https://login.salesforce.com/services/oauth2/token' to get access token based on token key return from step 1
          Paramters
          a.grant_type = it should be 'authorization_code'
          b.code = value returned in code paramters should be added here
          c.client_id = ConsumerKey of connected app
          d.client_secret = clientSecret of connected app
          e.redirect_uri =  remeber vf page created in first step, addedd its url to callback url of connected app, Add same url to this parameter
          after makeing rest call
          paramters returned are
          a.access_token 
          b.instance_url
          c.refresh_token : using this token we can request new access token
          
Step 4.  (POST method)(Header > 'Authorization' : Base64 encoded (ConsumerKey+':'+ConsumerSecret))
          Calling end point 'https://login.salesforce.com/services/oauth2/token' to get access token based on refresh token returned in step 3
          Parameters
          a.grant_type : it shlud be 'refresh_token'
          c.client_id = ConsumerKey of connected app
          d.client_secret = clientSecret of connected app
          e.refresh_token = refresh token returnd in step 3 that value is to be given here
          
          after makeing rest call
          paramters returned are
          a.access_token 
          b.instance_url
          
Ref:https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_oauth_endpoints.htm
