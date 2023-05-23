---
layout: default
title: Install
grand_parent: SMDRM
parent: Get Started
nav_order: 1
---

# Prerequisites

The following applications should be installed on your machine:
- Python 3.10.x
- wget

# Get source code

Clone the [source code](https://github.com/panc86/smdrm_prod) in the NFS server VM.

{: .warning}
You may need to request access to the repository owner.

# Build Virtual Environment

Build and activate a new Python 3.10.x virtual environment to host the requirements.

# Install dependencies

{: .warning}
First execution time may take few minutes to complete... ☕ ☕

Install the SMDRM package.

```shell
pip install .
```

Install the pipeline dependencies.

```shell
bash ./tools/init.sh </nfs/share/absolute/path>
```

# What's Next?

Let us [deploy the Docker stack in Swarm mode]({{ site.baseurl }}/smdrm/deploy).
