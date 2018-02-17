# Compilation on Macos

### Dependencies

[OpenModelica](https://github.com/OpenModelica/OpenModelica) is an open-source Modelica-based modeling and simulation environment intended for industrial and academic usage.

Installed extras:
- lpsolve
- omniORB (optional; CORBA is used by OMOptim)
- OpenSceneGraph
- Qt5 or Qt4, Webkit, QtOpenGL
- Sundials (optional; adds more numerical solvers to the simulation runtime)

Extras actually native on Macos:
- Lapack/BLAS (Accelerate Umbrella Framework: https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man7/LAPACK.7.html --> **Accelerate Umbrella Framework**)

Extras to be installed:
- hwloc (optional; queries the number of hardware CPU cores instead of logical CPU cores)
- libhdf5 ("hdf5" on brew: optional part of the MSL tables library supported by few other Modelica tools, so it does not do much)
- libexpat ("expat" on brew: it's actually included in the FMIL sources which are included... but we do not compile those and it's better to use the OS-provided dynamically linked version)
- ncurses, readline (optional, used by OMShell-terminal)

### Compilation from source

Download [OpenModelica](https://github.com/OpenModelica/OpenModelica) from Github, then run the following commands:
```
$ autoconf
$ ./configure CC=clang CXX=clang++
```
This command will end up with the error:
```
checking for qmake in env.vars QMAKE and QTDIR... no
checking for qmake... no
checking for qmake-mac... no
checking for qmake-qt4... no
configure: error: Could not find qmake (QT4)
configure: error: ./configure failed for OMPlot
```
`qmake` is an executable from QT. This formula is keg-only, which means it was not symlinked into /usr/local, because Qt 5 has CMake issues when linked. so we either link QT (`brew link qt`) which is unadvised, or add `qmake` (actually just the bin folder) to the PATH:
```
echo 'export PATH="/usr/local/opt/qt/bin:$PATH"' >> ~/.bash_profile
```
Then relaunch the `./configure` command.

Now build:
```
$ make -j8
```
We then get to the error:
```
configure: Configuration of Ipopt successful
configure: In case of trouble, first consult the troubleshooting page at https://projects.coin-or.org/BuildTools/wiki/user-troubleshooting
configure: Main configuration of Ipopt successful
make[1]: *** [omc-bootstrapped] Error 2
make: *** [omc] Error 2
```

Procedure status:
- [x] autoconf
- [x] ./configure CC=clang CXX=clang++
- [ ] make -j8
- [ ] build/bin/omc --version
- [ ] (cd testsuite/partest && ./runtests.pl)

This approach has been dropped after running into too many issues.



### Other external links

- https://guilhermetorri.wordpress.com/2012/07/09/openmodelica-and-homebrew-on-os-x/
- (old homebrew-science formula) https://github.com/Homebrew/homebrew-science/pull/3296/files/061b373e41b6b4e1d269878442819c47237b0657#diff-f1f609887676784ac12cf5a47bf236e1
- https://openmodelica.org/download/download-mac#homebrew


# Compilation on Linux

We've selected a Linux distribution (Ubuntu 16.04) installed in a virtual machine (VirtualBox). Refer to the wiki Linux_over_virtual_box.md for additional details on the installation of a Linux guest OS over a Macos using the virtualizer VirtualBox.

### Dependencies

How to install OpenModelica Compiler dependencies:
https://github.com/OpenModelica/OMCompiler/blob/master/README.Linux.md
https://www.openmodelica.org/download/download-linux

For getting the dependencies, we went for the Nightly builds, which should hev the more recent updates. It is recommended to build OpenModelica sources downloaded directly from the Github repository.

- [x] echo deb http://build.openmodelica.org/apt precise nightly | sudo tee -a /etc/apt/sources.list <sup id="p1">[1](#f1)</sup>
- [x] echo deb-src http://build.openmodelica.org/apt precise nightly | sudo tee -a /etc/apt/sources.list
- [x] sudo apt-get update
- [x] sudo apt-get build-dep openmodelica
- [x] git clone --recursive https://openmodelica.org/git-readonly/OpenModelica.git OpenModelica
- [x] cd OpenModelica
- [x] autoconf
- [x] ./configure --prefix=<installFolder> --with-omniORB
- [x] make -j4
- [ ] sudo make install



# Alternative software

http://www.jmodelica.org/


<b id="f1">1</b>: HowTo sources list with deb packages:
https://wiki.debian.org/SourcesList

deb packages are used by a majority of distributions like Debian and Ubuntu. [↩](#p1)
