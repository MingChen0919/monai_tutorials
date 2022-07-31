# Config colab python environment

## Put your project folder within Google Drive folder

First, put your project folder in your Google Drive. For example, below is my project path.

```text
/Users/mingchen0919/Google Drive/colab_projects/monai_tutorials
```

## Create python virtual environment

Create a python virtual environment within the project folder

```shell
cd /Users/mingchen0919/Google\ Drive/colab_projects/monai_tutorials
python -m venv venv
```

## Install packages

Activate python virtual environment and install dependencies

```shell
source ./venv/bin/activate

python -m pip install pip
python -m pip install matplotlib
python -m pip install notebook
pip install -r https://raw.githubusercontent.com/Project-MONAI/MONAI/dev/requirements-dev.txt
```

## Deal with symbolic links in virtual environment

If you go to the `./venv/bin` folder, you will find that the `python` and `python3` files are actually symbolic links.
Google Drive doesn't allow you to sync these symbolic links to it. We will have to replace them with the original python
interpreter files.

```text
(venv) (base) ➜  monai_tutorials git:(main) ✗ ls -lth venv/bin | grep python
lrwxr-xr-x   1 mingchen0919  staff     6B Jul 31 12:08 python3.9 -> python
lrwxr-xr-x   1 mingchen0919  staff     6B Jul 31 12:08 python3 -> python
lrwxr-xr-x   1 mingchen0919  staff    26B Jul 31 12:08 python -> /opt/miniconda3/bin/python
```

Remove symbolic links:

```shell
rm venv/bin/python3.9
rm venv/bin/python3
rm venv/bin/python
```

Copy original files to the same places:

```shell
cp /opt/miniconda3/bin/python ./venv/bin/python
cp /opt/miniconda3/bin/python ./venv/bin/python3
cp /opt/miniconda3/bin/python ./venv/bin/python3.9
```

## Config python environment in colab notebook

Now the project has a python virtual environment folder. We just need to mount google drive to colab to make it
available.

```shell
from google.colab import drive
drive.mount('/content/drive')
```

Colab has its own python interpreter. We need to update the environment variable `$PATH` so that our own python
environment can be picked up first.

```shell
import os
import sys

PROJECT_DIR = '/content/drive/MyDrive/colab_projects/monai_tutorials'
os.chdir(PROJECT_DIR)
os.environ['PATH'] = f'{os.path.join(PROJECT_DIR, "venv", "bin")}:{os.environ["PATH"]}'
```

When we upload the `./venv` folder to google drive, all the files in `./venv/bin` do not have execute permission. Let's
change that.

```shell
!chmod +x -R venv/bin/
```

Okay, now you are good to go. Enjoy!