![](https://armorpaint.org/img/git.jpg)

armorpaint
==============

[ArmorPaint](https://armorpaint.org) is a software for 3D PBR texture painting - check out the [manual](https://armorpaint.org/manual).

*Note 1: This repository is aimed at developers and may not be stable. Distributed binaries are currently [paid](https://armorpaint.org/download) to help with the project funding. All of the development is happening here in order to make it accessible to everyone. Thank you for support!*

*Note 2: If you are compiling git version of ArmorPaint, then you need to have a compiler ([Visual Studio](https://visualstudio.microsoft.com/downloads/) - Windows, [clang](https://clang.llvm.org/get_started.html) + [deps](https://github.com/Kode/Kha/wiki/Linux) - Linux, [Xcode](https://developer.apple.com/xcode/resources/) - macOS / iOS, [Android Studio](https://developer.android.com/studio) - Android), [nodejs](https://nodejs.org/en/download/) and [git](https://git-scm.com/downloads) installed. Learn more about [Kha](https://github.com/Kode/Kha/wiki), [Kinc](https://github.com/Kode/Kinc/wiki) and [Krom](https://github.com/Kode/Krom/blob/master/readme.md).*
```bash
git clone --recursive https://github.com/armory3d/armorpaint
cd armorpaint
```
```bash
# Windows
node armorcore/make -g direct3d11
cd armorcore
# Unpack `v8\libraries\win32\release\v8_monolith.7z` using 7-Zip - Extract Here (exceeds 100MB)
git apply patch/window_handling.diff --directory=Kinc
node Kinc/make -g direct3d11
# Open generated Visual Studio project
# Set `Project - Properties - Debugging - Command Arguments` to `..\..\build\krom`
# Build for x64 & release
```
```bash
# Linux
node armorcore/make -g opengl
cd armorcore
node Kinc/make -g opengl --compiler clang --compile
cd Deployment
strip Krom
./Krom ../../build/krom
```
```bash
# macOS
node armorcore/make -g metal
cp -a build/krom/ armorcore/Deployment
cd armorcore
node Kinc/make -g metal
# Open generated Xcode project
# Add `path/to/armorpaint/armorcore/v8/libraries/macos/release` into `Project - Krom - Build Settings - Search Paths - Library Search Paths`
# Build
```
```bash
# Android - wip
node armorcore/make android -g opengl --shaderversion 300
cp -r build/krom/ armorcore/build/Krom/app/src/main/assets/
cd armorcore
node Kinc/make android -g opengl
# Manual tweaking is required for now:
# https://github.com/armory3d/armorcore/blob/master/kincfile.js#L68
# Open generated Android Studio project
# Build for device
```
```bash
# iOS - wip
node armorcore/make ios -g metal
cp -a build/krom/ armorcore/Deployment
cd armorcore
git apply patch/ios_document_picker.diff --directory=Kinc
node Kinc/make ios -g metal
# Open generated Xcode project
# Add `path/to/armorcore/v8/libraries/ios/release` into `Project - Krom - Build Settings - Search Paths - Library Search Paths`
# Build for device
```
```bash
# Windows DXR - wip
node armorcore/make -g direct3d12
cd armorcore
# Unpack `v8\libraries\win32\release\v8_monolith.7z` using 7-Zip - Extract Here (exceeds 100MB)
git apply patch/window_handling.diff --directory=Kinc
git apply patch/d3d12_raytrace.diff --directory=Kinc
git apply patch/d3d12_wrap_sampler.diff --directory=Kinc
node Kinc/make -g direct3d12 --raytrace dxr
# Open generated Visual Studio project
# Set `Project - Properties - Debugging - Command Arguments` to `..\..\build\krom`
# Build for x64 & release
```
```bash
# Linux VKRT - wip
node armorcore/make -g vulkan
cd armorcore
git apply patch/vulkan_raytrace.diff --directory=Kinc
git clone --recursive https://github.com/Kode/krafix Libraries/krafix
node Kinc/make -g vulkan --raytrace vkrt --compiler clang --compile
cd Deployment
strip Krom
./Krom ../../build/krom
```
```bash
# Windows VR - wip
node armorcore/make -g direct3d11 --vr oculus
cd armorcore
# Unpack `v8\libraries\win32\release\v8_monolith.7z` using 7-Zip - Extract Here (exceeds 100MB)
git apply patch/window_handling.diff --directory=Kinc
node Kinc/make -g direct3d11 --vr oculus
# Open generated Visual Studio project
# Set `Project - Properties - Debugging - Command Arguments` to `..\..\build\krom`
# Build for x64 & release
```
```bash
# Updating cloned repository
git pull origin master
git submodule update --init --recursive
```
```bash
# How to generate or update a locale file
pip install typing_extensions -t Assets/locale/tools
python ./Assets/locale/tools/extract_locales.py <locale code>
# Generated or updated in `Assets/locale/<locale code>.json`
```
