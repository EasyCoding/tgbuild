# Build Telegram Desktop RPM package from sources
This will build telegram-desktop RPM package from sources. Just follow instructions or just enable repository and install package pre-built from this sources.

# Build package from sources manually
Select one of these ways:
 * [build using mock](doc/build_using_mock.md) (recommended);
 * [build using rpmbuild](doc/build_using_rpmbuild.md).

# Install pre-built from this sources packages
You can also install pre-built from this sources [package](http://koji.rpmfusion.org/koji/packageinfo?packageID=492) from RPMFusion repository.

Add RPMFusion repository:
```bash
sudo dnf install --nogpgcheck https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

Now install Telegram Desktop:
```bash
sudo dnf install telegram-desktop
```
