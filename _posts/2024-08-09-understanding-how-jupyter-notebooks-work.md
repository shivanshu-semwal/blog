---
title: "Understanding How Jupyter Notebooks Work"
layout: post
tags:
  - jupyter-notebook
  - ipython
description: A quick overview of how Jupyter notebooks, kernels, and the IPython protocol interact.
---

Main focus of this post is the YouTube video: Jupyter: Kernels, Protocols, and the IPython Reference Implementation.

REPL (Read Evaluate Print Loop) is an interactive shell for Python. A notebook extends this model by decoupling the interface and the execution engine.

The notebook server and kernel communicate over ZeroMQ, while the browser and notebook server communicate via WebSockets.

![Jupyter architecture](https://cdn.hashnode.com/res/hashnode/image/upload/v1723227154482/c9d0e90d-9b51-4368-b6fc-5817ba717938.png)

More details: https://jupyter-client.readthedocs.io/en/latest/messaging.html
