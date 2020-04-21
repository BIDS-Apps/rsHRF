# Your version: 0.6.0 Latest version: 0.7.0
# Generated by Neurodocker version 0.6.0
# Timestamp: 2020-04-21 18:20:37 UTC
# 
# Thank you for using Neurodocker. If you discover any issues
# or ways to improve this software, please submit an issue or
# pull request on our GitHub repository:
# 
#     https://github.com/kaczmarj/neurodocker

Bootstrap: docker
From: neurodebian

%post
export ND_ENTRYPOINT="/neurodocker/startup.sh"
apt-get update -qq
apt-get install -y -q --no-install-recommends \
    apt-utils \
    bzip2 \
    ca-certificates \
    curl \
    locales \
    unzip
apt-get clean
rm -rf /var/lib/apt/lists/*
sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
dpkg-reconfigure --frontend=noninteractive locales
update-locale LANG="en_US.UTF-8"
chmod 777 /opt && chmod a+s /opt
mkdir -p /neurodocker
if [ ! -f "$ND_ENTRYPOINT" ]; then
  echo '#!/usr/bin/env bash' >> "$ND_ENTRYPOINT"
  echo 'set -e' >> "$ND_ENTRYPOINT"
  echo 'export USER="${USER:=`whoami`}"' >> "$ND_ENTRYPOINT"
  echo 'if [ -n "$1" ]; then "$@"; else /usr/bin/env bash; fi' >> "$ND_ENTRYPOINT";
fi
chmod -R 777 /neurodocker && chmod a+s /neurodocker

apt-get update -qq
apt-get install -y -q --no-install-recommends \
    libpng \
    freetype \
    libstdc++ \
    openblas \
    libxml2 \
    libxslt \
    g++ \
    gfortran \
    file \
    binutils \
    openblas-dev \
    python3-dev \
    gcc \
    build-base \
    libpng-dev \
    musl-dev \
    freetype-dev \
    libxml2-dev \
    libxslt-dev
apt-get clean
rm -rf /var/lib/apt/lists/*

test "$(getent passwd rshrf)" || useradd --no-user-group --create-home --shell /bin/bash rshrf
su - rshrf

export PATH="/opt/miniconda-latest/bin:$PATH"
echo "Downloading Miniconda installer ..."
conda_installer="/tmp/miniconda.sh"
curl -fsSL --retry 5 -o "$conda_installer" https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash "$conda_installer" -b -p /opt/miniconda-latest
rm -f "$conda_installer"
conda update -yq -nbase conda
conda config --system --prepend channels conda-forge
conda config --system --set auto_update_conda false
conda config --system --set show_channel_urls true
sync && conda clean --all && sync
conda create -y -q --name rshrf
conda install -y -q --name rshrf \
    "python=3.7" \
    "pandas" \
    "scipy==1.3.3" \
    "numpy" \
    "matplotlib" \
    "joblib"
sync && conda clean --all && sync
bash -c "source activate rshrf
  pip install --no-cache-dir  \
      "rsHRF""
rm -rf ~/.cache/pip/*
sync
sed -i '$isource activate rshrf' $ND_ENTRYPOINT


mkdir -p ~/.jupyter && echo c.NotebookApp.ip = \"0.0.0.0\" > ~/.jupyter/jupyter_notebook_config.py

cd /home/rshrf

echo '{
\n  "pkg_manager": "apt",
\n  "instructions": [
\n    [
\n      "base",
\n      "neurodebian"
\n    ],
\n    [
\n      "_header",
\n      {
\n        "version": "generic",
\n        "method": "custom"
\n      }
\n    ],
\n    [
\n      "install",
\n      [
\n        "libpng",
\n        "freetype",
\n        "libstdc++",
\n        "openblas",
\n        "libxml2",
\n        "libxslt",
\n        "g++",
\n        "gfortran",
\n        "file",
\n        "binutils",
\n        "openblas-dev",
\n        "python3-dev",
\n        "gcc",
\n        "build-base",
\n        "libpng-dev",
\n        "musl-dev",
\n        "freetype-dev",
\n        "libxml2-dev",
\n        "libxslt-dev"
\n      ]
\n    ],
\n    [
\n      "user",
\n      "rshrf"
\n    ],
\n    [
\n      "miniconda",
\n      {
\n        "conda_install": [
\n          "python=3.7",
\n          "pandas",
\n          "scipy==1.3.3",
\n          "numpy",
\n          "matplotlib",
\n          "joblib"
\n        ],
\n        "pip_install": [
\n          "rsHRF"
\n        ],
\n        "create_env": "rshrf",
\n        "activate": true
\n      }
\n    ],
\n    [
\n      "run",
\n      "mkdir -p ~/.jupyter && echo c.NotebookApp.ip = \\\"0.0.0.0\\\" > ~/.jupyter/jupyter_notebook_config.py"
\n    ],
\n    [
\n      "entrypoint",
\n      "/neurodocker/startup.sh"
\n    ],
\n    [
\n      "workdir",
\n      "/home/rshrf"
\n    ]
\n  ]
\n}' > /neurodocker/neurodocker_specs.json

%environment
export LANG="en_US.UTF-8"
export LC_ALL="en_US.UTF-8"
export ND_ENTRYPOINT="/neurodocker/startup.sh"
export CONDA_DIR="/opt/miniconda-latest"
export PATH="/opt/miniconda-latest/bin:$PATH"

%runscript
/neurodocker/startup.sh
