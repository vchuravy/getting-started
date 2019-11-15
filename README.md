### Satori Getting Started Documentation for Early Access (EA)

This repo contains the .rst sources for Satori user documentation.

#### Watson Machine Learning Community Edition (WMLCE)

##### [1] Getting Anaconda
To install WMLCE you need to install on your login account [Anaconda for Power 9 / ppc64le architectue](https://www.anaconda.com/distribution/#download-section). 
Download of the Anaconda can be done using `wget`:

```bash
wget https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-ppc64le.sh
```

By default Anaconda will be insalled in your home folder under `anaconda3` and all the WMLCE pachages will be install in a sub-directory on chosen virtual name/folder (ie. anaconda3/envs/wmlce-1.6.2)

##### [2]Setting up the software repository
The WML CE MLDL packages are distributed as conda packages in an online conda repository. conda must be configured to give priority to installing packages from this channel.

Add the WML CE channel to the conda configuration by running the following command:
``` bash
conda config --prepend channels \
https://public.dhe.ibm.com/ibmdl/export/pub/software/server/ibm-ai/conda/
```
##### [3] Creating and activate conda environments (recommended)
With conda, you can create environments that have different versions of Python or packages installed in them. Conda environments are optional but recommended. If not used, packages are installed in the default environment called base, which often has a higher risk of containing conflicting packages or dependencies. Switching between environments is called activating the environment.

The syntax to create and activate a conda environment is:
```bash
conda create --name <environment name> python=<python version>
conda activate <environment name>
```
The only valid Python versions with WML CE are Python 3.6 and 3.7.
For example, to create an environment named wmlce_env with Python 3.6:
```bash
conda create --name wmlce-1.6.2 python=3.6
conda activate wmlce-1.6.2
```
##### [4] Installing all frameworks at the same time
All the MLDL frameworks except RAPIDS packages can be installed at the same time by using the powerai meta-package. All the RAPIDS packages can be installed using the powerai-rapids meta-package.
```bash
conda install powerai
```

Additionaly pachages can be installed with the conda environment activated, runining the following command:
```bash
conda install <package name>
```
For example:
```bash
conda install powerai-rapids
```

NOTE: During the conda install, the packages are downloaded from the internet and after downloading, the license agreement is presented. Read the license agreement and accept the terms and conditions to complete the install. If you decline the license agreement the packages are not installed.
After you finish reading the license agreement, future installations can be automated to silently accept the license agreement by running the following command before running the conda install command:
```bash
export IBM_POWERAI_LICENSE_ACCEPT=yes
```
The license accept has to be done only once on a per user basis.

#### Controlling WMLCE release packages
The conda installer uses a set of rules to determine which packages to install. Channel priorities and package versions are weighted heavily, but the installer also considers factors such as the number of packages that would need to be installed, whether any packages would need to be upgraded or removed, and so on.

The conda installer will sometimes come up with a surprising installation solution. It may prefer to install:
Packages from Anaconda channels over the WML CE channel in spite of channel priorities.
Packages from an older release of WML CE in spite of newer versions being available.
You can guide the conda installer to ensure that it chooses the desired WML CE package using the strict channel priority option and the powerai-release meta-package.

###### a. Strict channel priority

The strict channel priority option forces the conda installer to give additional weight to the priority of channels defined in the configuration. It is useful in cases where the conda installer is preferring packages from lower-priority channels. The simplest use is just to add --strict-channel-priority to the install command:
```bash
conda install --strict-channel-priority tensorflow
```
You can check the priority of the channels in the configuration by running the following:
```bash
conda config --show
...
channel_priority: flexible
channels:
  - https://public.dhe.ibm.com/ibmdl/export/pub/software/server/ibm-ai/conda/
  - defaults
...
```
You could permanently change the channel priority setting to strict:
```bash
conda config --set channel_priority strict
```
###### b. WMLCE release meta-package

The powerai-release meta-package can be used to specify the WML CE release you want to install from. It is useful when the installer prefers packages from an earlier release, or if you intentionally want to install packages from an older release.
Examples:
```bash
(my-wmlce-env) $ conda install pytorch powerai-release=1.6.2
(my-wmlce-env) $ conda install pytorch powerai-release=1.6.1
```
The --strict-channel-priority option can be used with powerai-release for greater control:
```bash
conda install --strict-channel-priority pytorch powerai-release=1.6.2
```
For more information on the conda installer's rules, see Running the solver at: [Understanding and Improving Conda’s performance] https://www.ibm.com/links?url=https%3A%2F%2Fwww.anaconda.com%2Funderstanding-and-improving-condas-performance%2F.

#### Additional conda channels
The main WML CE conda channel is described above. That channel includes the formal, supported WML CE releases.

Additional conda channels are available to complement the main channel. Packages in these channels are not formally supported. Both of these channels are optional. WML CE will install and run fine without either. They can also be used independently of each other (Supplementary does not need Early Access or vice versa). Use them if you want the packages they provide and do not need formal support.

##### The WML CE Supplementary channel is available at: https://anaconda.org/powerai/.
This channel includes packages that are not part of WML CE, but which may be useful to WML CE users. The packages are built from recipes in the WML CE GitHub repository: https://github.com/ibm/powerai.

Problem reports and recipe contributions from the community are welcome. More information about the Supplementary channel can be found in the PowerAI Supplementary Channel README.

##### The WML CE Early Access channel is available at: https://public.dhe.ibm.com/ibmdl/export/pub/software/server/ibm-ai/conda-early-access/.
This channel is updated occasionally with latest versions of various packages included in WML CE. The purpose of the channel is to make new versions of frameworks available in advance of formal WML CE releases. Packages published in the Early Access channel may not exactly match a later WML CE release. For example, package and prerequisite versions may differ.

Packages in the Early Access channel might depend on packages in the main channel, so both channels might be needed in the conda config.
