---
title: "Understanding Conda: Your Guide to Virtual Environment Management"
layout: post
tags:
  - python
  - conda
description: A practical introduction to Conda, environments, channels, and package management.
---

Anaconda Distribution is a free Python/R data science distribution. It contains:

- `conda`: a package and environment manager
- **Anaconda Navigator**: a desktop application built on `conda`
- over 250 scientific and machine learning packages

## How is it different from other virtual environment managers?

- `conda` lets you choose the Python version for each environment.
- Conda channels are package repositories.
- Each env change is tracked as a revision, allowing rollback.

## How to install packages in a conda environment?

Use `conda install pkgname` or `pip install` inside the environment.

`conda` first checks channels, dependencies, and conflicts before installing a package.

## Some basic operations in `conda`

- `conda info`
- `conda info -e`
- `conda activate`
- `conda activate envname`
- `conda deactivate`
- `conda create --name env`
- `conda list`
- `conda install ipykernel`
- `conda update --all --yes`
- `conda remove --name env --all`
- `conda search -f pkg`
- `conda clean`

## References

- Anaconda official site
- Conda documentation
