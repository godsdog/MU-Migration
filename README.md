# MU-Migration

This WP-CLI plugin makes the process of moving sites from single WordPress sites to a Multisite instance much easier.
It exports everything into a zip package which can be used to automatically import it within the desired Multisite installation.

### Install ###

#### Via WP-CLI Package Manager (requires wp-cli >= 0.23)
Just run `wp package install 10up/mu-migration`
#### Installing as a plugin
Clone this repo onto `plugins/` folder, run `composer install` to fetch dependencies and activate the plugin.

You need to install this on both the site you're moving and the target Multisite installation.

### Why do I need this? ###
Moving single WordPress sites to a Multisite environment can be challenging, specially if you're moving more than one site to
Multisite. You'd need to replace tables prefix, update post_author and wc_customer_user (if WooCommerce is installed) with the new
users ID (Multisite has a shared users table, so if you're moving more than one site you can't guarantee that users will have the same IDs) and more.

There are also a few housekeeping tasks that needs to be done to make sure that the new site will work smoothly and without loosing any data.

### How it works ###

With a simple command you can export a whole site into a zip package.

```
$ wp mu-migration export all site.zip --plugins --themes --uploads
```

The above command will export users, tables, plugins folder, themes folder and the uploads folder to a unique zip file that you can
move to the Multisite server in order to be imported with the `import all` command. The optional flags `--plugins --themes --uploads`,
add the plugins folder, themes folder and uploads folder to the zip file respectively.

The following command can be used to import a site from a zip package.
```
$ wp mu-migration import all site.zip
```
It will create a new site within your Multisite network based on the site you have just exported, the import all command will take care
of everything that needs to be done when moving a site to Multisite (replacing tables prefix, updating post_author IDs and etc).

If you need to set up a new url for the site you're importing (if importing into staging or local environments for example),
you can pass it to the `import all` command.

```
$ wp mu-migration import all site.zip --new_url=multisite.dev/site
```

After the migration you can also manage users password (reset passwords and/or force users to reset their passwords).
```
$ wp mu-migration update_passwords [<newpassword>] [--blog_id=<blog_id>] [--reset] [--send_email] [--include=<users_id>]  [--exclude=<users_id>]
```

E.g

The following command will update all users passwords of the site with ID 3 to `new_weak_password`.
```
$ wp mu-migration update_passwords 'new_weak_password' --blog_id=3
```

This next command will reset all users passwords to a random secure password and it will send a reset email to all users.

```
$ wp mu-migration update_passwords --reset --blog_id=3 --send_email
```

### Notes ###
If your theme and plugins have been done in the WordPress way, you should not have major problem after the migration, keep in mind
that some themes may experience incompatibilities issue if doing things in the wrong way. (E.g hardcoded links like '/contact' etc)
Depending of the codebase of the site you're migrating you may need to push some fixes to your code.

### License (MIT) ###
Copyright (c) 2016, Nícholas André, 10up Inc.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

### Credits ###


Created by Nícholas André ([@nicholas_io](https://profiles.wordpress.org/nicholas_io)), at [10up.com](http://10up.com).

Think this is a cool project? Love working with WordPress? [10up is hiring!](http://10up.com/careers/?utm_source=wphammer&utm_medium=community&utm_campaign=oss-code)
