## Redirection
- Whenever the user is allowed to pass (parts of) the url for redirection
  - It is possibly vulnerable
- The most obvious attack would be to redirect users to a fake app
  - That looks and feels exactly as the original one
  - This so-called phishing attack works by sending an unsuspicious link in email
    - And injecting the link by XSS in the webapp
    - Or putting the link into an external site
  - It is unsuspicious because the link starts with the url to the webapp
    - The url to the malicious site is hidden in the redirection parameter
    - 'http://www.example.com/site/redirect?to=www.attacker.com'
  - Include only the expected parameters in a legacy action and permit parameters
    - If you redirect to a url, check it with a permitted list or a regex
- Another redirection and self-contained XSS attack works by using data protocol
  - This protocol displays its contents directly in the browser
  - And can be anything from html or javascript to entire images
  - For example, base64 encoded javascript which displays a simple message box
    - data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4K
  - In a redirection url, an attacker could redirect to this url with malicious code in it
  - Do not allow user to supply (parts of) the url to be redirected to

## File Uploads
- File names which the user may choose (partly) should always be filtered
  - As an attacker could use a malicious file name to overwrite any file on the server
- If you store file uploads at /var/www/uploads
  - And the user enters a file name like '../../../etc/passwd'
  - It may overwrite an important file
  - Of course, ruby interpreter would need the appropriate permissions to do so
  - One more reason to run web servers, db servers, etc. as less privileged unix user
- When filtering user input file names
  - Don't try to remove malicious parts
    - Think where the webapp removes all '../' in a file name
    - And attacker uses a string like '....//' and the result will be '../'
  - It is best to use a permitted list approach
    - Which checks for validity of a file name with a set of accepted characters
    - This is opposed to restricted list approach that removes disallowed chars
- A significant disadvantage of synchronous processing of file uploads
  - Is its vulnerability to denial of service attacks
  - An attacker can synchronously start image file uploads from many computers
    - Which increases server load and may eventually crash or stall the server
  - Process media files asynchronously
    - Save the media file and schedule a processing request in the database
    - A second process will handle the processing of the file in background

## Executable Code in Files
- The popular Apache web server has an option called DocumentRoot
  - This is the home directory of the website
  - Everything in this directory tree will be served by the web server
- If there are files with a certain file name extention
  - The code in it will be executed when requested (might require some options to be set)
  - Examples are .php & .cgi files
- If your Apache DocumentRoot points to rails' /public directory
  - Don't put file uploads in it, store at least one level upwards

## File Downloads
- send_file method sends files from the server to the client
  - If you use a file name (that the user entered) without filtering
  - Any file can be downloaded
  - Example: send_file('/var/www/uploads/' + params[:filename])
- Simply pass a file name like '../../../etc/passwd' to download the server's login info
  - A simple solution is to check that the requested file is in the expected directory
- Another approach is to store file names in database
  - And name the files on the disk after the ids in the database
  - This also avoids possible code in an uploaded file
  - attachment_fu plugin does this in a similar way

```rb
basename = File.expand_path('../../files', __dir__)
filename = File.expand_path(File.join(basename, @file.public_filename))
raise if basename != File.expand_path(File.dirname(filename))
send_file(filename, disposition: 'inline')
```
