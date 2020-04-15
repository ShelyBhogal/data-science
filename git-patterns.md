# Common patterns in git

## Terminology

On this course you have three repositories

 - local: the repo on your machine. These are the files you play with all the time
 - origin: your repo on github. We push our local changes to the origin to make sure they are saved! Also to share them with others 
 - upstream: Noelia's (the source) repo on github. We can pull changes from here but we are not allowed to push.

## Get started

 - Fork Noelia's repo (the source) `https://git.generalassemb.ly/DSga38/ds_ga_38`
 - Now clone your fork `git clone https://git.generalassemb.ly/yourgitname/ds_ga_38`
 - You now have a local copy of your remote! `ls` will show you the new directory
 - Enter the new local repo `cd ds_ga_38`
 - Because we cloned the repo, your local repo already knows about the origin type `git remote -v` to see.
 - We're going to pull changes from Noelia's repo too (when she adds files). To do that we need to tell our local repo where to find those changes too. We add a new remote. `git remote add upstream https://git.generalassemb.ly/DSga38/ds_ga_38`

Done!

## Saving your work

Saving = commiting

 - You've made some local changes. Type `git status` and it'll show anything that hasn't been committed
 - `git add file.txt subfolder/image.png` will add files that you plan to save/commit
 - `git commit -m 'my fun changes` will commit the changes with a message
 - `git commit` will commit the changes and (unfortunately) through you into the Vim text editor. Very hard to escape from!
   - If you end up in Vim
   - Type `i` to start inserting text
   - Type your message into the terminal
   - Press the ESC key to stop inserting text
   - Type `:wq` to **w**rite your changes and **q**uit

## Save your work onto your remote

 - `git push origin master` will push your master branch onto your remote repo
 - If pushing gives you errors it's because you've mad clashing changes already and put those on your remote repo.
 - **DANGER** `git push origin master -f` will completely overwrite the remote with your local version
 - if you don't want that then read instructions for merging changes

## Getting changes from the upstream

Noelia regularly uploads new files to the upstream (the source) repo. We often need to combine her changes with our own.

 - `git fetch upstream` will get all of Noelia's changes and puts them on a local branch called `upstream/master`
 - You can even browse the work on your machine `git checkout upstream/master` then have a look around.
 - To go back to your latest work type `git checkout master`
 - Now we'll apply your changes on top of Noelia's. `git rebase upstream/master`
 - **Alternatively** you can merge `git merge upstream/master`
 - Now these might work perfectly and you have successfully combined the latest changes from upstream with your local changes.
 - **You're asked for a commit message**
   - You are probably thrown into vim. Follow the standard advice:
   - Type `i`
   - Type your message
   - Press ESC key
   - Type `:wq`
 - **You get merge conflicts / asked to do some work / rebase conflict**
   - Follow the advice below on resolving conflicts


## Resolving conflicts after a rebase

 - You typed `git rebase upstream/master` and you're told that there are conflicts. This means that you AND Noelia have made some changes to the same files.
 - Cancel the rebase so we're back to the start `git rebase --abort`
 - Normally you need to manually edit these conflicting files and then commit them again
 - **For this course** its usually fine to prefer your own changes. You can tell git to do that with `git rebase upstream/master -Xtheirs`. If git finds that you and Noelia changed a file, then it will choose your changes always. This way you tell git how to handle the conflits itself. Easy!

## Submitting homework

When you submit your homework, you need to submit a very clean commit (containing only your homework file) to Noelia's repo.

 - First! You must clean up your local repository. So follow the above instructions to commit all your local changes and also push them to your personal remote (the origin).
 - Now, you've probably made a lot of changes to many files including the homework file.
 - We want to create a really clean, simple commit that Noelia can easily merge into her work. We definetly don't want to submit all of our files and force her to wade through them to find what she needs.
 - The best way to start, when you're contributing to someone elses work, it start where they are right now. 
 - So fetch the latest changes from Noelia `git fetch upstream`.
 - Now switch to the upstream version of master `git checkout upstream/master`
 - We are **not allowed** to make changes to the upstream master and push them. Instead we create a new branch (where we'll put our homework), which uses the upstream as a starting point.
 - Type `git checkout -b my-homework` will create a new branch called `my-homework`, which starts off identical to `upstream/master` (because that's where we were when we created the new branch)
 - Let's review
  - We have `master` our local branch, full of lots of changes that we've changed including our homework file
  - We have `upstream/master` which is Noelia's, we can't make changes here
  - We have `my-homework`, which is currently identical to `upstream/master` but we're going to add our homework to it and then ask Noelia to merge that commit into her code.
  - So we need to work on `my-homework`!
 - Type `git branch -a` and you can see all the branches. Make sure that there is a `*` next to your homework branch e.g. `my-homework`
 - We're now going to grab our homework file from `master` and put it on `my-homework`.
 - `git checkout master path/to/homework-file.ipynb` you should type whatever file you want to checkout
 - Now `git status` will show that there is just one change! Your added file.
 - `git add path/to/homework-file.ipynb` to add the homework
 - `git commit -m 'oliver added homework'` to commit the work locally.
 - Now we have a tidy commit, that can easily placed on top of Noelia's work.
 - Push your changes to your personal remote `git push -u origin my-homework`
 - Now go to github and you'll see there is a new branch called `my-homework` which is very similar to Noelia's repo but with your homework placed on top.
 - Go to `https://git.generalassemb.ly/DSga38/ds_ga_38` and click pull requests.
 - Click "compare across forks"
 - Now change "head repository" to `yourgitname/my-homework`
 - And submit the PR!
