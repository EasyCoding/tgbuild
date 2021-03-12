# Build using rpmbuild
## Prepare build environment
### Step 1

Clone this repository with SPECs and patches to any directory:
```bash
git clone -b master https://github.com/xvitaly/tgbuild.git tgbuild
```

### Step 2

Install spectool and rpmbuild:
```bash
sudo dnf install rpm-build rpmdevtools
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

Copy patches and other files to sources directory:
```bash
cd tgbuild
cp -f *.patch $(rpm --eval %{_sourcedir})
```

## Build required VoIP library
### Step 1

Install build-requirements:
```bash
cd tgbuild
sudo dnf builddep libtgvoip.spec --best --allowerasing
```

### Step 2

Build RPM package:
```bash
rpmbuild -ba libtgvoip.spec
```

### Step 3

Wait for a while and then install result without debug subpackages:
```bash
sudo dnf install ~/rpmbuild/RPMS/$(uname -m)/libtgvoip*.rpm --exclude="*debug*" --best --allowerasing
```

## Build Telegram Desktop package
### Step 1

Install build-requirements:
```bash
cd tgbuild
sudo dnf builddep telegram-desktop.spec
```

### Step 2

Build RPM package:
```bash
rpmbuild -ba telegram-desktop.spec
```

### Step 3

Wait for a while and then install result without debug subpackages:
```bash
sudo dnf install ~/rpmbuild/RPMS/$(uname -m)/telegram-desktop*.rpm  --exclude="*debug*"  --best --allowerasing
```

## Cleanup

Now you can remove all files from `~/rpmbuild` directory.
