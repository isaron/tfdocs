# 在 Ubuntu 上安装 TensorFlow

本指南解释了如何在 Ubuntu 操作系统安装 TensorFlow。这些安装步骤和操作指令也可能在其他Linux变体中有效，但官方只测试了(同时官方也只支持)这些安装步骤和操作指令在Ubuntu 14.04或更高版本中的有效性。


## 确定需要安装的 TensorFlow 的类型

选择以下之一类型的 TensorFlow 来安装:

  * **仅支持 CPU 运算的 TensorFlow**。如果系统中没有NVIDIA® GPU，那么必须安装这个版本。这个版本的TensorFlow一般来说更容易安装（只需要5到10分钟），因此，即使你的系统中有NVIDIA® GPU，我们也建议首先安装这个版本。
  * **支持 GPU 运算的 TensorFlow**。TensorFlow程序一般运行在GPU上比运行在CPU上要快很多。因此，如果系统中有NVIDIA® GPU同时也满足如下所示的先决条件，并且需要运行性能关键型应用程序，那么还是安装这个版本吧。


<a name="NVIDIARequirements"></a>
### 运行支持 GPU 运算的 TensorFlow 必须满足的先决条件

需要安装 GPU 支持的 TensorFlow、并且期望能够实现本指南中所描述的各种应用场景，那么您的系统中首先必须安装下面这些 NVIDIA 软件：

  * CUDA® Toolkit 8.0. 详细的介绍，请参考
    [NVIDIA's documentation](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/#axzz4VZnqTJ2A)。
    确保按照 NVIDIA 的文档添加相关的 Cuda 路径到 `LD_LIBRARY_PATH` 环境变量里。
  * CUDA Toolkit 8.0 相关的 NVIDIA 驱动。
  * cuDNN v5.1. 详细的介绍，请参考
    [NVIDIA's documentation](https://developer.nvidia.com/cudnn)。
    确保按照 NVIDIA 的文档创建 `CUDA_HOME` 环境变量。
  * 具有 CUDA Compute Capability 3.0 以上支持的 GPU. 受支持的 GPU 列表请参考
    [NVIDIA documentation](https://developer.nvidia.com/cuda-gpus)。
  * libcupti-dev（NVIDIA CUDA Profile Tools Interface）开发库提供了对高级分析的支持。要安装这个库，可以使用如下命令：

    <pre>
    $ <b>sudo apt-get install libcupti-dev</b>
    </pre>

如果曾经安装过上面提到的软件包的早期版本，那么请升级到指定的版本。确实升级不了，也还是可以运行 GPU 的支持 TensorFlow 的,但需要满足以下几点：

  * 按照文档“[从源代码安装 TensorFlow](install_sources)”完成 TensorFlow 的安装。
  * 安装或者升级如下的 NVIDIA 软件到指定版本:
    * CUDA toolkit 7.0 及以上
    * cuDNN v3 及以上
    * 具有 CUDA Compute Capability 3.0 及以上支持的 GPU。


## 确定如何安装 TensorFlow

您需要从下面几种受支持的安装方式中挑一种来安装 TensorFlow：

  * [virtualenv](#InstallingVirtualenv)
  * ["native" pip](#InstallingNativePip)
  * [Docker](#InstallingDocker)
  * [Anaconda](#InstallingAnaconda)
  * 还有从源代码安装，这个有一个[单独的文档](install_sources)。

**(官方)强烈建议采用第一种方法：virtualenv。**
[Virtualenv](https://virtualenv.pypa.io/en/stable/)是一个虚拟的Python环境，用以隔离同一台设备上的Python开发和运行环境，使Python程序不干扰其它Python程序的运行，同时也不被其他Python程序所影响。在基于virtualenv的安装过程中，你不仅会安装TensorFlow、同时也会安装TensorFlow需要的所有软件包（所以，用这种方法安装TensorFlow真的是非常容易的）。要让TensorFlow开始工作，你需要做的仅仅是“激活（activate）”虚拟环境就可以了。总之，virtualenv提供了安全安装和运行TensorFlow的可靠机制。

不通过任何容器系统，而是直接通过本地pip命令安装TensorFlow。 **对于希望在多用户系统环境中快速部署TensorFlow的系统管理员们，我们推荐使用本地pip安装的方式。** 需要注意的是，由于本地pip命令的安装过程并没有被隔离在任何的容器环境中，所以可能会干扰同一系统里其他的基于python的安装过程。但是，如果你了解pip以及Python环境的话，同样就会知道，基于本地pip的安装通常只需要一个命令。

Docker在你的设备上为TensorFlow建立了完全隔离的环境。预安装了软件包的Docker容器中，包含了TensorFlow本身及其所有的依赖项。请注意，Docker映像可能会很大(几百MB)。如果你希望在一个规模比较大、而且已经使用了Docker的应用架构中建立TensorFlow，那么你可以选择基于Docker的安装。

在Anaconda中，你可以使用`conda`来创建虚拟环境。然而，对于Anaconda计算环境，我们还是建议使用`pip install`命令，而不是`conda install`命令来安装TensorFlow。

**注意：**conda安装包是由社区支持的，而不是官方支持的。也就是说，TensorFlow团队既不测试也不维护conda包。其中可能的风险自担。


<a name="InstallingVirtualenv"></a>
## 基于 virtualenv 安装

采取以下步骤来基于Virtualenv安装TensorFlow：

  1. 通过以下命令来安装pip和virtualenv：

     <pre>$ <b>sudo apt-get install python-pip python-dev python-virtualenv</b> # for Python 2.7
     $ <b>sudo apt-get install python3-pip python3-dev python-virtualenv</b> # for Python 3.n</pre>

  2. 通过以下命令来创建一个virtualenv环境：

     <pre>$ <b>virtualenv --system-site-packages</b> <i>targetDirectory</i> # for Python 2.7
     $ <b>virtualenv --system-site-packages -p python3</b> <i>targetDirectory</i> # for Python 3.n</pre>

     <code><em>targetDirectory</em></code> 指定了 virtualenv 目录所在，<code><em>targetDirectory</em></code> 的默认值是 `~/tensorflow`，当然你可以选择任何目录。

  3. Activate the virtualenv environment by issuing one of the following
     commands:

     <pre>$ <b>source ~/tensorflow/bin/activate</b> # bash, sh, ksh, or zsh
     $ <b>source ~/tensorflow/bin/activate.csh</b>  # csh or tcsh</pre>

     The preceding <tt>source</tt> command should change your prompt
     to the following:

     <pre>(tensorflow)$ </pre>

  4. Ensure pip ≥8.1 is installed:

     <pre>(tensorflow)$ <b>easy_install -U pip</b></pre>

  5. Issue one of the following commands to install TensorFlow in the active
     virtualenv environment:

     <pre>(tensorflow)$ <b>pip install --upgrade tensorflow</b>      # for Python 2.7
     (tensorflow)$ <b>pip3 install --upgrade tensorflow</b>     # for Python 3.n
     (tensorflow)$ <b>pip install --upgrade tensorflow-gpu</b>  # for Python 2.7 and GPU
     (tensorflow)$ <b>pip3 install --upgrade tensorflow-gpu</b> # for Python 3.n and GPU</pre>

     If the preceding command succeeds, skip Step 5. If the preceding
     command fails, perform Step 5.

  5. (Optional) If Step 4 failed (typically because you invoked a pip version
     lower than 8.1), install TensorFlow in the active virtualenv environment
     by issuing a command of the following format:

     <pre>(tensorflow)$ <b>pip install --upgrade</b> <i>tfBinaryURL</i>   # Python 2.7
     (tensorflow)$ <b>pip3 install --upgrade</b> <i>tfBinaryURL</i>  # Python 3.n </pre>

     where <code><em>tfBinaryURL</em></code> identifies the URL of the
     TensorFlow Python package. The appropriate value of
     <code><em>tfBinaryURL</em></code>depends on the operating system,
     Python version, and GPU support. Find the appropriate value for
     <code><em>tfBinaryURL</em></code> for your system
     [here](#the_url_of_the_tensorflow_python_package).  For example, if you
     are installing TensorFlow for Linux, Python 3.4, and CPU-only support,
     issue the following command to install TensorFlow in the active
     virtualenv environment:

     <pre>(tensorflow)$ <b>pip3 install --upgrade \
     https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.3.0-cp34-cp34m-linux_x86_64.whl</b></pre>

If you encounter installation problems, see
[Common Installation Problems](#common_installation_problems).


### Next Steps

After installing TensorFlow,
[validate the installation](#ValidateYourInstallation).

Note that you must activate the virtualenv environment each time you
use TensorFlow. If the virtualenv environment is not currently active,
invoke one of the following commands:

<pre>$ <b>source ~/tensorflow/bin/activate</b>      # bash, sh, ksh, or zsh
$ <b>source ~/tensorflow/bin/activate.csh</b>  # csh or tcsh</pre>

When the virtualenv environment is active, you may run
TensorFlow programs from this shell.  Your prompt will become
the following to indicate that your tensorflow environment is active:

<pre>(tensorflow)$ </pre>

When you are done using TensorFlow, you may deactivate the
environment by invoking the `deactivate` function as follows:

<pre>(tensorflow)$ <b>deactivate</b> </pre>

The prompt will revert back to your default prompt (as defined by the
`PS1` environment variable).


### Uninstalling TensorFlow

To uninstall TensorFlow, simply remove the tree you created.
For example:

<pre>$ <b>rm -r</b> <i>targetDirectory</i> </pre>


<a name="InstallingNativePip"></a>
## Installing with native pip

You may install TensorFlow through pip, choosing between a simple
installation procedure or a more complex one.

**Note:** The
[REQUIRED_PACKAGES section of setup.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/tools/pip_package/setup.py)
lists the TensorFlow packages that pip will install or upgrade.


### Prerequisite: Python and Pip

Python is automatically installed on Ubuntu.  Take a moment to confirm
(by issuing a `python -V` command) that one of the following Python
versions is already installed on your system:

  * Python 2.7
  * Python 3.3+

The pip or pip3 package manager is *usually* installed on Ubuntu.  Take a
moment to confirm (by issuing a `pip -V` or `pip3 -V` command)
that pip or pip3 is installed.  We strongly recommend version 8.1 or higher
of pip or pip3.  If Version 8.1 or later is not installed, issue the
following command, which will either install or upgrade to the latest
pip version:

<pre>$ <b>sudo apt-get install python-pip python-dev</b>   # for Python 2.7
$ <b>sudo apt-get install python3-pip python3-dev</b> # for Python 3.n
</pre>


### Install TensorFlow

Assuming the prerequisite software is installed on your Linux host,
take the following steps:

  1. Install TensorFlow by invoking **one** of the following commands:

     <pre>$ <b>pip install tensorflow</b>      # Python 2.7; CPU support (no GPU support)
     $ <b>pip3 install tensorflow</b>     # Python 3.n; CPU support (no GPU support)
     $ <b>pip install tensorflow-gpu</b>  # Python 2.7;  GPU support
     $ <b>pip3 install tensorflow-gpu</b> # Python 3.n; GPU support </pre>

     If the preceding command runs to completion, you should now
     [validate your installation](#ValidateYourInstallation).

  2. (Optional.) If Step 1 failed, install the latest version of TensorFlow
     by issuing a command of the following format:

     <pre>$ <b>sudo pip  install --upgrade</b> <i>tfBinaryURL</i>   # Python 2.7
     $ <b>sudo pip3 install --upgrade</b> <i>tfBinaryURL</i>   # Python 3.n </pre>

     where <code><em>tfBinaryURL</em></code> identifies the URL of the
     TensorFlow Python package. The appropriate value of
     <code><em>tfBinaryURL</em></code> depends on the operating system,
     Python version, and GPU support. Find the appropriate value for
     <code><em>tfBinaryURL</em></code>
     [here](#the_url_of_the_tensorflow_python_package).  For example, to
     install TensorFlow for Linux, Python 3.4, and CPU-only support, issue
     the following command:

     <pre>
     $ <b>sudo pip3 install --upgrade \
     https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.3.0-cp34-cp34m-linux_x86_64.whl</b>
     </pre>

     If this step fails, see
     [Common Installation Problems](#common_installation_problems).


### Next Steps

After installing TensorFlow, [validate your installation](#ValidateYourInstallation).


### Uninstalling TensorFlow

To uninstall TensorFlow, issue one of following commands:

<pre>
$ <b>sudo pip uninstall tensorflow</b>  # for Python 2.7
$ <b>sudo pip3 uninstall tensorflow</b> # for Python 3.n
</pre>


<a name="InstallingDocker"></a>
## Installing with Docker

Take the following steps to install TensorFlow through Docker:

  1. Install Docker on your machine as described in the
     [Docker documentation](http://docs.docker.com/engine/installation/).
  2. Optionally, create a Linux group called <code>docker</code> to allow
     launching containers without sudo as described in the
     [Docker documentation](https://docs.docker.com/engine/installation/linux/linux-postinstall/).
     (If you don't do this step, you'll have to use sudo each time
     you invoke Docker.)
  3. To install a version of TensorFlow that supports GPUs, you must first
     install [nvidia-docker](https://github.com/NVIDIA/nvidia-docker), which
     is stored in github.
  4. Launch a Docker container that contains one of the
     [TensorFlow binary images](https://hub.docker.com/r/tensorflow/tensorflow/tags/).

The remainder of this section explains how to launch a Docker container.


### CPU-only

To launch a Docker container with CPU-only support (that is, without
GPU support), enter a command of the following format:

<pre>
$ docker run -it <i>-p hostPort:containerPort TensorFlowCPUImage</i>
</pre>

where:

  * <tt><i>-p hostPort:containerPort</i></tt> is optional.
    If you plan to run TensorFlow programs from the shell, omit this option.
    If you plan to run TensorFlow programs as Jupyter notebooks, set both
    <tt><i>hostPort</i></tt> and <tt><i>containerPort</i></tt>
    to <tt>8888</tt>.  If you'd like to run TensorBoard inside the container,
    add a second `-p` flag, setting both <i>hostPort</i> and <i>containerPort</i>
    to 6006.
  * <tt><i>TensorFlowCPUImage</i></tt> is required. It identifies the Docker
    container. Specify one of the following values:
    * <tt>gcr.io/tensorflow/tensorflow</tt>, which is the TensorFlow CPU binary image.
    * <tt>gcr.io/tensorflow/tensorflow:latest-devel</tt>, which is the latest
      TensorFlow CPU Binary image plus source code.
    * <tt>gcr.io/tensorflow/tensorflow:<i>version</i></tt>, which is the
      specified version (for example, 1.1.0rc1) of TensorFlow CPU binary image.
    * <tt>gcr.io/tensorflow/tensorflow:<i>version</i>-devel</tt>, which is
      the specified version (for example, 1.1.0rc1) of the TensorFlow GPU
      binary image plus source code.

    <tt>gcr.io</tt> is the Google Container Registry. Note that some
    TensorFlow images are also available at
    [dockerhub](https://hub.docker.com/r/tensorflow/tensorflow/).

For example, the following command launches the latest TensorFlow CPU binary image
in a Docker container from which you can run TensorFlow programs in a shell:

<pre>
$ <b>docker run -it gcr.io/tensorflow/tensorflow bash</b>
</pre>

The following command also launches the latest TensorFlow CPU binary image in a
Docker container. However, in this Docker container, you can run TensorFlow
programs in a Jupyter notebook:

<pre>
$ <b>docker run -it -p 8888:8888 gcr.io/tensorflow/tensorflow</b>
</pre>

Docker will download the TensorFlow binary image the first time you launch it.


### GPU support

Prior to installing TensorFlow with GPU support, ensure that your system meets all
[NVIDIA software requirements](#NVIDIARequirements).  To launch a Docker container
with NVidia GPU support, enter a command of the following format:

<pre>
$ <b>nvidia-docker run -it</b> <i>-p hostPort:containerPort TensorFlowGPUImage</i>
</pre>

where:

  * <tt><i>-p hostPort:containerPort</i></tt> is optional. If you plan
    to run TensorFlow programs from the shell, omit this option. If you plan
    to run TensorFlow programs as Jupyter notebooks, set both
    <tt><i>hostPort</i></tt> and <code><em>containerPort</em></code> to `8888`.
  * <i>TensorFlowGPUImage</i> specifies the Docker container. You must
    specify one of the following values:
    * <tt>gcr.io/tensorflow/tensorflow:latest-gpu</tt>, which is the latest
      TensorFlow GPU binary image.
    * <tt>gcr.io/tensorflow/tensorflow:latest-devel-gpu</tt>, which is
      the latest TensorFlow GPU Binary image plus source code.
    * <tt>gcr.io/tensorflow/tensorflow:<i>version</i>-gpu</tt>, which is the
      specified version (for example, 0.12.1) of the TensorFlow GPU
      binary image.
    * <tt>gcr.io/tensorflow/tensorflow:<i>version</i>-devel-gpu</tt>, which is
      the specified version (for example, 0.12.1) of the TensorFlow GPU
      binary image plus source code.

We recommend installing one of the `latest` versions. For example, the
following command launches the latest TensorFlow GPU binary image in a
Docker container from which you can run TensorFlow programs in a shell:

<pre>
$ <b>nvidia-docker run -it gcr.io/tensorflow/tensorflow:latest-gpu bash</b>
</pre>

The following command also launches the latest TensorFlow GPU binary image
in a Docker container. In this Docker container, you can run TensorFlow
programs in a Jupyter notebook:

<pre>
$ <b>nvidia-docker run -it -p 8888:8888 gcr.io/tensorflow/tensorflow:latest-gpu</b>
</pre>

The following command installs an older TensorFlow version (0.12.1):

<pre>
$ <b>nvidia-docker run -it -p 8888:8888 gcr.io/tensorflow/tensorflow:0.12.1-gpu</b>
</pre>

Docker will download the TensorFlow binary image the first time you launch it.
For more details see the
[TensorFlow docker readme](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/tools/docker).


### Next Steps

You should now
[validate your installation](#ValidateYourInstallation).


<a name="InstallingAnaconda"></a>
## Installing with Anaconda

Take the following steps to install TensorFlow in an Anaconda environment:

  1. Follow the instructions on the
     [Anaconda download site](https://www.continuum.io/downloads)
     to download and install Anaconda.

  2. Create a conda environment named <tt>tensorflow</tt> to run a version
     of Python by invoking the following command:

     <pre>$ <b>conda create -n tensorflow</b></pre>

  3. Activate the conda environment by issuing the following command:

     <pre>$ <b>source activate tensorflow</b>
     (tensorflow)$  # Your prompt should change </pre>

  4. Issue a command of the following format to install
     TensorFlow inside your conda environment:

     <pre>(tensorflow)$ <b>pip install --ignore-installed --upgrade</b> <i>tfBinaryURL</i></pre>

     where <code><em>tfBinaryURL</em></code> is the
     [URL of the TensorFlow Python package](#the_url_of_the_tensorflow_python_package).
     For example, the following command installs the CPU-only version of
     TensorFlow for Python 3.4:

     <pre>
     (tensorflow)$ <b>pip install --ignore-installed --upgrade \
     https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.3.0-cp34-cp34m-linux_x86_64.whl</b></pre>


<a name="ValidateYourInstallation"></a>
## Validate your installation

To validate your TensorFlow installation, do the following:

  1. Ensure that your environment is prepared to run TensorFlow programs.
  2. Run a short TensorFlow program.


### Prepare your environment

If you installed on native pip, virtualenv, or Anaconda, then
do the following:

  1. Start a terminal.
  2. If you installed with virtualenv or Anaconda, activate your container.
  3. If you installed TensorFlow source code, navigate to any
     directory *except* one containing TensorFlow source code.

If you installed through Docker, start a Docker container
from which you can run bash. For example:

<pre>
$ <b>docker run -it gcr.io/tensorflow/tensorflow bash</b>
</pre>


### Run a short TensorFlow program

Invoke python from your shell as follows:

<pre>$ <b>python</b></pre>

Enter the following short program inside the python interactive shell:

```python
# Python
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```

If the system outputs the following, then you are ready to begin writing
TensorFlow programs:

<pre>Hello, TensorFlow!</pre>

If you are new to TensorFlow, see @{$get_started/get_started$Getting Started with TensorFlow}.

If the system outputs an error message instead of a greeting, see [Common
installation problems](#common_installation_problems).

## Common installation problems

We are relying on Stack Overflow to document TensorFlow installation problems
and their remedies.  The following table contains links to Stack Overflow
answers for some common installation problems.
If you encounter an error message or other
installation problem not listed in the following table, search for it
on Stack Overflow.  If Stack Overflow doesn't show the error message,
ask a new question about it on Stack Overflow and specify
the `tensorflow` tag.

<table>
<tr> <th>Stack Overflow Link</th> <th>Error Message</th> </tr>

<tr>
  <td><a href="https://stackoverflow.com/q/36159194">36159194</a></td>
  <td><pre>ImportError: libcudart.so.<i>Version</i>: cannot open shared object file:
  No such file or directory</pre></td>
</tr>

<tr>
  <td><a href="https://stackoverflow.com/q/41991101">41991101</a></td>
  <td><pre>ImportError: libcudnn.<i>Version</i>: cannot open shared object file:
  No such file or directory</pre></td>
</tr>

<tr>
  <td><a href="http://stackoverflow.com/q/36371137">36371137</a> and
  <a href="#Protobuf31">here</a></td>
  <td><pre>libprotobuf ERROR google/protobuf/src/google/protobuf/io/coded_stream.cc:207] A
  protocol message was rejected because it was too big (more than 67108864 bytes).
  To increase the limit (or to disable these warnings), see
  CodedInputStream::SetTotalBytesLimit() in google/protobuf/io/coded_stream.h.</pre></td>
</tr>

<tr>
  <td><a href="https://stackoverflow.com/q/35252888">35252888</a></td>
  <td><pre>Error importing tensorflow. Unless you are using bazel, you should
  not try to import tensorflow from its source directory; please exit the
  tensorflow source tree, and relaunch your python interpreter from
  there.</pre></td>
</tr>

<tr>
  <td><a href="https://stackoverflow.com/q/33623453">33623453</a></td>
  <td><pre>IOError: [Errno 2] No such file or directory:
  '/tmp/pip-o6Tpui-build/setup.py'</tt></pre>
</tr>

<tr>
  <td><a href="http://stackoverflow.com/q/42006320">42006320</a></td>
  <td><pre>ImportError: Traceback (most recent call last):
  File ".../tensorflow/core/framework/graph_pb2.py", line 6, in <module>
  from google.protobuf import descriptor as _descriptor
  ImportError: cannot import name 'descriptor'</pre>
  </td>
</tr>

<tr>
  <td><a href="https://stackoverflow.com/questions/35190574">35190574</a> </td>
  <td><pre>SSLError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify
  failed</pre></td>
</tr>

<tr>
  <td><a href="http://stackoverflow.com/q/42009190">42009190</a></td>
  <td><pre>
  Installing collected packages: setuptools, protobuf, wheel, numpy, tensorflow
  Found existing installation: setuptools 1.1.6
  Uninstalling setuptools-1.1.6:
  Exception:
  ...
  [Errno 1] Operation not permitted:
  '/tmp/pip-a1DXRT-uninstall/.../lib/python/_markerlib' </pre></td>
</tr>

<tr>
  <td><a href="http://stackoverflow.com/questions/36933958">36933958</a></td>
  <td><pre>
  ...
  Installing collected packages: setuptools, protobuf, wheel, numpy, tensorflow
  Found existing installation: setuptools 1.1.6
  Uninstalling setuptools-1.1.6:
  Exception:
  ...
  [Errno 1] Operation not permitted:
  '/tmp/pip-a1DXRT-uninstall/System/Library/Frameworks/Python.framework/
   Versions/2.7/Extras/lib/python/_markerlib'</pre>
  </td>
</tr>

</table>


<a name="TF_PYTHON_URL"></a>
## The URL of the TensorFlow Python package

A few installation mechanisms require the URL of the TensorFlow Python package.
The value you specify depends on three factors:

  * operating system
  * Python version
  * CPU only vs. GPU support

This section documents the relevant values for Linux installations.


### Python 2.7

CPU only:

<pre>
https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.3.0-cp27-none-linux_x86_64.whl
</pre>


GPU support:

<pre>
https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.3.0-cp27-none-linux_x86_64.whl
</pre>

Note that GPU support requires the NVIDIA hardware and software described in
[NVIDIA requirements to run TensorFlow with GPU support](#NVIDIARequirements).


### Python 3.4

CPU only:

<pre>
https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.3.0-cp34-cp34m-linux_x86_64.whl
</pre>


GPU support:

<pre>
https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.3.0-cp34-cp34m-linux_x86_64.whl
</pre>

Note that GPU support requires the NVIDIA hardware and software described in
[NVIDIA requirements to run TensorFlow with GPU support](#NVIDIARequirements).


### Python 3.5

CPU only:

<pre>
https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.3.0-cp35-cp35m-linux_x86_64.whl
</pre>


GPU support:

<pre>
https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.3.0-cp35-cp35m-linux_x86_64.whl
</pre>


Note that GPU support requires the NVIDIA hardware and software described in
[NVIDIA requirements to run TensorFlow with GPU support](#NVIDIARequirements).

### Python 3.6

CPU only:

<pre>
https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.3.0-cp36-cp36m-linux_x86_64.whl
</pre>


GPU support:

<pre>
https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.3.0-cp36-cp36m-linux_x86_64.whl
</pre>


Note that GPU support requires the NVIDIA hardware and software described in
[NVIDIA requirements to run TensorFlow with GPU support](#NVIDIARequirements).

<a name="Protobuf31"></a>
## Protobuf pip package 3.1

You can skip this section unless you are seeing problems related
to the protobuf pip package.

**NOTE:** If your TensorFlow programs are running slowly, you might
have a problem related to the protobuf pip package.

The TensorFlow pip package depends on protobuf pip package version 3.1. The
protobuf pip package downloaded from PyPI (when invoking
<tt>pip install protobuf</tt>) is a Python-only library containing
Python implementations of proto serialization/deserialization that can run
**10x-50x slower** than the C++ implementation. Protobuf also supports a
binary extension for the Python package that contains fast
C++ based proto parsing.  This extension is not available in the
standard Python-only pip package.  We have created a custom binary
pip package for protobuf that contains the binary extension. To install
the custom binary protobuf pip package, invoke one of the following commands:

  * for Python 2.7:

  <pre>
  $ <b>pip install --upgrade \
  https://storage.googleapis.com/tensorflow/linux/cpu/protobuf-3.1.0-cp27-none-linux_x86_64.whl</b></pre>

  * for Python 3.5:

  <pre>
  $ <b>pip3 install --upgrade \
  https://storage.googleapis.com/tensorflow/linux/cpu/protobuf-3.1.0-cp35-none-linux_x86_64.whl</b></pre>

Installing this protobuf package will overwrite the existing protobuf package.
Note that the binary pip package already has support for protobufs
larger than 64MB, which should fix errors such as these:

<pre>[libprotobuf ERROR google/protobuf/src/google/protobuf/io/coded_stream.cc:207]
A protocol message was rejected because it was too big (more than 67108864 bytes).
To increase the limit (or to disable these warnings), see
CodedInputStream::SetTotalBytesLimit() in google/protobuf/io/coded_stream.h.</pre>
