Laptop
======

Laptop is a script to set up a macOS laptop for web development.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages
based on what is already installed on the machine.

Requirements
------------

We support:

* macOS Ventura (13.x) on Apple Silicon and Intel
* macOS Monterey (12.x) on Apple Silicon and Intel

Older versions may work but aren't regularly tested.
Bug reports for older versions are welcome.

Install
-------

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/eduardofavarato/laptop/main/mac
```

Review the script (avoid running scripts you haven't read!):

```sh
less mac
```

Execute the downloaded script:

```sh
sh mac 2>&1 | tee ~/laptop.log
```

Optionally, review the log:

```sh
less ~/laptop.log
```

What it sets up
---------------

### macOS tools:

* [Apple's Command Line Developer Tools](https://developer.apple.com/) to enable developer functionality on our macOS system.

### Command line tools:

* [Homebrew](http://brew.sh/) for managing operating system libraries.
* [Git](https://git-scm.com/) for version control
* [Github CLI](https://cli.github.com/) for using GitHub in your terminal
* [Zsh](http://www.zsh.org/) as your command line shell
* [Oh My Zsh](https://github.com/robbyrussell/oh-my-zsh) to add nice features to zsh - autocompletions, shortcuts etc.
* [tree](https://linux.die.net/man/1/tree) for visualising directory structure from the Terminal
* [OpenSSL](https://www.openssl.org/) for Transport Layer Security (TLS)
* [Mas-CLI](https://github.com/mas-cli/mas) as your CLI for the Mac App Store

### Programming languages and configuration:

* [Node.js](http://nodejs.org/) for JavaScript back-end development, and
* [NPM](https://www.npmjs.org/) for installing JavaScript packages
* [Yarn](https://yarnpkg.com/en/) for managing JavaScript packages
* [nvm](https://github.com/nvm-sh/nvm) for managing node versions

### Devops:

* [Docker](https://www.docker.com/) for building, sharing, and running modern applications
* [awscli](https://aws.amazon.com/pt/cli/) for managing AWS services

### Databases:

* [PostgreSQL](http://www.postgresql.org/) for storing relational data

### GUI Apps:

* [Google Chrome](https://www.google.com/chrome/) for web browsing and development
* [VS Code](https://code.visualstudio.com/) for text editing
* [JetBrains Toolbox](https://www.jetbrains.com/toolbox-app/) for managing JetBrains tools the easy way
* [Slack](https://slack.com) for team chat
* [iTerm2](https://iterm2.com/) for terminal
* [Insomnia](https://insomnia.rest/) for collaborative API development 

It should take less than 15 minutes to install (depends on your machine).

Customize in `~/.laptop.local`
------------------------------

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.
For example:

```sh
#!/bin/sh

brew bundle --file=- <<EOF
brew "Caskroom/cask/dockertoolbox"
brew "go"
brew "ngrok"
brew "watch"
EOF

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup

if [ -r "$HOME/.rcrc" ]; then
  fancy_echo "Updating dotfiles ..."
  rcup
fi
```

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.

See this [wiki](https://github.com/thoughtbot/laptop/wiki)
for more customization examples.

License
-------

Laptop is © 2011-2022 thoughtbot, inc.
It is free software,
and may be redistributed under the terms specified in the [LICENSE] file.

[LICENSE]: LICENSE

About thoughtbot
----------------

![thoughtbot](https://thoughtbot.com/brand_assets/93:44.svg)

Laptop is maintained and funded by thoughtbot, inc.
The names and logos for thoughtbot are trademarks of thoughtbot, inc.

We are passionate about open source software.
See [our other projects][community].
We are [available for hire][hire].

[community]: https://thoughtbot.com/community?utm_source=github
[hire]: https://thoughtbot.com?utm_source=github
