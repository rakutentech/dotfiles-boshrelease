# Contributing to Dotfiles Boshrelease

We utilize Github Issues for issue tracking and contributions. You can contribute in two ways:

Reporting an issue or making a feature request [here](#issues).
Adding features or fixing bugs yourself and contributing your code.


## Contributor License Agreement

Follow these steps to make a contribution to any of our open source repositories:

1. Ensure that you have completed our CLA Agreement for Rakuten,Inc.

1. Set your name and email (these should match the information on your submitted CLA)

        git config --global user.name "Firstname Lastname"
        git config --global user.email "your_email@example.com"

## General Workflow

Follow these steps to make a contribution to any of our open source repositories:

1. Fork the repository
1. Update submodules (`git submodule update --init`)
1. Create a feature branch from develop(`git checkout -b better`)

1. Make changes on the branch:
    * Adding a feature
      1. Add tests for the new feature
      1. Make the tests pass
    * Fixing a bug
      1. Add a test/tests which exercises the bug
      1. Fix the bug, making the tests pass
    * Refactoring existing functionality
      1. Change the implementation
      1. Ensure that tests still pass
        * If you find yourself changing tests after a refactor, consider
          refactoring the tests first

1. Push to your fork (`git push origin better_bosh`) and submit a pull request selecting `develop` as the target branch

We favor pull requests with very small, single commits with a single purpose.

Your pull request is much more likely to be accepted if:

* Your pull request is small and focused with a clear message that conveys the intent of your change.

