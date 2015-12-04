
# Dotfiles Boshrelease
For higher productivity with `bosh ssh`.

# What does this release do
Inspired by `cloudfoundry-community/root-env-boshrelease`, `dotfiles-boshrelease` aims to make a better admin experience for `bosh ssh`, which is crucial for both dev & ops on bosh.

`Dotfiles-boshrelease` and `root-env-boshrelease` are similar in a sense that both enables the customization of `/root` directory. However they do it in different ways.

`root-env-boshrelease` customizes /root by downloading a tarball from a url daily and then replace the /root. The good thing of such implementation is one can update `/root` without recreating release/re-deploying. However a concern would be downloading root-executing scripts to production servers might not be the best practice. Also, dotfiles usually should not be changing in a daily pace.

In contrast, `dotfiles-boshrelease` only templates scripts from /var/vcap/jobs/shrc/dotfiles/*. And `shrc/monit` ensure that when a new version of shrc is deployed, it will update it into /root.

# How to use
In manifest file, add `dotfiles` to releases. In template, add `shrc` job.
Deploy the `shrc` job in a template, then find the amazing changes with `sudo su root`.

# What if I want to add some more bash sugar?
In order to modify and deploy new rc files, you have to create a new release, upload it, then `bosh deploy`

# What is done by default
1. Inputrc:
  - Enable word jump (alt + Left/Right);
  - Enable history search (alt + Up/Down; alt + p/n; PageUp/PageDown)
  - Completion-ignore-case on
2. shrc:
  - Frequently used dirs: link to /root(vcap, packages, jobs, sys/log, etc..)
  - PS1: Meaningful PS1
  - packages bins: add packages/*/bin to PATH.
  - cd $HOME: Home sweet home.
3. /root/bin:
  - ack
  - (home made) link_apps: link warden container directories to /root/app.
  - (home made) wwsh: a shortcut to wsh into warden containers.
4. completion:
  - monit completion
  - wwsh completion
5. properties dump:
  - Properties are dumped to `~/.properties.json` for debugging.

# Something else
For some reason, by default the bash in stemcell doesnot recognize `.profile` or `.bash_profile`, and it will only read the already existed `.bashrc`. Therefore this release use `.bash_aliases` to trigger customized `shrc` script.

# Reference
- Inspired by root-env: https://github.com/cloudfoundry-community/root-env-boshrelease
- ack: http://beyondgrep.com/
- monit.complete is taken from: https://github.com/linsong/my_bash/blob/master/completion/monit.completion.bash

# Author
[Weichu Liu](https://twitter.com/weichuliu) (Rakuten, Inc.)
