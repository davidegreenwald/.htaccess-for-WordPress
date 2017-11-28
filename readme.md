# Apache .htaccess Server Configuration for WordPress

 If possible, this code should go in Apache's configuration files directly (such as `httpd.conf`).

However, on shared hosting or other limited-access servers, the `.htaccess` file may be used instead to set these directives at the user level. It's slower, but will accomplish the same results.

WordPress will also automatically generate this file (with just `mod_rewrite`) if it is not already present.

Certain plugins will write to `.htaccess` for cache, gzip, or redirection, be sure to avoid duplicate or conflicting code.

`<IfModule></IfModule>` works by checking for an Apache module, turning it on, and listing your directives.

See also:
* [HTML5 Boilerplate](https://github.com/h5bp/html5-boilerplate/blob/master/dist/.htaccess)
* [Apache documentation](https://httpd.apache.org/docs/current/howto/htaccess.html)

Included within:

## WordPress `mod_rewrite`

* Sets the standard WordPress permalink rewrite for `.php` pages. This will cause `example.com/index.php` to display as `example.com`. Don't touch this - WordPress will overwrite any additions.

## WordPress Security

* Disable `xmlrpc.php` to limit hacker login attempts. `xmlprc.php` is used for Jetpack, to blog by email, and pingbacks. If you're not doing any of this, you don't need it.

* Protect `wp-config.php` from HTTP access.

## SSL

Enable HSTS HTTPS preload. Don't do this unless you have an SSL certificate and HTTPS working!

## `mod_expires` for Cache

Serve resources with far-future expires headers. Tells users to keep static, unchanging resources like images and JavaScript for a set amount of time so they don't have to download them again on their next visit.

Test if your config is doing this already: [gtmetrix.com](https://gtmetrix.com/)

Look for "Leverage browser caching." If you find "expiration not specified," you can specify it with this code.

This is a different cache technique than page cache, which leaves an `.html` file on your server to avoid future database/PHP usage, and
it can certainly be used in tandem with a cache plugin such as KeyCDN's Cache Enabler.

Be sure not to duplicate this code with a more complicated plugin if you add it manually.

"Bust" the cache period by renaming the file.

## `mod_deflate` for `gzip`

`mod_deflate` compresses text-based assets (but not pre-compressed files such as .jpgs).

Test if your config is doing this already, it probably is:
* [gtmetrix.com](https://gtmetrix.com/)
* [checkgzipcompression.com](https://checkgzipcompression.com/)

[Knackforge.com: Setting up mod_deflate from scratch](https://knackforge.com/blog/karalmax/how-enable-gzip-compression-apache)

## The Vary header

The `vary` header tells compatible browsers to use `gzip` and sends unzipped content if not.

Apache’s `mod_deflate` sends out the `Vary: Accept-Encoding` header automatically and there should be no need to add the module separately.
[Apache: mod_deflate](https://httpd.apache.org/docs/2.4/mod/mod_deflate.html)

Confirm your `vary` is working:
Chrome DevTools: Network > Response Headers > Vary
[Gtmetrix.com](https://gtmetrix.com/)

## Still to come:

`mod_deflate`, which your server hopefully has configured already.
