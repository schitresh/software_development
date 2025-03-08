## Forms
```js
// Generic Form
<%= form_with url: "/search", method: :get do |form| %>
  <%= form.label :query, "Search for:" %>
  <%= form.text_field :query %>
  <%= form.submit "Search" %>
<% end %>

// Model Form: Create or update @article
<%= form_with model: @article do |form| %>
  <%= form.text_field :title %>
  <%= form.text_area :body, size: "60x10" %>
  <%= form.submit %>
<% end %>

// Handling Association: ContactDetail
<%= form_with model: @person do |person_form| %>
  <%= person_form.text_field :name %>
  <%= fields_for :contact_detail, @person.contact_detail do |contact_detail_form| %>
    <%= contact_detail_form.text_field :phone_number %>
  <% end %>
<% end %>

// Handling Custom Route
form_with(model: @article, url: article_path(@article), method: :patch)
// Namespaces
form_with(model: [:admin, @article])
```

## Elements
```js
<%= form.text_area :message, size: "70x5" %>
<%= form.hidden_field :parent_id, value: "foo" %>
<%= form.password_field :password %>
<%= form.number_field :price, in: 1.0..20.0, step: 0.5 %>
<%= form.date_field :born_on %>
<%= form.time_field :started_at %>

// Select
<%= form.select :city, ["Berlin", "Chicago", "Madrid"] %>
// To assign different values (Chicago will be label & CHI will be value)
<%= form.select :city, [["Berlin", "BE"], ["Chicago", "CHI"], ["Madrid", "MD"]] %>
<%= form.select :city_id, City.order(:name).map { |city| [city.name, city.id] } %>

// Checkboxes
<%= form.check_box :pet_dog %>
<%= form.label :pet_dog, "I own a dog" %>
<%= form.check_box :pet_cat %>
<%= form.label :pet_cat, "I own a cat" %>

// Radio Buttons
<%= form.radio_button :age, "child" %>
<%= form.label :age_child, "I am younger than 21" %>
<%= form.radio_button :age, "adult" %>
<%= form.label :age_adult, "I am over 21" %>
```
