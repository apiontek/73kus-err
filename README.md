# 73k.us custom rainbow error pages (40x/50x)

Based off [frontend-webpack-boilerplate](https://github.com/WeAreAthlon/frontend-webpack-boilerplate) with TailwindCSS added.

Builds html & css that can be used as custom error page templates with caddy.

## NOTE

After production build, optionally remove the empty, 0-byte app.js script tags from the built files, just to optimize things further.

### caddy example

With this snippet in the Caddyfile, for a site one can `import err` to handle errors for that site.

```
(err) {
	@errpaths {
		path /err_css/*
		path /err_images/*
	}
	route @errpaths {
		root * /www/err
		file_server
	}
	respond /unknown_error_not_4xx_not_5xx "{http.error.status_code} {http.error.status_text}"

	handle_errors {
		@4xx expression `{http.error.status_code} >= 400 && {http.error.status_code} < 500`
		@5xx expression `{http.error.status_code} >= 500 && {http.error.status_code} < 600`

		root * /www/err
		encode zstd gzip
		file_server

		rewrite @4xx /4xx.html
		rewrite @5xx /5xx.html
		rewrite * /unknown_error_not_4xx_not_5xx
		templates
	}
}
```
