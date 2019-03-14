#### Branches

Skycoin repos use two branches:

* master
* develop

*Note: while in early development, or for minor applications/libraries, a repo might only use a master branch*

The `develop` branch is set to the default branch. All pull requests will have a default target of `develop`, and a freshly cloned repo will clone the `develop` branch.

#### Release management

When the software is ready for a versioned release, the `develop` branch is merged to the `master` branch and a new release is created from the "releases" page. This create a git tag with a version number. All versions should follow [semantic versioning](https://semver.org), the essential part being that all version tags follow the format `vMAJOR.MINOR.PATCH`.

#### Working with the branches

Therefore, to checkout the latest stable version of the source, one must `git checkout master`.

To setup your local copy of the repo for development:

1. Fork the repo within github			
    1. in github.com, click on "fork" in top right. Select your github account as the destination.
1. Clone the original repo to your local machine
    1. `git clone git@github.com/skycoin/REPO-NAME`
1. Add your fork as a remote  
    1. `cd REPO-NAME`			
    1. `git remote add YOUR-USERNAME git@github.com/YOUR-USERNAME/REPO-NAME.git`
    1. `git remote -v` (this should show the following:)
```		
    origin  git@github.com:skycoin/REPO-NAME.git (fetch)
    origin  git@github.com:skycoin/REPO-NAME.git (push)
    YOUR-USERNAME        git@github.com:YOUR-USERNAME/REPO-NAME.git (fetch)
    YOUR-USERNAME        git@github.com:YOUR-USERNAME/REPO-NAME.git (push)
```

To make and submit changes to the repo:
	
1. Checkout a new branch
    1.	`git checkout -b my_branch`
1. Stage changed files for a commit:
    1.	`git add .` (you can specify individual files, or use `.` for all files under the current directory)
1. Commit changes
    1. `git commit -m "A good commit message" (Note: you can include the issue number in the message and Github will link it, e.g. "fixed #39" in the commit message will cause issue #39 to close automatically when the pull request is merged)
1. Push this branch to your upstream fork
    1. `git push YOUR-USERNAME my_branch`
1. Go to the github.com/skycoin/REPO-NAME page and create a pull request from `YOUR-USERNAME/my_branch` to `skycoin/develop`
1. After tests pass and the PR is approved, it can be merged.
1. After the PR is merged, update your local repo copy
    1. `git checkout develop`
    1. `git pull`
    1. `git branch -d my_branch` (optional: delete your merged branch)
1. If there are merge conflicts in the PR, they can be fixed this way:
    1. `git checkout develop`
    1. `git pull`
    1. `git checkout my_branch`
    1. `git merge develop`
    1. `git diff`
    1. Observe files which have merge conflicts and fix the merge conflicts. There are GUI tools that can assist, or the files can be merged by hand in a text editor.
    1. `git add .`
    1. `git push YOUR-USERNAME my_branch`
1. If you need to make more changes to your branch, commit the changes and push again. The PR will automatically update
    1. `git checkout my_branch`
    1. <edit files>
    1. `git add .`
    1. `git commit -m "A good commit message"`
    1. `git push YOUR-USERNAME my_branch`