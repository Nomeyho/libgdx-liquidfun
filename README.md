# libgdx-liquidfun

An integration of [Liquidfun](https://github.com/google/liquidfun) to [Libgdx](https://github.com/libgdx/libgdx).
Based on previous integrations:
* [gdx-liquidfun](https://github.com/Thommil/gdx-liquidfun)
* [gdx-liquidfun-extension](https://github.com/finnstr/gdx-liquidfun-extension)

Fluid simulations are CPU-intensive operations. This is why Liquidfun is implemented in C++. The JNI is used to make the link between Java and C++. A few setup steps are therefore required before being able to use Liquidfun inside your Libgdx project.

## Installation
Adapted from [here](https://github.com/finnstr/gdx-liquidfun-extension/wiki/Setup)

These instructions only consider the desktop (Linux 32 bits & 64 bits) and android platforms.

Please refer to the [build](#build) section for other platforms.

### Step1: removing Box2D
This extension already includes all the code for Box2D. So if you are currently using Box2D in your project you have to remove it to keep everything clean. If you're not using Box2D you can skip this and continue with the second step.

To remove the Box2D dependecies you have to open up the `build.gradle` file in your root directory. Here are the dependencies you have to remove:

**Core Dependency:**
```groovy
compile "com.badlogicgames.gdx:gdx-box2d:$gdxVersion"
```
**Desktop Dependency:**
```groovy
compile "com.badlogicgames.gdx:gdx-box2d-platform:$gdxVersion:natives-desktop"
```
**Android Dependency:**
```groovy
compile "com.badlogicgames.gdx:gdx-box2d:$gdxVersion"
natives "com.badlogicgames.gdx:gdx-box2d-platform:$gdxVersion:natives-armeabi"
natives "com.badlogicgames.gdx:gdx-box2d-platform:$gdxVersion:natives-armeabi-v7a"
natives "com.badlogicgames.gdx:gdx-box2d-platform:$gdxVersion:natives-x86"
```

### Step 2: Adding new dependecies
Now you have to add the new dependencies for the new files. Those are the lines you have to add:

**Core Dependency:**
```groovy
compile fileTree(dir: 'libs', include: '*.jar')
```
**Desktop Dependency:**
```groovy
compile fileTree(dir: '../core/libs', include: '*.jar')
compile fileTree(dir: 'libs', include: '*.so')
```
**Android Dependency:**
```groovy
compile fileTree(dir: '../core/libs', include: '*.jar')
compile fileTree(dir: 'libs', include: '*.so')
```

### Step 3: Adding the files
Finaly, you need to add the actual files. You can find them inside the `libs` folder of this repository.
Place the files as follows (create the `libs` folders if needed):

`gdx-liquidfun.jar` into `core/libs`

`gdx-liquidfun-natives.jar` into `desktop/libs`

`libgdx-liquidfun.so` from `libs/arm64-v8a` into `android/libs/arm64-v8a`

`libgdx-liquidfun.so` from `libs/armeabi` into `android/libs/armeabi`

`libgdx-liquidfun.so` from `libs/armeabi-v7a` into `android/libs/armeabi-v7a`

`libgdx-liquidfun.so` from `libs/x86` into `android/libs/x86`

`libgdx-liquidfun.so` from `libs/x86_64` into `android/libs/x86_64`

NB: you can also remove the `libgdx-box2d.so` files from those directories

### Step 4: Testing
Rebuild your project and try the code: `demo/Demo.java`

## More platforms
### HTML
It is not possible to use this extension in an HTML project, because of the native code (C++).

### iOS
You have to generate the `.a` file, see below

### Windows 
You have to generate the `.ddl` file, see below

## Build
The extensions files of the `libs` folder can be built from the sources. This allows you to build this extension for other platforms (e.g. iOS and Windows). To do so, you need to use [ant](http://ant.apache.org/) to generate the `libs` files from the `jni` sources. Here is how to proceed:
* Go to the `jni` folder
* Configure `Application.mk` (specify the supported Android architectures by listing them on the first line)
* Configure `build.xml` (add/remove platform targets)
* Configure the individual `builder-*.xml` files. For instance, set the path of the Android NDK in `builder-android32.xml`. If you have installed the NDK from Android studio, the path should look like `~/Android/Sdk/ndk-bundle`
* Run the command `ant`

The extensions files are generated in the `libs` folder.




