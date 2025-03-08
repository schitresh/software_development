## Action Cable
- Integrates web sockets and allows real-time features
- Uses web sockets instead of HTTP request-response protocol

## Components
- Connections
  - An action cable server handles multiple connection instances
  - It has one connection instance per web socket connection
  - A single user may have multiple web sockets through multiple browser tabs
- Consumers
  - Client of a web socket connection created by client-side JS framework
- Channels
  - Each channel encapsulates a logical unit of work (similar to controller in MVC)
  - Each consumer can subscribe multiple channels
- Subscribers
  - When a consumer is subscribed to a channel, it acts as subscriber
- Pub/Sub (Publish/Subscribe)
  - Message queue paradigm where publishers send data to subscribers
- Broadcastings
  - A pub/sub link where transmissions are sent directly to the channel subscribers

## Connections
- Callbacks: (before, after, around)_command

```rb
# app/channels/application_cable/connection.rb
module ApplicationCable
  class Connection < ActionCable::Connection::Base
    identified_by :current_user

    def connect
      self.current_user = find_verified_user
    end

    private
      def find_verified_user
        if verified_user = User.find_by(id: cookies.encrypted[:user_id])
          verified_user
        else
          reject_unauthorized_connection
        end
      end
  end
end
```

## Channels
- Callbacks: (before, after)_subscribe, (before, after)_unsubscrbe

```rb
# app/channels/application_cable/channel.rb
module ApplicationCable
  class Channel < ActionCable::Channel::Base
  end
end

# app/channels/chat_channel.rb
class ChatChannel < ApplicationCable::Channel
  # Called when the consumer has successfully
  # become a subscriber to this channel
  def subscribed
    # Streams provide the mechanism by which channels route published content
    # (broadcasts) to their subscribers
    stream_from "chat_#{params[:room]}"
  end
end
```

## Broadcasting
```rb
# Somewhere in your app this is called, perhaps from a NewCommentJob
ActionCable.server.broadcast(
  "chat_#{room}",
  {
    sent_by: 'Paul',
    body: 'This is a cool chat app'
  }
)
```

## Consumer
```js
// app/javascript/channels/consumer.js
// Action Cable provides the framework to deal with WebSockets in Rails
// You can generate new channels where WebSocket features live
// Using the `bin/rails generate channel` command
import { createConsumer } from "@rails/actioncable"

export default createConsumer()

// Specify a different URL to connect to
createConsumer('wss://example.com/cable')
// Or when using websockets over HTTP
createConsumer('https://ws.example.com/cable')
// Use a function to dynamically generate the URL
createConsumer(getWebSocketURL)

function getWebSocketURL() {
  const token = localStorage.get('auth-token')
  return `wss://example.com/cable?token=${token}`
}
```

## Subscriber
```js
// app/javascript/channels/chat_channel.js
import consumer from "./consumer"
// A consumer can act as a subscriber to a given channel any number of time
consumer.subscriptions.create({ channel: "ChatChannel", room: "1st Room" })
consumer.subscriptions.create({ channel: "ChatChannel", room: "2nd Room" })
consumer.subscriptions.create({ channel: "ChatChannel", room: "Best Room" }, {
  received(data) {
    this.appendLine(data)
  },

  appendLine(data) {
    const html = this.createLine(data)
    const element = document.querySelector("[data-chat-room='Best Room']")
    element.insertAdjacentHTML("beforeend", html)
  },

  createLine(data) {
    return `
      <article class="chat-line">
        <span class="speaker">${data["sent_by"]}</span>
        <span class="body">${data["body"]}</span>
      </article>
    `
  }
})
```
