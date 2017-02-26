# git-press
An example of maintaining a WordPress project in a Git repository.

# Develop locally
- Clone the repo: `git clone https://github.com/mrjpsilver/git-press.git`
- Configure a local development server to serve the `git-press` folder.
  - I like good ol' [MAMP](https://www.mamp.info/en/) for my good ol' Mac.
  - If I were using Windows, I'd go with [XAMPP](https://www.apachefriends.org/), which is essentially the same thing.
- Configure an empty database to hold your WordPress data.
- Copy the contents of `wp-config-sample.php` into a new file, `wp-config.php`.
- In `wp-config.php`, edit the `DB_NAME`, `DB_USER`, and `DB_PASSWORD` such that WordPress can connect to your database.
- Initialize WordPress by visiting your local server in a web browser.

# Sync w/ production server
Suppose you're syncing with a production server. On production, new posts are made and images are uploaded all the time. Meanwhile, dev work is progressing within your local environment. Want to keep the two in sync?
- Adding the following rewrite rule to your `.htaccess` will force your server to check for uploads (images, videos, etc) on your production server when they aren't found locally.
```PHP
# BEGIN ProductionUploadRoutes
SetEnv PROD http://yourserver.com
<IfModule mod_rewrite.c>
  RewriteEngine on

  # Attempt to load files from production if
  # they're not in our local version
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule wp-content/uploads/(.*) \
    http://{PROD}/wp-content/uploads/$1 [NC,L]
</IfModule>

# END ProductionUploadRoutes
```
- Migrate the production database to your local development environment to get posts, comments, etc.
- Work within the local environment, enjoying the benefits of up-to-date posts and images
- When your dev work is complete and tested, and you're ready to update code in production, push the files up via FTP/SFTP/SSH
