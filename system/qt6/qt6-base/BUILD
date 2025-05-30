# This is required
export LANG='en_US.UTF-8'

sedit 's:/bin/ls:/usr/bin/ls:g' src/corelib/CMakeLists.txt util/cmake/tests/data/quoted.pro &&

# CVE-2024-33861
sedit 's:action, &state, nullptr, nullptr:action, state, nullptr, nullptr:g' src/corelib/text/qstringconverter.cpp &&

# This applies here as well; https://github.com/neovim/neovim/issues/2557
CFLAGS="-U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=2"
CXXFLAGS="-U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=2"

# The officially supported CMake generator for building Qt is Ninja/Ninja Multi-Config.
OPTS+=" -DCMAKE_BUILD_TYPE=Release \
        -DFEATURE_optimize_full=ON  \
        -DCMAKE_INSTALL_PREFIX=$MODULE_PREFIX \
        -DINSTALL_BINDIR=qt6/bin \
        -DINSTALL_LIBDIR=qt6 \
        -DINSTALL_LIBEXECDIR=qt6 \
        -DINSTALL_DOCDIR=share/doc/qt6 \
        -DINSTALL_ARCHDATADIR=qt6 \
        -DINSTALL_DATADIR=share/qt6 \
        -DINSTALL_INCLUDEDIR=include/qt6 \
        -DINSTALL_MKSPECSDIR=qt6/mkspecs \
        -DINSTALL_EXAMPLESDIR=share/doc/qt6/examples \
        -DINSTALL_TRANSLATIONSDIR=share/doc/translations \
        -DINSTALL_SYSCONFDIR=/etc/xdg/qt6 \
        -DQT_FEATURE_system_xcb_xinput=ON \
        -DCMAKE_INTERPROCEDURAL_OPTIMIZATION=ON \
        -DFEATURE_libproxy=ON \
        -DCMAKE_MESSAGE_LOG_LEVEL=STATUS \
        -Wno-dev \
        -DQT_UNITY_BUILD=ON \
        -DFEATURE_no_direct_extern_access=ON \
        -DQT_FEATURE_testlib=ON \
        -DBUILD_WITH_PCH=OFF \
        -DFEATURE_mtdev=OFF"

cmake -B $MODULE-$VERSION -S . -G Ninja $OPTS &&
cmake --build $MODULE-$VERSION &&
prepare_install &&

#These installed by qt6-base get overwritten by those from othet qt6-x modules
#So delete these and let the others have their way
rm -f build/qt6/*.prl &&

cmake --install $MODULE-$VERSION &&

# Private hearders
install -d -m755 /usr/include/qt6xcb-private/{gl_integrations,nativepainting} &&

cat > qt6.rc << EOF &&
  export QT6_DIR="${MODULE_PREFIX}"
  export QT6_PLUGIN_PATH="\${QT6_DIR}/qt6/plugins"
  export XDG_DATA_DIRS="\${XDG_DATA_DIRS:-/usr/share}:/usr/share/qt6"
  export PKG_CONFIG_PATH="\${QT6_DIR}/qt6/pkgconfig:\${PKG_CONFIG_PATH}"
  export PATH="\${QT6_DIR}/qt6/bin:\${PATH}"
  export CMAKE_PREFIX_PATH="\${QT6_DIR}/qt6/cmake:\${CMAKE_PREFIX_PATH}"
  export RCC_QT6="\${QT6_DIR}/qt6/bin/rcc"
  export MOC_QT6="\${QT6_DIR}/qt6/bin/moc"
  export UIC_QT6="\${QT6_DIR}/qt6/bin/uic"
  export LANG='en_US.UTF-8'
EOF

install -m644 qt6.rc /etc/profile.d/qt6.rc
