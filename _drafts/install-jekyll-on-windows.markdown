# Install Jekyll on Windows

Table of Contents

- [Prerequisites](install-jekyll.markdown#prerequisites)
- [Install Ruby and Ruby Development Kit](#install-ruby-and-ruby-development-kit)
- [Install dependencies through Bundler](#install-dependencies-through-bundler)
- [Install Bundler](#install-bundler)
- [Resources](#resources)

## [Prerequisites](install-jekyll.markdown#prerequisites)

## Install Ruby and Ruby Development Kit

First you must install Ruby because Jekyll is a Ruby-based program and needs Ruby to run.

1. Go to [RubyInstaller for Windows](http://rubyinstaller.org/downloads/).
1. Download a Ruby installer.
1. Double-click the downloaded file and proceed through the wizard to install it. Run the `ridk install` step on the last stage of the installation wizard.

## Install dependencies through Bundler

Some Jekyll themes will require certain Ruby gem dependencies. These dependencies are stored in something called a Gemfile, which is packaged with the Jekyll theme. You can install these dependencies through Bundler.

Bundler is a package manager for RubyGems. You can use it to get all gems (or Ruby plugins) that you need for your Jekyll project.

## Install Bundler

1. Install Bundler: `gem install bundler`
1. Delete or rename the existing `Gemfile` and `Gemfile.lock` files.
1. Initialize Bundler: `bundle init`
1. `bundle install`

## Resources

- <https://idratherbewriting.com/documentation-theme-jekyll/mydoc_install_jekyll_on_windows.html#install-ruby-and-ruby-development-kit>

  Install Jekyll on Windows | Jekyll theme for documentation
