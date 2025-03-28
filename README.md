# Sqlite3 amalgamation

Mirror of sqlite3 amalgamation, but also provide CMake scripts to build library so that it can be used in other CMake-based projects.

```shell
mkdir build
cd build
cmake .. -G "Visual Studio 17 2022" -DCMAKE_INSTALL_PREFIX=C:\Users\SonnyCalcr\EDisk\CppCodes\Libraries\AllLibs\
cmake --build . --config Debug --target install
```
