## Action Mailer
- Allows sending emails using mailer classes (app/mailers) and views (app/views)
- To generate user mailer, run 'rails g mailer User'
- Work very similar to controllers, inherits 'ActionMailer::Base'
  - Have actions and corresponding view
  - Have action filters (before, after, around)_action
  - Have delivery callbacks (before, after, around)_delivery
- Mailer instances don't have any context about the request
  - So, *_path cannot be used inside an email
  - Need to use *_url for links

```rb
# app/mailers/user_mailer.rb
class UserMailer < ApplicationMailer
  default from: 'notifications@example.com'

  # Can also specify layout
  # layout 'awesome'

  def welcome_email
    @user = params[:user]

    mail(to: @user.email, subject: 'Welcome')
    # To send name of the person
    mail(to: email_address_with_name(@user.email, @user.name), subject: 'Welcome')
    # Custom template path
    mail(to: @user.email, subject: 'Welcome',
      template_path: 'notifications', template_name: 'another')
  end

  end
end

# app/controllers/users_controller.rb
class UsersController < ApplicationController
  def create
    # All attributes in 'with' are passed as params
    # Will send mail async using 'deliver_later'
    # Can use 'deliver_now' to send mail immediately
    UserMailer.with(user: @user).welcome_email.deliver_later
  end
end
```

```js
// app/views/user_mailer/welcome_email.html.rb
<!DOCTYPE html>
<html>
  <head>
    <meta content='text/html; charset=UTF-8' http-equiv='Content-Type'/>
  </head>
  <body>
    <h1>Welcome to example.com, <%= @user.name %></h1>
    <p>
      You have successfully signed up to example.com<br>
      Your username is: <%= @user.login %><br>
    </p>
  </body>
</html>
```

## Attachments
```rb
# Will guess the mime_type and set the encoding (base64)
attachments['filename.jpg'] = File.read('/path/to/filename.jpg')

# Inline Attachements
attachments.inline['image.jpg'] = File.read('/path/to/image.jpg')
# Included in view like this
# <%= image_tag attachments['image.jpg'].url %>
```

## Previews
- Renders email in browser for testing, added in 'test/mailers/previews'
- Available at localhost:3000/rails/mailers/

```rb
# test/mailers/previews/user_mailer_preview.rb
class UserMailerPreview < ActionMailer::Preview
  def welcome_email
    UserMailer.with(user: User.first).welcome_email
  end
end

```

## Intercepting Emails
- Makes modifications to emails before they are handed off to delivery agents
- Useful to avoid sending to actual emails in staging environment

```rb
class SandboxEmailInterceptor
  def self.delivering_email(message)
    message.to = ['sandbox@example.com']
  end
end

Rails.application.configure do
  if Rails.env.staging?
    config.action_mailer.interceptors = %w[SandboxEmailInterceptor]
  end
end
```

## Observing Emails
- Gives access to emaill message after it has been sent

```rb
class EmailDeliveryObserver
  def self.delivered_email(message)
    EmailDelivery.log(message)
  end
end

Rails.application.configure do
  config.action_mailer.observers = %w[EmailDeliveryObserver]
end
```
