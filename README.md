# Dotfiles Boshrelease
For higher productivity with `bosh ssh`.

# What does Dotfiles-boshrelease Do
The `dotfiles-boshrelease` lets you set up a cozy terminal environment within BOSH VMs. It sets up not only `root` user, but also `vcap` user, temporary `bosh_<random>` users, and you own users (`shrc.extra_users`).

With `dotfiles`, life is much easier when doing dev & ops on BOSH based infrastructures.

This boshrelease is inspired by [`root-env`](https://github.com/cloudfoundry-community/root-env-boshrelease). However, unlike `root-env` which downloads a tarball daily to the `/root` dir, `dotfiles-boshrelease` uses only local files.

It's more secure (no root executable script from internet); All changes to VMs are visible in properties; Changes take place explicitly (only when you do the `bosh deploy`); And it's easy to uninstall - simply remove `shrc` job from manifest, and your admin environment is (almost) clean again.

# How to use
In manifest file, add `dotfiles` to releases. In template, add `shrc` job.

Deploy the `shrc` job in a template, then find the amazing changes with `bosh ssh`.

You can also check `jobs/shrc/spec` to tune properties as you like.

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
2. shrc:
  - Frequently used dirs: link to $HOME (vcap, packages, jobs, sys/log, etc..)
  - PS1: Meaningful and colorful PS1. You can set your favorite PS1 color in spec.
  - packages bins: add packages/*/bin to PATH.
3. extra_users:
  - Set `shrc.extra_users` property to let dotfiles create admin account and inject public key for you. Note that there is a sanity check when creating account. If user's `name` and `public_key` failed to pass the check, they will be ignored. For more details, check `bin/add-users.erb`.
  - CAVEAT: dotfiles will remove /home/*/.ssh/authorized_keys before it injects public keys, in order to keep system safe. If you unfortunately set /home/vcap/.ssh/authorized_keys, it will be removed.
4. extrarc:
  - You can inject commands from manifest by overriding `shrc.extra_commands` property. However be careful not to crash your machine ^_^
5. shrc/userbin:
  - ack
  - (home made) link_apps: link warden container directories to $HOME/app.
  - (home made) wwsh: a shortcut to wsh into warden containers.
  - (home made) haste: a hastebin client, is convenient when you have a internal hastebin server.
6. completion:
  - monit completion
  - wwsh completion
7. properties dump:
  - Use `show-bosh-properties` command to see properties that's used by bosh-template. Useful for debugging.
8. `set-ps1`
  - A small function helps you to set informative prompt. By default it puts information such as `spec.deployment`, `job_name/index`, `ip` on PS1.
  - You can also easily customize your prompt with `set-ps1`. Check `set-ps1 -h` for details.

# Something else
For some reason, by default the bash in stemcell doesnot recognize `.profile` or `.bash_profile`, and it will only read the already existed `.bashrc`. Therefore this release use `.bash_aliases` to trigger customized `shrc` script.

# Reference
- Inspired by [root-env](https://github.com/cloudfoundry-community/root-env-boshrelease)
- [ack](http://beyondgrep.com/)
- monit.complete is taken from [linsong/my_bash](https://github.com/linsong/my_bash/blob/master/completion/monit.completion.bash)
- haste bash client are modified from [haste-client](https://github.com/seejohnrun/haste-client/#lightweight-alternative)

# Author
[Weichu Liu](https://twitter.com/weichuliu) (Rakuten, Inc.)
