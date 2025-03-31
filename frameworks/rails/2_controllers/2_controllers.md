## Controller
- After the router has determined which controller to use for a request
  - Controller makes sense of the request and produces appropriate output
- Makes the model data available to the view
  - To display data to the user or save the user data to the model
- Plural names are used for file & class names
  - 'ClientsController' class in 'clients_controller.rb' file
- `controller_name` & `action_name` methods can be used to get routing info
- ApplicationController is inherited to individual controllers

```rb
class ClientsController < ApplicationController
  # GET /clients?status=activated
  def index
    if params[:status] == "activated"
      @clients = Client.activated
    else
      @clients = Client.inactivated
    end
  end

  # POST /clients, data will be sent as part of the request body
  def create
    @client = Client.new(params[:client])
    if @client.save
      redirect_to @client
    else
      render "new" # Overrides the default rendering of 'create' view
    end
  end
end
```

## Parameters
- `params` is used to access data including URL params & POST data
- URL params: params[:id] will contain id specified in /photos/:id
- POST data: Data sent in query string (everything after '?' in the url)

### Nested Params
- Array
  - `GET /clients?ids[]=1&ids[]=2&ids[]=3`
  - params[:ids] will be ['1', '2', '3']
- Hash
  - `<input type="text" name="client[address][city]" value="Carrot City"/>`
  - params[:address] will be { "city" => "Carrot City" }

### Strong Params
- Action Controller params are forbidden to be used in Active Model mass assignments
- They should be permitted first to be used, required params can also be specified

```rb
# Will raise an error
Person.create(params[:person])

person_params = params.require(:person).permit(:name, :age)
# To permit the entire hash or parameters
person_params = params.require(:person).permit!
# To permit nested params
params.require(:product).permit(:name, data: {}) # permits name & whole data attribute
params.permit(:name, { emails: [] }, friends: [:name, { family: [:name] }])
# Using `fetch` you can supply a default
params.fetch(:blog, {}).permit(:title, :author)
```

## Filters
- Methods that run before, after or around a controller action

```rb
class ApplicationController < ActionController::Base
  before_action :require_login

  private

    def require_login
      return if logged_in?
      flash[:error] = "You must be logged in to access this section"
      redirect_to new_login_url
    end
end

class LoginsController < ApplicationController
  before_action :do_something, only: [:action1, :action2]
  after_action :do_something_else, except: [:action1, :action2]
  skip_before_action :require_login, only: [:new, :create]
end

class ChangesController < ApplicationController
  around_action :wrap_in_transaction, only: :show

  private
    def wrap_in_transaction
      ActiveRecord::Base.transaction do
        yield
      end
    end
end
```

## Streaming and File Downloads
- `send_data` is used to send file data
- `send_file` is used to send an existing file on the disk
- To turn off streaming 'stream' option is used
- To adjust block size 'buffer_size' option is used

```rb
require "prawn"
class ClientsController < ApplicationController
  # Generates a PDF document and returns it as a string
  # This string will then be streamed to the client as a file download
  # If the file is not meant to be downloaded, convey it to browser
  # By setting disposition: :inline (default value is :attachment)
  def download_pdf
    send_data(generate_pdf, filename: "#{client.name}.pdf", type: "application/pdf")
  end

  private
    def generate_pdf
      Prawn::Document.new do
        text(client.name, align: :center)
        text("Address: #{client.address}")
      end.render
    end
end

# Using Format
class ClientsController < ApplicationController
  def show
    respond_to do |format|
      format.html
      # GET /clients/1.pdf
      format.pdf { render pdf: generate_pdf(client) }
    end
  end
end

# For live streaming, `ActionController::Live` module is included
response.headers['Content-Type'] = 'text/event-stream'
response.stream.write(text)`
```

## Rescue From
- Rails default exception handling displays 500 for all exceptions
- If there was a routing error or record was not found, it displays 404
- These messages are in 500.html & 404.html of public folder
  - These can be customized but are static files (erb, scss, etc. cannot be used)
- Do not use 'rescue_from' with Exception or StandardError
  - It can cause serious side-effects
  - And prevents rails from handling exceptions properly

```rb
class ApplicationController < ActionController::Base
  rescue_from User::NotAuthorized, with: :user_not_authorized

  private
    def user_not_authorized
      flash[:error] = "You don't have access to this section."
      redirect_back(fallback_location: root_path)
    end
end
```
