## Sessions
- Sessions enable the application to maintain user specific state
  - While users interact with the application
  - Allow users to authenticate once and remain signed in for future requests
  - Track contents of a shopping basket
- Authentication
  - User provides username & password and webapp authenticates them
  - After auth, app stores user id in session hash and the session is now valid
  - On every request, application will identify the user by its id in the session
  - The session id in the cookie identifies the session

## Session Hijacking
- Anyone who seizes a cookie from someone else may use the app as this user
- Sniffing the cookie in an insecure network
  - In an unencrypted wireless LAN, it is easy to listen the traffic of connected clients
  - Webapp should provide secure connection over SSL
  - In rails, this can be accomplished by forcing SSL connection in application config
    - config.force_ssl = true
- Not clearing out the cookies after working at a public terminal
  - Provide a logout button in the webapp and make it prominent

## Session Storage
- CookieStore saves the session hash in a cookie on the client-side
  - Server retrieves it from the cookies and eliminates the need for session ID
  - But cookies have a size limit of 4 kB
  - This increases the speed of app but there can be security concerns
- Avoid storing sensitive data in cookies
  - The client may preserve cookie contents even for expired cookies
  - The client may copy cookies to other machines
- Client may delete the cookie before expiration
  - Persist all data on the server side
  - Session cookies do not invalidate themselves and can be maliciously reused
    - App should invalidate old session cookies using a stored timestamp
- Rails encrypts cookies by default
  - Client cannot read or edit the contents without breaking encryption
  - Take appropriate care of your secrets to keep cookies secured
  - Use different salt values for encrypted and signed cookies

## Replay Attack
- A user receives credits, the amount is stored in session
  - Which is a bad idea anyway, but we'll use this for example
  - The user buys something and the new adjusted credit value in updated in session
  - The user copies the cookie during the first step and replaces the current cookie
  - The user has their original credit back
- Including a nonce (a random value) in the session solves replay attacks
  - It is valid only once and server keeps track of all valid nonces
  - If there are several application servers
    - Storing nonces in database would defeat the purpose of CookieStore
    - That is to avoid access to database
- The best solution is not to store this kind of data in session
  - But in the database

## Session Fixation
- Fixing a user's session ID and forcing the user's browser into using this ID
- The attacker creates a valid session ID
  - Load the login page and take the session ID in the cookie from the response
  - They keep session alive by accessing the app periodically
- They force the user's browser into using this session ID
  - They may not change a cookie of another domain (because of same origin policy)
  - They have to run a javascript from the domain of the target webapp
  - Injecting the javascript code into the app by XSS accomplishes this attack
  - `<script>document.cookie="_session_id=16d5b78abb28e3d6206b60f22a03c8d9";</script>`
- Attacker lures victim to the infected page with the javascript code
  - By viewing the page, the victim's browser will change
    - the session ID to the trap session ID
  - As the new trap session is unused, the webapp will require user to authenticate
  - From now on, the victim & the attacker will co-use the same session
  - The session became valid and the victim didn't notice the attack
- The most effective countermeasure is to issue a new session identifier
  - And declare the old one invalid after a successful login (reset_session)
  - This is a good countermeasure against session hijacking as well
  - Devise gem automatically expires sessions on sign in & sign out
- Another countermeasure is to save user specific properties in the session
  - Verify them every time a request comes in
  - Such properties can be remote IP address & user agent (browser name)
  - But many ISPs and organizations put their users behind proxies
  - And these properties might change over the course of a session

## Session Expiry
- We can set expiration for the cookie with the session ID
  - But the client can edit cookies that are stored in the web browser
  - So expiring sessions on the server is safer
- Need to check both updated_at & created_at
  - The attacker may be keeping session alive through session fixation

```rb
class Session < ApplicationRecord
  def self.sweep(time = 1.hour)
    where(updated_at: ...time.ago).or(where(created_at: ...2.days.ago)).delete_all
  end
end
```
