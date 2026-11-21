---
title: "Understanding Conda: Your Guide to Virtual Environment Management"
layout: post
tags:
  - python
  - conda
description: A practical introduction to Conda, environments, channels, and package management.
---


Anaconda Distribution is a free Python/R data science distribution. It contains:
- `conda` - a package and environment manager for your command line interface. `conda` is also written in python and is a python package.
- **Anaconda Navigator** - a desktop application built on `conda`, with options to launch other development applications from your managed environments
- over 250 scientific and machine learning packages

## How is it different compared to other virtual environment managers?

- **Python** - In `conda` you can also chose the version of the python you want. It will then download the version of python that you asked for and then install it for your virtual environment.
- **Channels** - Conda channels are the locations where packages are stored. You can have multiple channels from where you can get packages, and you can set there priority. Also they are prebuilt packages, so you won't have to compile the whole package yourself. Some famous channels are - `conda-forge`, `anaconda`
- **History/Revisions** - whenever you make some change to the environment it is stored as a revision to the environment, and if you want to revert back to a previous revision you can do that.

## How to install packages in a `conda` environment?

In `conda` environment you can install packages using normal `pip`, that will download the latest version of packages that your according the python you selected.

To install packages using conda you can use `conda install pkgname`. This will install packages form the conda repository. There are `channels` in conda, each channel is connected to a specific repository for packages.

### What happens when you do `conda install`?

When you do conda install, conda first look for the channels to which it have to search in, then it check for other dependencies and conflicts and find a suitable match which package to install, which package to upgrade and which to downgrade to meet the user need.

This take more time if the metadata is more and more packages are installed in an environment through conda. So it is advised to install all the packages in the starting when creating an environment.

## Some basic operations in `conda`

- `conda info` - Display information about current conda install.
- `conda info -e` - Display information about all the conda environments.

- `conda activate` - activate the default base environment
- `conda activate envname` - activate the `envname` environment
- `conda deactivate` - deactivate the active environment
- `conda config --set auto_activate_base false` - this will create a `.condarc` file in the home directory and then add this option there. This option will stop the base environment form activating automatically every time you start a shell.
- `conda create --name env` - create a environment with name env
- `conda create -n temp python=3.4` - create a environment `temp` with `python 3.4` version
- `conda list` - List linked packages in a conda environment.
- `conda install -c channel-name pkg` install `pkg` form channel `channel-name`
- `conda install ipykernel` - install `ipykernel` in current environment so we can open it in Jupiter notebook.
- `conda update --all --yes` - update all packages
- `conda remove --name env --all` - remove `env` package
- `conda config --show channels` - show list of channels for current env
- `conda config --show default_channels` - show default channels
- `conda config --add channels channel-name` - add channel `channel-name` to current env
- `conda config --get channels` - show channels and there priority also.
- `conda config --set channel_priority strict` - set the channel priority to strict
- `conda list --revision` - show the list of revisions a env have
- `conda search -f pkg` - search for a package in conda.
- `conda clean` - Remove unused packages and caches.

## References

- [Anaconda Official Site](https://www.anaconda.com/)
- [Conda Documentation](https://docs.anaconda.com/anaconda/getting-started/index.html)
- [Managing conda channels](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-channels.html)
