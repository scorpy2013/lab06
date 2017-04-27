## Laboratory work VIII

Данная лабораторная работа посвещена изучению средств пакетирования на примере **CPack**

```bash
$ open https://cmake.org/Wiki/CMake:CPackPackageGenerators
```

## Tasks

- [ ] 1. Создать публичный репозиторий с названием **lab8** на сервисе **GitHub**
- [ ] 2. Выполнить инструкцию учебного материала
- [ ] 3. Ознакомиться со ссылками учебного материала

## Tutorial

```bash
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_EMAIL=<адрес_почтового_ящика>
```

```bash
$ git clone https://github.com/${GITHUB_USERNAME}/lab7 lab8
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab8
$ cd lab8
```

```bash
$ sed '/project(print)/a set(PRINT_VERSION_STRING "v${PRINT_VERSION}")'  CMakeLists.txt
$ sed '/project(print)/a set(PRINT_VERSION \
${PRINT_VERSION_MAJOR}.${PRINT_VERSION_MINOR}.${PRINT_VERSION_PATCH}.${PRINT_VERSION_TWEAK})' \ 
CMakeLists.txt
$ sed '/project(print)/a set(PRINT_TWEAK_VERSION 0)' CMakeLists.txt
$ sed '/project(print)/a set(PRINT_PATCH_VERSION 0)' CMakeLists.txt
$ sed '/project(print)/a set(PRINT_MINOR_VERSION 1)' CMakeLists.txt
$ sed '/project(print)/a set(PRINT_MAJOR_VERSION 1)' CMakeLists.txt
$ sed '/project(print)/a )' CMakeLists.txt
```

```bash
$ touch DESCRIPTION && nano DESCRIPTION
$ touch ChangeLog.md
$ DATE=`date` cat > ChangeLog.md <<EOF
* ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0
- Initial RPM release
EOF
```

```bash
$ cat > CPackConfig.cmake <<EOF
include(InstallRequiredSystemLibraries)
```

```bash
$ cat >> CPackConfig.cmake <<EOF
set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
set(CPACK_PACKAGE_VERSION_MAJOR ${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK ${PRINT_VERSION_TWEAK)
set(CPACK_PACKAGE_VERSION ${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE ${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static c++ library for printing")
EOF
```

```bash
$ cat >> CPackConfig.cmake <<EOF
set(CPACK_RESOURCE_FILE_LICENSE ${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README ${CMAKE_CURRENT_SOURCE_DIR}/README.md)
EOF
```

```bash
$ cat >> CPackConfig.cmake <<EOF
set(CPACK_RPM_PACKAGE_NAME "PRINT-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "PRINT")
set(CPACK_RPM_PACKAGE_URL "https://github.com/${GITHUB_USERNAME}/lab7")
set(CPACK_RPM_CHANGELOG_FILE ${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)
```

```bash
$ cat >> CPackConfig.cmake <<EOF
set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
set(CPACK_DEBIAN_PACKAGE_HOMEPAGE "https://${GITHUB_USERNAME}.github.io/lab7")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
```

```bash
$ cmake -H. -B_build
$ cmake --build _build
$ cd _build
$ cpack -G "TGZ"
$ cpack -G "RPM"
$ cpack -G "DEB"
$ cpack -G "NSIS"
$ cpack -G "DragNDrop"
$ cd ..
```

```bash
$ mkdir artifacts
$ mv _build/*.tgz artifacts
```

```bash
$ cat >> CPackConfig.cmake <<EOF
include(CPack)
EOF
```

```bash
$ git add .
$ git commit -m"added cpack config"
$ git push origin master
```
## Links

- [DMG](https://cmake.org/cmake/help/latest/module/CPackDMG.html)
- [DEB](https://cmake.org/cmake/help/latest/module/CPackDeb.html)
- [RPM](https://cmake.org/cmake/help/latest/module/CPackRPM.html)
- [NSIS](https://cmake.org/cmake/help/latest/module/CPackNSIS.html)