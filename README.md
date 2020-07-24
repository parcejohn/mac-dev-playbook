# Mac Development Ansible Playbook

This playbook installs and configures most of the software I use on my Mac for web and software development. Some things in macOS are slightly difficult to automate, so I still have some manual installation steps, but at least it's all documented here.

This is a work in progress, and is mostly a means for me to document my current Mac's setup. I'll be evolving this set of playbooks over time.

*Inspired by*:
[mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook)
[![Build Status](https://travis-ci.org/geerlingguy/mac-dev-playbook.svg?branch=master)](https://travis-ci.org/geerlingguy/mac-dev-playbook)


## Installation

Assuming you have an admin account, and a regular account john

Create a group ‘brew’ and add admin and regular accounts
brew doctor (admin checks if he installed brew correctly)
sudo chgrp -R brew $(brew --prefix)/* (Change the group of homebrew installation directory)
sudo chmod -R g+w $(brew --prefix)/*(Allow group members to write inside this directory)
brew doctor (john checks if he can use homebrew)

### Grant sudo to regular account
Need sudo to install some packages (admin on mac is not needed)

### Install Brew as admin account and apply proper permissions

$ su - macadmin
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
$ brew doctor
$ sudo chgrp -R brew $(brew --prefix)/*
$ sudo chmod -R g+w $(brew --prefix)/*
$ sudo chown john -R g+w $(brew --prefix)/*


### Install Ansible
$ brew install ansible

### Run this ansible playbook
  1. Clone this repository to your local drive.
  2. Run `$ ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible roles.
  3. Run `ansible-playbook main.yml -i inventory -K` inside this directory. Enter your account password when prompted.

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook`'s `--tags` flag. The tags available are `dotfiles`, `homebrew`, `mas`, `extra-packages` and `osx`.

    ansible-playbook main.yml -i inventory -K --tags "dotfiles,homebrew"

## Overriding Defaults

Not everyone's development environment and preferred software configuration is the same.

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and apps with something like:

    homebrew_installed_packages:
      - cowsay
      - git
      - go
    
    mas_installed_apps:
      - { id: 443987910, name: "1Password" }
      - { id: 498486288, name: "Quick Resizer" }
      - { id: 557168941, name: "Tweetbot" }
      - { id: 497799835, name: "Xcode" }
    
    composer_packages:
      - name: hirak/prestissimo
      - name: drush/drush
        version: '^8.1'
    
    gem_packages:
      - name: bundler
        state: latest
    
    npm_packages:
      - name: webpack
    
    pip_packages:
      - name: mkdocs

Any variable can be overridden in `config.yml`; see the supporting roles' documentation for a complete list of available variables.

## Included Applications / Configuration (Default)

Applications (installed with Homebrew Cask)

Packages (installed with Homebrew)

My dotfiles are also installed into the current user's home directory, including the `.osx` dotfile for configuring many aspects of macOS for better performance and ease of use. You can disable dotfiles management by setting `configure_dotfiles: no` in your configuration.

Finally, there are a few other preferences and settings added on for various apps and services.

### Things that still need to be done manually

1. Set iTerm Preferences
 Preferences - Profiles - Colors - Color Presets : Tango Dark (not needed when using powerlevel10k)
 Preferences - Profiles - Text - Font : Menlo (not needed when using powerlevel10k)
 Preferences - Profiles - Window - Tranparency

2. Install AWS CLI v2
https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-mac.html

3. Configure aws-mfa

4. Install [Sublime Package Manager](http://sublime.wbond.net/installation).

5. Set JJG-Term as the default Terminal theme (it's installed, but not set as default automatically).

6. Install App Store apps
* iMovie
* Microsoft Remote Desktop
* Microsoft To Do
* QNAP QVR Client




