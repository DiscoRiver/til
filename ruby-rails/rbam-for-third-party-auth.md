When building internal business applications, it's likely that the auth system used will be a third party provider, like Okta.

Roles in these cases should ideally be managed by Okta groups, and can be used in Rails in the following way;

- A file `config/app_permissions.yml` can hold roles required for various services;

```yaml
admin_tools: ["tool-admin"]

user_tools: ["tool-admin", "tool-user"]

some_other_tools: ["tool-admin", "tool-service-user"]
```

Then you can have a helper function `does_the_current_user_have_permission_to_use_this?` that accepts your array, which would be accessed via `Rails.configuration.app_permissions[:some_other_tools]`, and looks for the relevant user groups in the database (which should have pulled from the identity provider). 

In your controller, you can use;

```ruby
def index
    @has_permission = helpers.does_the_current_user_have_permission_to_use_this?(Rails.configuration.app_permissions[:admin_tools])
  end
```

And to use this in your view;

```html
<% if @has_permission %>
    <!-- Do stuff -->
<% else %>
    <!-- Show restricted page -->  
<% end %>      
```

