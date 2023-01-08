Sometimes javascript code wouldn't load in my Rails app, and while this was mostly during my initial learning of web dev concepts, this might be useful in the future.

Rail 7 needs the file `/app/assets/javascripts/application.js` to have required packages included;

```
//= require jquery3
//= require bootstrap
```

Alternatively, but less correct when using gems for package management, is adding the script to `app/views/layouts/application.html.erb`

```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js"></script>
```