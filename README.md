# Dotfiles Boshrelease
For higher productivity with `bosh ssh`.

# What does this release do
Inspired by `cloudfoundry-community/root-env-boshrelease`, `dotfiles-boshrelease` aims to make a better admin experience for `bosh ssh`, which is crucial for both dev & ops on bosh.

`Dotfiles-boshrelease` and `root-env-boshrelease` are similar in a sense that both enables the customization of `/root` directory. `root-env-boshrelease` achieves it by downloading a tarball from internet daily and replace the /root. The good point of such implementation is one can update `/root` without recreating release/re-deploying. However a concern would be downloading root-executing scripts to production servers might not be the best practice. Also, dotfiles usually should not be changing in a daily pace.

In contrast, `dotfiles-boshrelease` only templates scripts from /var/vcap/jobs/shrc/dotfiles/*. And `shrc/monit` ensure that when a new version of shrc is deployed, it will update it into /root. The shortcoming will be that it'll be necessary to create and upload release when you need to modify rc scripts.

# Something else
For some reason, by default the bash in stemcell doesnot recognize `.profile` or `.bash_profile`, and it will only read the already existed `.bashrc`. Therefore this release use `.bash_aliases` to trigger customized `shrc` script.

# How to use
Deploy the `shrc` job in a template, then find the amazing changes with `sudo su root`.

# Reference
monit.complete is from: https://github.com/linsong/my_bash/blob/master/completion/monit.completion.bash
