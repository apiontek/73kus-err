# 73k.us custom rainbow error pages (40x/50x)

Based off [frontend-webpack-boilerplate](https://github.com/WeAreAthlon/frontend-webpack-boilerplate) with TailwindCSS added.

Builds html & css that can be used as custom error page templates with caddy.

## NOTE

After production build, optionally remove the empty, 0-byte app.js script tags from the built files, just to optimize things further.

### caddy example

This could probably be optimized but it's working:

```
# Custom Unicorn Rainbow error pages
location /err_css {
  root /var/www/err;
  autoindex off;
  expires max;
}
location /err_images {
  root /var/www/err;
  autoindex off;
  expires max;
}
error_page 403 404 /40x.html;
location = /40x.html {
  root /var/www/err;
}
error_page 500 502 503 504 /50x.html;
location = /50x.html {
  root /var/www/err;
}
```
