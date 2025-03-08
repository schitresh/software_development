## Request Forgery Protection
- Cross-site request forgery is a type oof attack
  - In which a site tricks a user into making requests on another site
  - Possibly adding, modifying, or deleting data on that site
  - Without the user's knowledge or permission
- The first step to avoid this is to make sure that all destructive actions
  - That is, create, update, destroy can be accessed only with non-GET requests
- A malicious site can still send a non-GET request to your site
  - That's where request forgery protection conmes in
  - It adds a non-guessable token (which is only know to the server) to each request
- Rails adds this token to every form generated using form helpers
  - For a manual form or custom Ajax calls
    - It's available through `form_authenticity_token`

```js
<form accept-charset="UTF-8" action="/users/1" method="post">
  <input type="hidden"
         value="67250ab105eb5ad10851c00a5621854a23af5489"
         name="authenticity_token"/>
  <!-- fields -->
</form>
```

## HTTP Authentications
- Rails has three built-in HTTP authentication mechanisms
  - Basic Auth
  - Digest Auth
  - Token Auth

```rb
# Basic Auth
class AdminsController < ApplicationController
  http_basic_authenticate_with name: "humbaba", password: "5baa61e4"
end

# Digest Auth
class AdminsController < ApplicationController
  USERS = { "lifo" => "world" }
  before_action :authenticate

  private
    def authenticate
      authenticate_or_request_with_http_digest do |username|
        USERS[username]
      end
    end
end

# Token Auth
class PostsController < ApplicationController
  TOKEN = "secret"
  before_action :authenticate

  private
    def authenticate
      authenticate_or_request_with_http_token do |token, options|
        ActiveSupport::SecurityUtils.secure_compare(token, TOKEN)
      end
    end
end
```
