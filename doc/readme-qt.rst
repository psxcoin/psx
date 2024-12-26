# NiggaCoin2-qt: Qt5 GUI for NiggaCoin2

## Build Instructions

### Debian

To build NiggaCoin2-qt on Debian or Ubuntu, first install the required Qt5 development packages:

```bash
sudo apt-get install qt5-default qt5-qmake qtbase5-dev-tools qttools5-dev-tools \
    build-essential libboost-dev libboost-system-dev \
    libboost-filesystem-dev libboost-program-options-dev libboost-thread-dev \
    libssl-dev libdb++-dev
```

Then execute:

```bash
qmake
make
```

Alternatively, use Qt Creator to open the `NiggaCoin2-qt.pro` file and build the project. This will create an executable named `NiggaCoin2-qt`.

---

### Windows

Follow these steps to build on Windows:

1. Download and install the [QT Windows SDK](http://qt.nokia.com/downloads/sdk-windows-cpp). Only the desktop Qt is needed.
2. Download and extract the [dependencies archive](https://download.visucore.com/NiggaCoin2/qtgui_deps_1.zip) or compile OpenSSL, Boost, and Berkeley DB yourself.
3. Copy the contents of the `deps` folder into the `X:\QtSDK\mingw` directory, replacing `X:\` with the path to your Qt SDK installation. Ensure the files from `deps\include` are placed in the current `include` directory.
4. Open the `.pro` file in Qt Creator and build (Ctrl + B).

PGP signature for the dependencies archive: [qtgui_deps_1.zip.sig](https://download.visucore.com/NiggaCoin2/qtgui_deps_1.zip.sig). Signed with [RSA key ID 610945D0](http://pgp.mit.edu:11371/pks/lookup?op=get&search=0x610945D0).

---

### macOS

1. Download and install the [Qt macOS SDK](http://qt.nokia.com/downloads/sdk-mac-os-cpp). It's recommended to also install Xcode with UNIX tools.
2. Install [MacPorts](http://www.macports.org/install.php).
3. Install the dependencies using MacPorts:

   ```bash
   sudo port selfupdate
   sudo port install boost db48 miniupnpc
   ```

4. Open the `.pro` file in Qt Creator and build (Cmd + B).

---

## Build Configuration Options

### UPnP Port Forwarding

To enable UPnP for better connectivity, pass the following argument to `qmake`:

```bash
qmake "USE_UPNP=1"
```

In Qt Creator, add this argument under **Projects > Build Settings > Build Steps > Details** (next to **qmake**).  
This requires [miniupnpc](http://miniupnp.tuxfamily.org/files/). UPnP is disabled by default.  

| `USE_UPNP` Value | Description                                   |
|-------------------|-----------------------------------------------|
| `-`              | No UPnP support; miniupnpc not required.      |
| `0`              | Built with UPnP, disabled by default.         |
| `1`              | Built with UPnP, enabled by default.          |

---

### Desktop Notifications for Ubuntu

To enable desktop notifications on (K)Ubuntu 10.04 or newer, use:

```bash
qmake "USE_DBUS=1"
```

---

### QR Code Generation

Enable QR code generation for payment requests with:

```bash
qmake "USE_QRCODE=1"
```

This requires [libqrencode](http://fukuchi.org/works/qrencode/index.html.en).  

| `USE_QRCODE` Value | Description                                |
|---------------------|--------------------------------------------|
| `0`                | No QR code support (default).              |
| `1`                | QR code support enabled.                   |

---

## Notes and Warnings

### Berkeley DB Version Compatibility

Static binaries of NiggaCoin2 are linked against `libdb 5.0`. Berkeley DB databases are **not forward compatible**. If your system has `libdb 5.X` and you run the wallet, the database will upgrade and no longer work with older `libdb` versions.

For more details, refer to [this Debian issue](http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=621425).

---

### Ubuntu 11.10 Qt Issue

Ubuntu 11.10 includes a package called `qt-at-spi`, which can cause NiggaCoin2-qt to crash. The issue is reported as [Launchpad bug 857790](https://bugs.launchpad.net/ubuntu/+source/qt-at-spi/+bug/857790).  

To work around this issue, uninstall `qt-at-spi`:

```bash
sudo apt-get remove qt-at-spi
```

Note: This might disable screen reader functionality for Qt applications.