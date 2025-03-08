## Session
- The application has a session for each user
  - In which small amounts of data can be stored and persisted between requests
- Available storage mechanism in the module ActionDispatch::Session::
  - CookieStore: Stores everything on the client
  - CacheStore: Stores in rails cache
  - ActiveRecordStore: Stores in a database using ActiveRecord (requires session_store gem)
- All session stores use a cookie to store a unique ID for each session
  - Rails won't allow to pass session ID in URL as it is less secure
  - This ID is used to look up the session data on the server (e.g. in db table)
- Though the default & recommended session store (which is CookieStore)
  - Stores all session data in the cookie itself
  - This is very lightweight and requires no setup to use the session
  - Cookie data is cyptographically signed & encrypted to make it tamper-proof
  - This is done using the secret_key_base in credentials
- CookieStore can store around 4 kB of data
  - Much less than others but usually enough
  - Storing large amounts of data in the session is discouraged in any type of session store
- Sessions are lazily loaded

```rb
class LoginsController < ApplicationController
  def create
    if user = User.authenticate(params[:username], params[:password])
      session[:current_user_id] = user.id
      redirect_to root_url
    end
  end

  def destroy
    session.delete(:current_user_id)
    @_current_user = nil
    redirect_to root_url, status: :see_other
  end
end

class ApplicationController < ActionController::Base
  private
    def current_user
      @_current_user ||= session[:current_user_id] &&
        User.find_by(id: session[:current_user_id])
    end
end
```

### Flash
- Special part of session which is cleared with each request
- It is only available in the next request
  - Useful for passing error message, etc.
- If you want flash to be carried over to another request
  - Use `flash.keep` or `flash.keep(key)` before redirecting
- If you want to use flash in the same request
  - Use `flash.now` like `flash.now[:error] = 'message'` before render
  - For example, if create action fails and you render new action in the same request

```rb
class LoginsController < ApplicationController
  def destroy
    session.delete(:current_user_id)
    flash[:notice] = "You have successfully logged out."
    redirect_to root_url, status: :see_other
  end
end

redirect_to root_url, notice: "You have successfully logged out."
redirect_to root_url, alert: "You're stuck here!"
redirect_to root_url, flash: { referral_code: 1234 }
```

```js
<% flash.each do |name, msg| %>
  <%= content_tag :div, msg, class: name %>
<% end %>

<% if flash[:just_signed_up] %>
  <p class="welcome">Welcome to our site!</p>
<% end %>
```

## Cookies
- Used to store small amounts of data on the client in cookies
- Persisted across requests and even sessions

```rb
class CommentsController < ApplicationController
  def new
    @comment = Comment.new(author: cookies[:commenter_name])
  end

  def create
    # ...
    if params[:remember_name]
      cookies[:commenter_name] = @comment.author
    else
      cookies.delete(:commenter_name)
    end
  end
end

# Encrypted Cookie Jar
class CookiesController < ApplicationController
  def set_cookie
    cookies.encrypted[:expiration_date] = Date.tomorrow # Thu, 20 Mar 2014
    redirect_to action: 'read_cookie'
  end

  def read_cookie
    cookies.encrypted[:expiration_date] # "2014-03-20"
  end
end
```
