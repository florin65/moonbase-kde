source /etc/profile.d/qt6.rc &&

#Generating .rcc/qmlcache/qtgraphicaleffectsplugin_ColorOverlay_qml.cpp
#Detected locale "en_US" with character encoding "ISO-8859-1", which is not UTF-8.
#Qt depends on a UTF-8 locale, and has switched to "en_US.UTF-8" instead.
export LANG='en_US.UTF-8'

# This applies here as well; https://github.com/neovim/neovim/issues/2557
CFLAGS="-U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=2"
CXXFLAGS="-U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=2"

OPTS+=" -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=$MODULE_PREFIX \
        -DINSTALL_LIBEXECDIR=qt6 \
        -DINSTALL_LIBDIR=qt6 \
        -DINSTALL_DOCDIR=share/doc/qt6 \
        -DINSTALL_ARCHDATADIR=qt6 \
        -DINSTALL_DATADIR=share/qt6 \
        -DINSTALL_INCLUDEDIR=include/qt6 \
        -DINSTALL_MKSPECSDIR=qt6/mkspecs \
        -DINSTALL_EXAMPLESDIR=share/doc/qt6/examples \
        -DINSTALL_SYSCONFDIR=/etc/xdg \
        -DCMAKE_MESSAGE_LOG_LEVEL=STATUS \
        -DQT_NO_PACKAGE_VERSION_CHECK=TRUE \
        -Wno-dev"

cmake -B build -S . -G Ninja $OPTS &&
cmake --build build --target docs &&
prepare_install &&

prepare_install &&
cmake --build build --target install_docs
