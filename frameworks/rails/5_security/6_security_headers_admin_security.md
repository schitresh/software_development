## HTTP Security Headers
### Default Security Headers
- By default rails is configured to return the following response headers
- Can be overrided in config/application: config.action_dispatch.default_headers
- X-Frame-Options
  - Indicates if a browser can render the page in a frame, iframe, embed, object tag
  - Set to SAMEORIGIN by default to allow framing on the same domain only
  - Set it to DENY to deny framing at all
  - Remove this header compeletely if you want to allow framing on all domains
- X-XSS-Protection
  - A deprecated legacy header
  - Set to 0 in Rails by default to disable problematic legacy XSS auditors
- X-Content-Type-Options
  - Set to 'nosniff' in Rails by default
  - Stops the browser from guessing the MIME type of a file
- X-Permitted-Cross-Domain-Policies
  - Set to none in Rails by default
  - Disallows Adobe Flash and PDF clients from embedding your page on other domains
- Referrer-Policy
  - Set to strict-origin-when-cross-origin in Rails by default
  - For cross-origin requests, this only sends the origin in the Referer header
  - Prevents leaks of private data that may be accessible from other parts of the full URL
    - Such as the path and query string

### Strict-Transport-Security Header
- Makes sure the browser automatically upgrades to HTTPS for current and future connections
- Added to the response when enabling the force_ssl option
  - config.force_ssl = true

### Content-Security-Policy Header
- To help protect against XSS and injection attacks
  - It is recommended to define a Content-Security-Policy response header
  - Defined in config/initializers/content_security_policy.rb

#### Adding a Nonce
- If you are considering 'unsafe-inline', consider using nonces instead
- Nonces provide a substantial improvement over 'unsafe-inline'
- When implementing a Content Security Policy on top of existing code
- Using `SecureRandom.base64(16)` is a good default value
  - Because it will generate a new random nonce for each request
- However, this method is incompatible with conditional GET caching
  - Because new nonces will result in new ETag values for every request
  - An alternative to per-request random nonces would be to use the session id
  - Its security depends on the session id being sufficiently random
    - And not being exposed in insecure cookies
  ```rb
  Rails.application.config.content_security_policy_nonce_generator =
    -> request { request.session.id.to_s }
  ```
- Once nonce generation is configured in an initializer
  - Automatic nonce values can be added to script tags by passing nonce: true
  ```js
  <%= javascript_tag nonce: true do %>
    alert('Hello, World!');
  <% end %>

  <%= javascript_include_tag "script", nonce: true %>

  // Per-session nonce value for allowing inline <script> tags
  <head>
    <%= csp_meta_tag %>
  </head>
  ```

### Feature-Policy Header
- Renamed to Permissions-Policy
- Requires a different implementation and isn't yet supported by all browsers

```rb
# config/initializers/permissions_policy.rb
Rails.application.config.permissions_policy do |policy|
  policy.camera      :none
  policy.gyroscope   :none
  policy.microphone  :none
  policy.usb         :none
  policy.fullscreen  :self
  policy.payment     :self, "https://secure.example.com"
end
```

### Cross-Origin Resource Sharing (CORS)
- Browsers restrict cross-origin HTTP requests initiated from scripts
- If you want to run Rails as an API, and run a frontend app on a separate domain
  - You need to enable Cross-Origin Resource Sharing (CORS)
- You can use the Rack CORS middleware for handling CORS
  - If you've generated your application with the --api option
  - Rack CORS has probably already been configured and you can skip these steps
  - To get started, add the rack-cors gem to your Gemfile: `gem 'rack-cors'`

```rb
# config/initializers/cors.rb
Rails.application.config.middleware.insert_before 0, "Rack::Cors" do
  allow do
    origins 'example.com'

    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

## Intranet and Admin Security
- Intranet and administration interfaces are popular attack targets
  - Because they allow privileged access
  - Highest threats are XSS and CSRF

### Cross-Site Scripting
- If your app re-displays malicious user input from extranet, it's vulnerable to XSS
- Having one single place in the admin interface or intranet
  - Where the input has not been sanitized makes the entire app vulnerable
- Possible exploits include
  - Stealing the privileged administrator's cookie
  - Injecting an iframe to steal the administrator's password
  - Installing malicious software through browser security holes
    - To take over the administrator's computer

### Cross-Site Request Forgery
- Cross-Site Request Forgery (CSRF), also known as Cross-Site Reference Forgery (XSRF)
- Allows the attacker to do everything the administrator or intranet user may do
- Example: Router reconfiguration by CSRF
  - Attackers sent a malicious e-mail, with CSRF in it
    - It claimed there was an e-card waiting for the user
    - It contained an image tag that resulted in an HTTP-GET request
    - To reconfigure the user's router
  - The request changed the DNS-settings
    - So that requests would be mapped to the attacker's site
    - Everyone saw the attacker's fake website and had their credentials stolen

### Additional Precautions
- Common admin interface works like this
  - It's located at www.example.com/admin
  - May be accessed only if the admin flag is set in the User model
  - Re-displays user input and allows the admin to delete/add/edit desired data
- It is very important to think about the worst case
  - What if someone really got hold of your cookies or user credentials
  - Introduce roles for the admin interface to limit the possibilities of the attacker
  - Or how about special login credentials for the admin interface
  - Or a special password for very serious actions
- Does the admin really have to access the interface from everywhere in the world
  - Limiting login to a bunch of source IP addresses
  - Examine request.remote_ip to find out the user's IP address
  - This is not bullet-proof but a great barrier, though there might be a proxy in use
- Put the admin interface to a special subdomain such as admin.application.com
  - And make it a separate application with its own user management
  - This makes impossible to steal an admin cookie from the usual domain
  - This is because of the same origin policy in your browser

## Unsafe Query Generation
- Due to the way Active Record interprets parameters
  - In combination with the way that Rack parses query parameters
  - It was possible to issue unexpected database queries with IS NULL where clauses
  - deep_munge method was introduced as a solution to keep Rails secure by default

```rb
# When params[:token] is one of: [nil], [nil, nil, ...] or ['foo', nil]
# it will bypass the test for nil, but IS NULL or IN ('foo', NULL) where clauses
# still will be added to the SQL query
# To keep Rails secure by default, deep_munge replaces some of the values with nil
# null -> nil, [null] -> [], [null, null, ...] -> [], ['foo', null] => ['foo']
unless params[:token].nil?
  user = User.find_by_token(params[:token])
  user.reset_password!
end
```
