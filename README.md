# Make sure that Jupyter Lab/Notebook follows the XDG Base Directory Specification

[Project Jupyter](https://jupyter.org/) offers a great way for anyone who wishes for an interactive computing experience, especially data scientists.

To use Jupyter, one can use extensions for existing text editors/IDE such as VS Code/Codium, Vim/Neovim, and so on, or they can use Jupyter's own in house solutions Jupyter Notebook/Lab.

While they are not perfect, they get the job done and provide an environment that solely focuses on the project. There are even some basic customisation such as dark theme and whatnot. For my personal workflow, I use Neovim for everything else, and Jupyter Lab for jupyter things, mainly because I have not found a plug-in that quite suit my needs. The text-base interface that Vim/Neovim has as the largest advantage falls apart rather quickly when doing something interactive I find.

The problem is that out of the box, neither support the [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/latest/) and [put files in the home directory](https://github.com/jupyterlab/jupyterlab/issues/14900). It should not matter to a normal person who have a life beyond the computer, but I'm not, and messy files on the home directory get on my nerves. Things like Kate or Sublime Text might not put random files on the home directory, but they are hard to set up with Jupyter in my humble experience and in the case of Sublime, are not even Free software (I tend to stick to free software when I can), VS Code/Codium are technically open source, so not the best, but better, and setting up is easy, but they put their own folder in the home directory, which is, again, bad.

I'm sure the devs have very good reasons to not follow the Specification, but I don't care, and for now at least, I have found a way to make sure that at least on the surface level, Jupyter Lab/Notebook follows the XDG Base Directory Specification.

**I am doing this on Vanilla Arch, the distro should not matter, but I'm not sure.**

[The Arch Wiki](https://wiki.archlinux.org/title/XDG_Base_Directory) suggested a bunch of environmental variables, which normally I put in my `bash_profile` file. However, putting them there and sourcing the file on startup doesn't do it. It stops the programs from creating redundant files if they're launched from the terminal, but if you use something like Rofi, you're out of luck.

What I've done was edit the `/etc/environment` file using the `sudoedit` command. In there, you should put:

```
JUPYTER_CONFIG_DIR=/home/<yourusername>/.config/jupyter/
JUPYTER_PLATFORM_DIRS="1"
```

Next was the problem of npm. Again, [The Arch Wiki](https://wiki.archlinux.org/title/XDG_Base_Directory) have a suggestion, but it was not enough. Changing the location of `npmrc` and putting the right settings there (as indicated in the page) was one thing, but I still had to edit the environment file again. Again, the command is:

```sudoedit /etc/environment```

In there, you'd want to put:

```
npm_config_cache=/home/<yourusername/.cache/npm/
```

After that, make sure that all the changes are saved, and reboot. You should be good to go.

I wanted to include this in my install script, but I figured not a lot of people are going to use my script, and if they do, they won't have much use for such customisation that could break their system since putting user names on such an important file could spell disaster if not done correctly. If they have that use, I could find this guide, or they could figure it out on their own. The point is that it benefits to few people to warrant the risks and work that I have to manage.
