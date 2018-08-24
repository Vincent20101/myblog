#Anaconda 简介

```
Anaconda 提供一个管理工具 conda ，可以把 conda 看作是
pip + virtualenv +PVM (Python Version Manager) +
一些必要的底层库，也就是一个更完整也更大的集成管理工具。
一般的搞深度学习的都会用这个工具


Anaconda Navigtor ：
用于管理工具包和环境的图形用户界面，后续涉及的众多管理命令也可以在 Navigator 中手工实现。

Jupyter notebook ：
基于web的交互式计算环境，可以编辑易于人们阅读的文档，用于展示数据分析的过程。

qtconsole ：
一个可执行 IPython 的仿终端图形界面程序，相比 Python Shell 界面，
qtconsole 可以直接显示代码生成的图形，实现多行代码输入执行，以及内置许多有用的功能和函数。

spyder ：一个使用Python语言、跨平台的、科学运算集成开发环境。
```


#安装Anaconda
```
从官网可以直接下载，有不同平台的安装文件的。这里马哥采用的是Linux平台，
https://www.anaconda.com/download/#linux

这个是linux平台的下载链接 https://repo.anaconda.com/archive/Anaconda3-5.2.0-Linux-x86_64.sh （2018年08月24日更新）

下载之后是一个shell文件，这个文件有几百MB大小，这是个安装文件，文件的前半部分是普通的shell代码，可以用vim打开看的，
文件的后部分都是二进制内容，看不到什么的，这些内容最后会解压缩到你的安装路径下的。

安装 直接执行 这个 Anaconda3-5.2.0-Linux-x86_64.sh 文件进行

如果没有执行权限可以使用chmo 755 Anaconda3-5.2.0-Linux-x86_64.sh 加上执行权限。

然后是  ./Anaconda3-5.2.0-Linux-x86_64.sh  执行

执行过程是交互式的，非常的简单。

最后是设置一下环境变量path，安装的最后会提示你的。


bash 把环境变量配置到 ~/.bashrc 文件中最后 export PATH="$HOME/anaconda3/bin:$PATH".
这里假设你的anaconda3安装到了家目录下面。

zsh 把环境变量配置到 ~/.zshrc  文件中，添加
path=(
$HOME/anaconda3/bin
~/bin
$path
)



```

安装完之后就可以试试命令是否可以正确执行了


#常用命令
```
切换环境
activate learn  切换环境，linux需要前面加上个source

列出所有的环境
conda env list

conda upgrade --all 先把所有工具包进行升级

```


```

$ conda env create --help
usage: conda-env create [-h] [-f FILE] [-n ENVIRONMENT | -p PATH] [-q]
                        [--force] [--json] [--debug] [--verbose]
                        [remote_definition]

Create an environment based on an environment file

Options:

positional arguments:
  remote_definition     remote environment definition / IPython notebook

optional arguments:
  -h, --help            Show this help message and exit.
  -f FILE, --file FILE  environment definition file (default: environment.yml)
  -n ENVIRONMENT, --name ENVIRONMENT
                        Name of environment.
  -p PATH, --prefix PATH
                        Full path to environment prefix.
  -q, --quiet
  --force               force creation of environment (removing a previously
                        existing environment of the same name).
  --json                Report all output as json. Suitable for using conda
                        programmatically.
  --debug               Show debug output.
  --verbose, -v         Use once for info, twice for debug, three times for
                        trace.

examples:
    conda env create
    conda env create -n name
    conda env create vader/deathstar
    conda env create -f=/path/to/environment.yml
    conda env create -f=/path/to/requirements.txt -n deathstar
    conda env create -f=/path/to/requirements.txt -p /home/user/software/deathstar


$ conda env create -f py35.yml


```

# 导入环境

这里给个python3.5.4的环境。可以直接导入使用，非常方便的。
```text
name: py35
channels:
- conda-forge
- defaults
dependencies:
- bleach=2.0.0=py35_0
- entrypoints=0.2.3=py35_1
- gmp=6.1.2=0
- html5lib=1.0.1=py_0
- ipywidgets=7.1.0=py35_0
- jinja2=2.10=py35_0
- jsonschema=2.6.0=py35_0
- markupsafe=1.0=py35_0
- mistune=0.8.3=py_0
- nbconvert=5.3.1=py_1
- nbformat=4.4.0=py35_0
- notebook=5.2.2=py35_1
- pandoc=2.0.5=0
- pandocfilters=1.4.1=py35_0
- terminado=0.8.1=py35_0
- testpath=0.3.1=py35_0
- webencodings=0.5=py35_0
- widgetsnbextension=3.1.0=py35_0
- bzip2=1.0.6=h6d464ef_2
- ca-certificates=2017.08.26=h1d4fec5_0
- cairo=1.14.10=hdf128ce_6
- certifi=2017.11.5=py35h9749603_0
- cycler=0.10.0=py35hc4d5149_0
- dbus=1.10.22=h3b5a359_0
- decorator=4.1.2=py35h3a268aa_0
- expat=2.2.5=he0dffb1_0
- ffmpeg=3.4=h7264315_0
- fontconfig=2.12.4=h88586e7_1
- freetype=2.8=hab7d2ae_1
- glib=2.53.6=h5d9569c_2
- graphite2=1.3.10=hc526e54_0
- gst-plugins-base=1.12.2=he3457e5_0
- gstreamer=1.12.2=h4f93127_0
- h5py=2.8.0=py35hca9c191_0
- harfbuzz=1.5.0=h2545bd6_0
- hdf5=1.8.18=h6792536_1
- icu=58.2=h9c2bf20_1
- intel-openmp=2018.0.0=hc7b2577_8
- ipykernel=4.6.1=py35h29d130c_0
- ipython=6.2.1=py35hd850d2a_1
- ipython_genutils=0.2.0=py35hc9e07d0_0
- jasper=1.900.1=hd497a04_4
- jedi=0.11.0=py35h48b7ba3_0
- jpeg=9b=h024ee3a_2
- jupyter_client=5.1.0=py35h2bff583_0
- jupyter_core=4.4.0=py35ha89e94b_0
- libedit=3.1=heed3624_0
- libffi=3.2.1=hd88cf55_4
- libgcc-ng=7.2.0=h7cc24e2_2
- libgfortran-ng=7.2.0=h9f7466a_2
- libopus=1.2.1=hb9ed12e_0
- libpng=1.6.32=hbd3595f_4
- libsodium=1.0.15=hf101ebd_0
- libstdcxx-ng=7.2.0=h7a57d05_2
- libtiff=4.0.9=h28f6b97_0
- libvpx=1.6.1=h888fd40_0
- libxcb=1.12=hcd93eb1_4
- libxml2=2.9.4=h2e8b1d7_6
- libxslt=1.1.29=h78d5cac_6
- lxml=4.1.1=py35ha19ceee_0
- matplotlib=2.1.0=py35h2cbf27e_0
- mkl=2018.0.1=h19d6760_4
- ncurses=6.0=h9df7e31_2
- numpy=1.13.3=py35hd829ed6_0
- olefile=0.44=py35h2c86149_0
- opencv=3.4.1=py35h40b0b35_1
- openssl=1.0.2m=h26d622b_1
- pandas=0.21.0=py35hee8c687_1
- parso=0.1.0=py35ha74fa24_0
- patsy=0.4.1=py35h51b66d5_0
- pcre=8.41=hc27e229_1
- pexpect=4.3.0=py35hf410859_0
- pickleshare=0.7.4=py35hd57304d_0
- pillow=4.3.0=py35h550890c_1
- pip=9.0.1=py35h7e7da9d_4
- pixman=0.34.0=hceecf20_3
- prompt_toolkit=1.0.15=py35hc09de7a_0
- ptyprocess=0.5.2=py35h38ce0a3_0
- pygments=2.2.0=py35h0f41973_0
- pyparsing=2.2.0=py35h041ed72_1
- pyqt=5.6.0=py35h0e41ada_5
- python=3.5.4=h417fded_24
- python-dateutil=2.6.1=py35h90d5b31_1
- pytz=2017.3=py35hb13c558_0
- pyzmq=16.0.3=py35ha889422_0
- qt=5.6.2=h974d657_12
- readline=7.0=ha6073c6_4
- scikit-learn=0.19.1=py35hbf1f462_0
- scipy=1.0.0=py35hcbbe4a2_0
- seaborn=0.8.1=py35h04cba02_0
- setuptools=36.5.0=py35ha8c1747_0
- simplegeneric=0.8.1=py35h2ec4104_0
- sip=4.18.1=py35h9eaea60_2
- six=1.11.0=py35h423b573_1
- sqlite=3.20.1=hb898158_2
- statsmodels=0.8.0=py35haa9d50b_0
- tk=8.6.7=hc745277_3
- tornado=4.5.2=py35hf879e1d_0
- tqdm=4.19.4=py35h68e51d2_0
- traitlets=4.3.2=py35ha522a97_0
- wcwidth=0.1.7=py35hcd08066_0
- wheel=0.30.0=py35hd3883cf_1
- xz=5.2.3=h55aa19d_2
- zeromq=4.2.2=hbedb6e5_2
- zlib=1.2.11=ha838bed_2
- pip:
  - absl-py==0.1.11
  - audioread==2.1.6
  - beautifulsoup4==4.6.0
  - easydict==1.8
  - enum34==1.1.6
  - ipython-genutils==0.2.0
  - joblib==0.12.0
  - jupyter-client==5.1.0
  - jupyter-core==4.4.0
  - jupyterthemes==0.18.3
  - lesscpy==0.11.2
  - librosa==0.6.1
  - llvmlite==0.23.0
  - markdown==2.6.11
  - numba==0.38.0
  - opencv-python==3.4.1.15
  - ply==3.10
  - prompt-toolkit==1.0.15
  - protobuf==3.5.2.post1
  - pydub==0.22.1
  - pypinyin==0.30.0
  - pytesseract==0.1.7
  - pytube==7.0.18
  - resampy==0.2.0
  - tensorflow-gpu==1.5.0
  - tensorflow-tensorboard==1.5.1
  - werkzeug==0.14.1
prefix: $HOME/anaconda3/envs/py35


```
















