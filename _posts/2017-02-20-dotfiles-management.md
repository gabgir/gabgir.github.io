---
layout: post
title: How to put your dotfiles on Github
---

A lot of unix apps have their user configuration files saved as `.<something>` in the user home directory (`~/`). Think of Vim for example, which hold the user carefully crafted custimizations in the `.vimrc` file. Those files are often called dotfiles. It is important to back to files up and one good way of doing so is by using git, and even github. This allows you to not only have a version control of your dotfiles, but also sync them between machines.

To create a git repository with your dotfiles, you could simply initialize a repository in your `~/` folder, but then you would need to create a huge and `.gitignore` file to make sure you only cover your dotfiles in that repository. A better solution would be to copy your dotfiles to a separated folder, let's say `~/dotfiles/` and then initialize a git repo in that folder. For example, here is my dotfiles folder structure:

```
gabgir$ ls ~/dotfiles
README.md
vim/
vimrc
tmux.conf
```
(I removed the "." in the files names to make it easier to manipulate them)

The issue with that solution is that if you change your dotfiles in your home directory, you need to remember to copy them over to your `~/dotfiles/` folder.

To avoid that, we can utilize symbolic links. So for each file in your dotfiles folder, you can create a symbolic link with the following command:

```
ln -s ~/dotfiles/vimrc ~/.vimrc
ln -s ~/dotfiles/vim ~/.vim
...
```

That way, they appear in the usual `~/` location, but also sync to the `~/dotfiles/` folder.

On top of backing your dotfiles with git, you can sync them between machines by using github. Simply push your local git repo to your github account. Then, whenever you need to download them to a new machine, you can use:

```
git clone --recursive https://github.com/<yourUserName>/dotfiles.git
```

The recursive flag ensures that if your dotfiles contain git repository, which is often the case with vim plugins, they are also downloaded.

After that, you run the `ln -s` commands again to create the symbolic links on your new machine. A good way to remember those commands is to add them to the repo readme file or even to create an install bash script with those commands.

Another cool trick when using git to manage your dotfiles is that you can create multiple branches to support different configurations. For example, you might have a branch called `red-hat` to save a version of your dotfiles tailored for red hat linux. 
