## Responses
- There are three ways to create HTTP responses
  - render: Renders a view
  - redirect_to: Redirects to another path
  - head: Sends only headers to the browsers
- For BooksController and index action
  - A corresponding view should be present at 'app/views/books/index.html.erb'
  - It's not required to explicitely render anything from controller
  - Just the presence of action method & this file will render it at the route

## Render
```rb
def update
  @book = Book.find(params[:id])
  if @book.update(book_params)
    redirect_to(@book)
  else
    render :edit, status: :unprocessable_entity
  end
end

# To render views for products from books controller
render "products/show"
render template: "products/show"

# Different Formats
render plain: "OK"
render json: @product
render xml: @product
render js: "alert('Hello Rails');"
render file: "#{Rails.root}/public/404.html", layout: false

# Default content_type is 'text/html'
render template: "feed", content_type: "application/rss"
render layout: "special_layout"
render layout: false

render status: 500
render status: :forbidden

# There should exist a template with the specified format
render formats: :xml
render formats: [:json, :xml]
```

## Redirect
```rb
redirect_to photos_url
redirect_back(fallback_location: root_path)

# Alternatives
def show
  @book = Book.find_by(id: params[:id])
  if @book.nil?
    # This will redirect to index and it will have to be rendered again
    redirect_to action: :index

    # A better option can be to handle the data here and render index directly
    @books = Book.all
    flash.now[:alert] = "Your book was not found"
    render 'index'
  end
end
```

## Layouts
- Rails combines the view with a layout
- Layout is a basic structure that needs to be rendered everywhere
  - For example, menu, navigation bar, headers, footers, etc
- It provides a yield method where the view is required to render
- To yield specific sections, content_for is specified

```js
// Layout
<html>
  <head>
  <%= yield :head %>
  </head>
  <body>
  <%= yield %>
  </body>
</html>

// View
<% content_for :head do %>
  <title>A simple page</title>
<% end %>

<p>Hello, Rails!</p>
```

### Layout Rendering
- To find the current layout, rails first looks in 'apps/views/layouts'
- For PhotosController, it will look for 'app/views/layouts/photos.html.erb'
- If not found, it will use 'app/views/layouts/application.html.erb'

```rb
# To override default conventions for layout
# They also cascade downward in hierarchy
class ProductsController < ApplicationController
  layout "inventory"
  layout "p
  roduct", only: [:index, :rss]
end
```

## Partials
- Renders a partial view inside a view

```js
<%= render "application/ad_banner" %>
<h1>Products</h1>
<%= render "application/footer" %>

// Passing variables
<h1>Editing zone</h1>
<%= render partial: "form", locals: { zone: @zone } %>

// Renders show page of customer
<%= render @customer %>
// Renders index page of articles
<%= render @articles %>

// Collection
// Will render partial for each product
<h1>Products</h1>
<%= render partial: "product", collection: @products %>
```
