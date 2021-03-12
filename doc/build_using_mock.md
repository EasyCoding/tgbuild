# Build using mock
## Prepare build environment
### Step 1

Clone this repository with SPECs and patches to any directory:
```bash
git clone -b master https://github.com/xvitaly/tgbuild.git tgbuild
```

### Step 2

Install mock, spectool and rpmbuild:
```bash
sudo dnf install rpm-build rpmdevtools mock mock-rpmfusion-free
```

Add yourself to `mock` group (you must run this only for the first time after installing mock):
```bash
sudo usermod -a -G mock $(whoami)
```
You need to relogin to your system after doing this or run:
```bash
newgrp mock
```

### Step 3

Create RPM build base directories:
```bash
rpmdev-setuptree
```

## Download sources and patches
### Step 1

Download sources:
```bash
cd tgbuild
spectool -g -R libtgvoip.spec
spectool -g -R telegram-desktop.spec
```

### Step 2

Copy patches to sources directory:
```bash
cd tgbuild
cp -f *.patch $(rpm --eval %{_sourcedir})
```

## Build required VoIP library
### Step 1

Generate SRPM package for mock:
```bash
rpmbuild -bs libtgvoip.spec
```

### Step 2

Start mock build sequence:
```bash
mock -r fedora-$(rpm -E %fedora)-$(uname -m)-rpmfusion_free --rebuild ~/rpmbuild/SRPMS/libtgvoip*.src.rpm
```

### Step 3

Wait for a while and then install result into chroot:
```bash
mock -r fedora-$(rpm -E %fedora)-$(uname -m)-rpmfusion_free --install /var/lib/mock/*/result/libtgvoip*.$(uname -m).rpm
```

## Build Telegram Desktop package
### Step 1

Generate SRPM package for mock:
```bash
rpmbuild -bs telegram-desktop.spec
```

### Step 2

Start mock build sequence without chroot cleanup:
```bash
mock -r fedora-$(rpm -E %fedora)-$(uname -m)-rpmfusion_free --rebuild --no-clean ~/rpmbuild/SRPMS/telegram-desktop*.src.rpm
```

### Step 3

Wait for a while and then install result without debug subpackages:
```bash
sudo dnf install /var/lib/mock/*/result/*.$(uname -m).rpm --exclude="*debug*"
```

## Cleanup

Remove temporary files from `~/rpmbuild`, `/var/cache/mock`, `/var/lib/mock` directories.
