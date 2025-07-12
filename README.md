# Coupdoeil
[![Gem Version](https://badge.fury.io/rb/coupdoeil.svg?icon=si%3Arubygems&icon_color=%23ff2600)](https://badge.fury.io/rb/coupdoeil)

A framework to easily handle popovers.

This repository is a placeholder. The original repository is hosted on GitLab: [Coupdoeil repository on GitLab](https://gitlab.com/Pagehey/coupdoeil).

## Documentation
See [coupdoeil.org](https://coupdoeil.org) for full documentation.

There is also an [introduction article I wrote for the first release.](https://dev.to/pagehey/coupdoeil-ruby-gem-174a)

## Overview

### What is a Coupdoeil::Popover?

Popovers are ruby objects used to easily build powerful popups (from basic tooltips to full dialog popovers).
They are mostly inspired by GitHub popovers (like hovering on a link to a repo, or a username)
and Wikipedia popups (when hovering a link to another wikipedia article).

Click to toggle examples
<details>
    <summary><i>A quick look on a contact details</i></summary>
    <img src="https://coupdoeil.org/overview-example.gif" alt="popover example" />
</details>
<details>
    <summary><i>A form in a popup</i></summary>
    <img src="https://coupdoeil.org/overview-example-2.gif" alt="popover example" />
</details>
<details>
    <summary><i>A side menu with sub items</i></summary>
    <img src="https://coupdoeil.org/overview-example-3.gif" alt="side menu with sub items popover example" />
</details>

[See more use-cases in documentation](https://coupdoeil.org/case-studies.html)

The current implementation takes inspiration from both ViewComponent and ActionMailer, to make it as easy as possible to use while offering a wide range of possibilities.

```ruby
# app/popovers/contact_popover.rb
class ContactPopover < Coupdoeil::Popover
  def details
    @contact = params[:contact]
  end
end
```

```erb
<%# app/popovers/contact_popover/details.html.erb %>
<div class="contact-details">
  <%= image_tag @contact.avatar %>
  <strong><%= @contact.first_name %> <%= @contact.last_name %></strong>
  <!-- ... -->
</div>
```

Which is embedded by doing so:

```erb
<%# app/views/messages/show.html.erb %>
<div>
    <span>From: </span>
    <%= coupdoeil_popover_tag ContactPopover.with(contact: @contact).details do %>
      <span class="contact-pill"><%= @contact.email %></span>
    <% end %>
</div>
```

### Why use Coupdoeil popovers?

The concept of 'quick look' allowed by popovers can really improve UX by avoiding the need to navigate to another page and back again just to check summary for a resource.
It also permits to hiding until use some parts of UI like small forms, or quick actions in a list of elements.

The most common examples of such popover are on GitHub (when hovering over a link to a repo, user profile, a PR, etc.) or on
Wikipedia (when hovering over a link to another article).

While basic popups implementations can be made with simple helpers, data-attributes, and a bit of JavaScript,
it tends to get too complex over time and often not handle enough edge-cases to be used everywhere it could improve UX.

`Coupdoeil::Popover` offers a way to easily encapsulate and power up popovers rendering logic.
It allows a lot of customization possibilities (see [options](https://coupdoeil.org/opts.html)) and promotes re-usability by using an API similar to
parameterized ActionMailer (see [Why does it look like a mailer?](https://coupdoeil.org/architectural-decisions.html#why-does-it-look-like-a-mailer)).

This gem also tries to handle all known issues of such popovers, such as
the user's mouse quickly leaving and re-entering popover without closing it,
preventing opening of a popover if mouse did not stop on it, etc.

### Performances

By default, a popover content is not loaded until it is needed.
It means the DOM is not cluttered with 'maybe to be used' HTML, hidden in template tags or data-attributes.
However, params have to be present in HTML so they are sent for rendering the popover (eg: a record id),
so try passing as few as required to build it later during render.
Also, popovers are cached so the fetch happens only the first time a popover is open.

When needed, popovers can still be preloaded and included in DOM on initial page load. See [preload option](https://coupdoeil.org/options/loading.html).

## Licence 
Coupdoeil is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT)
