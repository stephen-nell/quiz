# openzfs devx


## new debian env
```
su -l
apt install sudo
usermod -aG sudo steve
```

## prep for zfs build
```
sudo apt install \
alien \
autoconf \
automake \
build-essential \
debhelper-compat \
dh-autoreconf \
dh-dkms \
dh-python \
dkms \
fakeroot \
gawk \
git \
libaio-dev \
libattr1-dev \
libblkid-dev \
libcurl4-openssl-dev \
libelf-dev \
libffi-dev \
libpam0g-dev \
libssl-dev \
libtirpc-dev \
libtool \
libudev-dev \
linux-headers-generic \
parallel \
po-debconf \
python3 \
python3-all-dev \
python3-cffi \
python3-dev \
python3-packaging \
python3-setuptools \
python3-sphinx \
uuid-dev \
zlib1g-dev
```

## quiz-history
```
# needed by quiz - but choose right architecture
sudo apt install qemu-system-x86
sudo apt install qemu-system-arm64


# these seem to be needed by quiz
sudo apt install \
git \
mmdebstrap \
linux-headers-$(uname -r) build-essential \
vim \
curl \
libssl-dev \
bc \
libelf-dev \
bison flex \
genext2fs \
rsync




# for me
sudo apt install ripgrep

# clone and prep quiz
cd ~
mkdir dev && cd dev
git clone https://github.com/stephen-nell/quiz.git
cd quiz

# set your kernel version in the config
# but here i just built ___allll___  the kernels
./quiz-prepare-kernel -K
./quiz-prepare-root
./quiz-prepare-work
./quiz-prepare-work -k 5.10.227
```


## and voila -- a zfs dev workflow
```
cd ~/dev
git clone {zfs...}
cd zfs
./autogen.sh
~/dev/quiz/quiz-build-zfs -k 5.10.227 configure --enable-debug --enable-debuginfo
make -j5
~/dev/quiz/quiz-build-zfs make install
cd ~/dev/quiz
./quiz -k 5.10.227  -p zfs,memdev 'zpool create tank quizm0 quizm1 && zpool status && sleep 5'

#or more like this
sudo /home/steve/dev/quiz/quiz -k 5.10.227 -p zfs,memdev zpool create tank quizm0 quizm1 && zpool status && sleep 5 && /bin/bash

```


## references
[robs-workflow](https://despairlabs.com/blog/posts/2024-03-04-quiz-rapid-openzfs-development/#start-to-finish)
