---
title:   Composer and Packagist
isChild: true
anchor:  composer_and_packagist
---

## Composer and Packagist {#composer_and_packagist_title}

Composer is a **brilliant** dependency manager for PHP. List your project's dependencies in a `composer.json` file and,
with a few simple commands, Composer will automatically download your project's dependencies and setup autoloading for
you. Composer is analogous to NPM in the node.js world, or Bundler in the Ruby world.

There are already a lot of PHP libraries that are compatible with Composer, ready to be used in your project. These
"packages" are listed on [Packagist], the official repository for Composer-compatible PHP libraries.

### How to Install Composer

The safest way to download composer is by [following the official instructions](https://getcomposer.org/download/).   
This will verify the installer is not corrupt or tampered with.  
The installer installs Composer *locally*, in your current working directory.

We recommend installing it *globally* (e.g. a single copy in /usr/local/bin) - to do so, run this afterwards:

{% highlight console %}
mv composer.phar /usr/local/bin/composer
{% endhighlight %}

**Note:** If the above fails due to permissions, prefix with `sudo`.

To run a locally installed Composer you'd use `php composer.phar`, globally it's simply `composer`.

#### Installing on Windows

For Windows users the easiest way to get up and running is to use the [ComposerSetup] installer, which
performs a global install and sets up your `$PATH` so that you can just call `composer` from any
directory in your command line.

### How to Install Composer (manually)

Manually installing Composer is an advanced technique; however, there are various reasons why a 
developer might prefer this method vs. using the interactive installation routine. The interactive
installation checks your PHP installation to ensure that:

- a sufficient version of PHP is being used
- `.phar` files can be executed correctly
- certain directory permissions are sufficient
- certain problematic extensions are not loaded
- certain `php.ini` settings are set

Since a manual installation performs none of these checks, you have to decide whether the trade-off is 
worth it for you. As such, below is how to obtain Composer manually:

{% highlight console %}
curl -s https://getcomposer.org/composer.phar -o $HOME/local/bin/composer
chmod +x $HOME/local/bin/composer
{% endhighlight %}

The path `$HOME/local/bin` (or a directory of your choice) should be in your `$PATH` environment 
variable. This will result in a `composer` command being available.

When you come across documentation that states to run Composer as `php composer.phar install`, you can
substitute that with:

{% highlight console %}
composer install
{% endhighlight %}

This section will assume you have installed composer globally.

### How to Define and Install Dependencies

Composer keeps track of your project's dependencies in a file called `composer.json`. You can manage it
by hand if you like, or use Composer itself. The `composer require` command adds a project dependency 
and if you don't have a `composer.json` file, one will be created. Here's an example that adds [Twig]
as a dependency of your project.

{% highlight console %}
composer require twig/twig:~1.8
{% endhighlight %}

Alternatively, the `composer init` command will guide you through creating a full `composer.json` file
for your project. Either way, once you've created your `composer.json` file you can tell Composer to
download and install your dependencies into the `vendor/` directory. This also applies to projects 
you've downloaded that already provide a `composer.json` file:

{% highlight console %}
composer install
{% endhighlight %}

Next, add this line to your application's primary PHP file; this will tell PHP to use Composer's 
autoloader for your project dependencies.

{% highlight php %}
<?php
require 'vendor/autoload.php';
{% endhighlight %}

Now you can use your project dependencies, and they'll be autoloaded on demand.

### Updating your dependencies

Composer creates a file called `composer.lock` which stores the exact version of each package it
downloaded when you first ran `composer install`. If you share your project with others, 
ensure the `composer.lock` file is included, so that when they run `composer install` they'll 
get the same versions as you.  To update your dependencies, run `composer update`. Don't use 
`composer update` when deploying, only `composer install`, otherwise you may end up with different 
package versions on production.

This is most useful when you define your version requirements flexibly. For instance, a version
requirement of `~1.8` means "anything newer than `1.8.0`, but less than `2.0.x-dev`". You can also use 
the `*` wildcard as in `1.8.*`. Now Composer's `composer update` command will upgrade all your
dependencies to the newest version that fits the restrictions you define.

### Update Notifications

To receive notifications about new version releases you can sign up for [VersionEye], a web service
that can monitor your GitHub and BitBucket accounts for `composer.json` files and send emails with new
package releases.

### Checking your dependencies for security issues

The [Security Advisories Checker] is a web service and a command-line tool, both will examine your `composer.lock`
file and tell you if you need to update any of your dependencies.

### Handling global dependencies with Composer

Composer can also handle global dependencies and their binaries. Usage is straight-forward, all you need
to do is prefix your command with `global`. If for example you wanted to install PHPUnit and have it 
available globally, you'd run the following command:

{% highlight console %}
composer global require phpunit/phpunit
{% endhighlight %}

This will create a `~/.composer` folder where your global dependencies reside. To have the installed
packages' binaries available everywhere, you'd then add the `~/.composer/vendor/bin` folder to your 
`$PATH` variable.

* [Learn about Composer]

[Packagist]: http://packagist.org/
[Twig]: http://twig.sensiolabs.org
[VersionEye]: https://www.versioneye.com/
[Security Advisories Checker]: https://security.sensiolabs.org/
[Learn about Composer]: http://getcomposer.org/doc/00-intro.md
[ComposerSetup]: https://getcomposer.org/Composer-Setup.exe
