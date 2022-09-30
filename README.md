### TLDR
Bumping from v7.1.0 to v7.2.0 seems to be causing `data-turbo-action: 'advance'` to not update the URL when a link is inside a different turbo-frame.

### Explanation

In v7.1.0, it was possible to have:

```ruby
<%= turbo_frame_tag "modal" %>

<%= turbo_frame_tag "other_frame" do %>
  <%= link_to "New post", new_post_path, data: { turbo_frame: "modal", turbo_action: 'advance' } %>
<% end %>
```

which would have the `New post` link render its response in the `modal` frame, and `data-turbo-action: 'advance'` was properly updating the URL to the `/posts/new` path:

![image](https://user-images.githubusercontent.com/2895888/193297711-8525e20b-976a-4324-bcfa-de286cd8e1a7.png)

With the bump to v7.2.0, it's still able to render the response in the `modal` frame, but the `advance` is no longer updating the URL.

### Reproduction
- Clone https://github.com/diachini/turbo_rails_advance_in_frame
- `bin/rails db:migrate`
- `bin/rails s`
- Visit http://localhost:3000/posts/
- See that:
  - `New post (from other_frame - broken)` does **not** update the URL to `/posts/new`
  - `New post (not in a frame - advances)` does update the URL as expected
  - `New post (from the same frame - advances)` does update the URL as expected
- Checkout the `v7.1.0_advance_works_fine` branch (which pins `@hotwired/turbo` and `@hotwired/turbo-rails` to v7.1.0)
- See that the link `from other_frame` **does** update the URL to `/posts/new'

### Version information
Rails 7.0.4
Turbo-rails gem 1.3.0
@hotwired/turbo-rails 7.2.0
@hotwired/turbo 7.2.0
