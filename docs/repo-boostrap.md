## History of this repository

To be able to focus on the maintenance of ksh93, the sources in this repository
got pulled out of the **https://github.org/att/ast** repository
(**ast-open-archive** is a clone of it). Its master branch contains the last
stable ksh version (93u+ or 2012-08-01) released by the main AT&T Software
Technology authors, while they were at AT&T (plus some minor fixes and
documentation added in 2020).


You may reproduce the bootstrapping of this repo, using the following steps:

```
git clone https://github.com/att/ast.git
cd ast
unset PROTO
PATH=${PATH}:${PWD}/arch/`bin/package`/bin
# build nmake and pax incl. dependencies. E.g. on Solaris 11 with SunStudio:
bin/package make ksh \
    SHELL=/bin/bash CC=cc CCFLAGS="-xc99 -D_XPG6 -m64" LDFLAGS="-m64"
# we need a little helper script stored in this repo
git clone https://github.com/jelmd/ksh-ast.git
# create INIT.2012-08-01.txz and ast-ksh.2012-08-01.tgz
ksh-ast/etc/mk-ksh-archives.sh ${PWD}

mkdir ../ksh-ast && cd ../ksh-ast
xz -dc ../ast/INIT.2012-08-01.txz | tar xvf -

# the clean way:
bin/package make
mkdir lib/package/tgz && cp ../ast/ast-ksh.2012-08-01.tgz lib/package/tgz
bin/package read
bin/package clean INIT
rm -rf lib/package/tgz arch = bin/execrate* lib/package/CONVERT.mk

# not so clean but should work, too:
#gunzip -c ../ast/ast-ksh.2012-08-01.tgz | tar xvf -

cp -pr ../ast/docs ../ast/LICENSE.md .
```
