
# Dotfiles Boshrelease
For higher productivity with `bosh ssh`.

# What does this release do
Inspired by `cloudfoundry-community/root-env-boshrelease`, `dotfiles-boshrelease` aims to make a better admin experience for `bosh ssh`, which is crucial for both dev & ops on BOSH based infrastructures.

`Dotfiles-boshrelease` and `root-env-boshrelease` are similar in a sense that both enables the customization of `/root` directory. However they do it in different ways.

`root-env-boshrelease` customizes /root by downloading a tarball from a url daily and then replace the /root. The good thing of such implementation is one can update `/root` without recreating release/re-deploying. However a concern would be downloading root-executing scripts to production servers might not be the best practice. Also, dotfiles usually should not be changing in a daily pace.

In contrast, `dotfiles-boshrelease` only use local rc files from `/var/vcap/jobs/shrc`. They are deployed by bosh. If you remove `shrc` from manifest, your admin environment will be (almost) clean again.

# How to use
In manifest file, add `dotfiles` to releases. In template, add `shrc` job.
Deploy the `shrc` job in a template, then find the amazing changes with `bosh ssh`.

# What if I want to add some more bash sugar?
A property `shrc.extra_commands` are provided for you to inject your own commands easily. The value of `extra_commands` will be executed after `.shrc`.  
On the other hand, you can also just fork and modify this project, then use bosh to create, upload and deploy your own release.

# BIG NOTES
1. It is known that if you modify `shrc.extra_commands` (although this should not happen frequently), all jobs those have `shrc` installed will be re-deployed since the templated files are changed. Considering that it is common to have `shrc` installed on ALL jobs, this will trigger re-deploy on ALL jobs, and sometimes it is known to be a pain in the ass.
2. `shrc` is bound to `root`, `vcap` and `bosh_$TEMP` account. Means that when you su to `root`, `extra_commands` will be executed automatically with root priviledge. The extra commands are executed with `set -eu` for your flight safety. Don't shoot your feet.

# What is done by default
1. inputrc:
  - Enable word jump (alt + Left/Right);
  - Enable history search (alt + Up/Down; alt + p/n; PageUp/PageDown)
  - Completion-ignore-case on
1. shrc:
  - Frequently used dirs: link to $HOME (vcap, packages, jobs, sys/log, etc..)
  - PS1: Meaningful and colorful PS1. You can set your favorite PS1 color in spec.
  - packages bins: add packages/*/bin to PATH.
  - cd $HOME: Home sweet home.
1. extrarc:
  - You can inject commands from manifest by overriding `shrc.extra_commands` property. However be careful not to crash your machine ^_^
1. shrc/userbin:
  - ack
  - (home made) link_apps: link warden container directories to $HOME/app.
  - (home made) wwsh: a shortcut to wsh into warden containers.
  - (home made) haste: a hastebin client, is convenient when you have a internal hastebin server.
1. completion:
  - monit completion
  - wwsh completion
1. properties dump:
  - Use `show-bosh-properties` command to see properties that's used by bosh-template. Useful for debugging.

# Something else
For some reason, by default the bash in stemcell doesnot recognize `.profile` or `.bash_profile`, and it will only read the already existed `.bashrc`. Therefore this release use `.bash_aliases` to trigger customized `shrc` script.

# Reference
- Inspired by [root-env](https://github.com/cloudfoundry-community/root-env-boshrelease)
- [ack](http://beyondgrep.com/)
- monit.complete is taken from [linsong/my_bash](https://github.com/linsong/my_bash/blob/master/completion/monit.completion.bash)
- haste bash client are modified from [haste-client](https://github.com/seejohnrun/haste-client/#lightweight-alternative)

# Author
[Weichu Liu](https://twitter.com/weichuliu) (Rakuten, Inc.)
