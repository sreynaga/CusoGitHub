# How To Set The Development Environment

## Pre-Requirements

- Apache Server
- PHP (Recommended 5.6.30 version)
- MySQL (Recommended 5.6.35 version)
- PHP IDE
- Git

## Recommended Tools

- [MAMP](https://www.mamp.info/en/) (Mac OS, Apache Server, MySQL, PHP)
- PhpMyAdmin(included on MAMP) or Workbench
- [Sublime Text](https://www.sublimetext.com/) or [Atom](https://atom.io/) or [PHP Storm](https://www.jetbrains.com/phpstorm/) among other ones. It's your choice :)
- To use git you will need the Terminal or [GitHub Desktop](https://desktop.github.com/) or you can use the IDE.

## Steps to set up the App

- Clone the repository from [GitHub](https://github.com/OMIproduct/omiweb)
    
    > With the terminal:

    ```sh
    git clone git@github.com:OMIproduct/omiweb.git

    ```
- Download a full backup of production site from [wpengine](https://my.wpengine.com/installs/omiweb/backup_points#production)
- Merge the backup folder into the repository folder from GitHub
- Remove the folder `wp-content/mu-plugins/`
- Remove the file `wp-content/advanced-cache.php`
- Remove the folder `wp-content/fix-my-feed-rss-repair`
- Create a virtual host on MAMP and select the GitHub repository as root folder. Example [how to create a virtual host on MAMP](http://foundationphp.com/tutorials/vhosts_mamp.php)
- Import the database into `MAMP MySQL` server. You can find a copy of the database on the folder `wp-content` as `mysql.sql`
- Change the connection parameters to the database. You can change that on the `wp-config.php` file

    > Original:
    ```php
    # Database Configuration
    define( 'DB_NAME', 'wp_omiweb' );
    define( 'DB_USER', 'omiweb' );
    define( 'DB_PASSWORD', 'fgDFaAe94oKIBFC5' );
    define( 'DB_HOST', '127.0.0.1' );
    ```

    > Should be:
    ```php
    # Database Configuration
    define( 'DB_NAME', 'YOUR_DATABASE_NAME' );
    define( 'DB_USER', 'YOUR_DATABASE_USERNAME' );
    define( 'DB_PASSWORD', 'YOUR_DATABASE_PASSWORD' );
    define( 'DB_HOST', 'localhost' );
    ```
- Change the option `DOMAIN_CURRENT_SITE` on the `wp-config.php` file without `http` or `/` at the end of the url

    > Original:
    ```php
    define( 'DOMAIN_CURRENT_SITE', 'www.onlinemarketinginstitute.org' );
    ```

    > Should be:
    ```php
    define( 'DOMAIN_CURRENT_SITE', 'YOUR_DOMAIN_HERE_WITHOUT_HTTP_OR_SLASH_AT_THE_END' );
    ```

    > Example:
    ```php
    define( 'DOMAIN_CURRENT_SITE', 'omiweb.dev' );
    ```
- Change the option `WPE_FORCE_SSL_LOGIN` and `FORCE_SSL_LOGIN` on the `wp-config.php` file

    > Original
    ```php
    define( 'WPE_FORCE_SSL_LOGIN', true );

    define( 'FORCE_SSL_LOGIN', true );
    ```

    > Should be:
    ```php
    define( 'WPE_FORCE_SSL_LOGIN', false );

    define( 'FORCE_SSL_LOGIN', false );
    ```

## Steps to set up the database

We need modify some tables on the database. Run the following sql sentences on your database.

1. Update the `domain` for MAMP's hostname **WITHOUT** `http` **OR** `/` on `wp_blogs` table:

    ```sql
    UPDATE wp_blogs SET domain = "YOUR_DOMAIN_HERE_WITHOUT_HTTP_OR_SLASH_AT_THE_END";
    ```

    > Example
    ```sql
    UPDATE wp_blogs SET domain = "omiweb.dev";
    ```

2. Update the `domain` for MAMP's hostname **WITHOUT** `http` **OR** `/` at the end on `wp_site` table:

    ```sql
    UPDATE wp_site SET domain = "YOUR_DOMAIN_HERE_WITHOUT_HTTP_OR_SLASH_AT_THE_END";
    ```

    > Example:
    ```sql
    UPDATE wp_site SET domain = "omiweb.dev";
    ```

3. Update the `meta_value` for MAMP's hostname **WITH** `http` **AND** `/` at the end on `wp_sitemeta` table:

    ```sql
    UPDATE wp_sitemeta SET meta_value = "YOUR_DOMAIN_HERE_WITH_HTTP_AND_SLASH_AT_THE_END" WHERE meta_id = 14;
    ```

    > Example:
    ```sql
    UPDATE wp_sitemeta SET meta_value = "http://omiweb.dev/" WHERE meta_id = 14;
    ```

4. Update the `option_value` for MAMP's hostname **WITH** http and **WITHOUT** `/` at the end on `wp_options` table:

    ```sql
    UPDATE wp_options SET option_value = "YOUR_DOMAIN_HERE_WITH_HTTP_AND_WITHOUT_SLASH_AT_THE_END" WHERE option_id = 1;
    UPDATE wp_options SET option_value = "YOUR_DOMAIN_HERE_WITH_HTTP_AND_WITHOUT_SLASH_AT_THE_END" WHERE option_id = 2;
    ```

    > Example:
    ```sql
    UPDATE wp_options SET option_value = "http://omiweb.dev" WHERE option_id = 1;
    UPDATE wp_options SET option_value = "http://omiweb.dev" WHERE option_id = 2;
    ```
