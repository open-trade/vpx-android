## Build libvpx for Android
####
<table>
<thead>
<tr><td>library</td><td>version</td><td>platform support</td><td>arch support</td></tr>
</thead>
<tr><td>libvpx</td><td>1.8.0</td><td>android</td><td>armeabi-v7a arm64-v8a x86 x86_64</td></tr>
</table>

### Build For Android
- Follow the instructions below to build libvpx for android
- aTalk v1.8.2 release is compatible with libvpx-1.8.0 or 1.9.0 (./source/libvpx-master-20190420.zip)<br/>
- Following problem has been fixed with inclusion of configure option --disable-avx2<br/>
  see https://bugs.chromium.org/p/webm/issues/detail?id=1623#c1<br/>
  i.e.: The compiled libjnvpx.so for aTalk has problem when exec on x86_64 android platform (libvpx asm source errors):<br/>
  org.atalk.android A/libc: Fatal signal 31 (SIGSYS), code 1 in tid 5833 (Loop thread: ne), pid 4781 (g.atalk.android)
- When you first exec build-libvpx4android.sh, it applies the required patches to libvpx<br/>
  Note: the patches defined in libvpx_patch.sh is for libvpx-1.8.0+, libvpx-1.7.0 and libvpx-1.6.1+<br/>
  
The ./build-libvpx4android.sh script build the static libvpx.a for the various architectures as defined in ./_settings.sh<br/>
i.e. ABIS=("armeabi-v7a" "arm64-v8a" "x86" "x86_64")<br/>
All the built libvpx.a and *.h are installed in the ./output/android/\<ABI>/lib and ./output/android/\<ABI>/include respectively

**Android libvpx build instructions:**
```
## git clone vpx-android directory into your linux working directory.
git clone https://github.com/cmeng-git/vpx-android.git ./vpx-android
cd vpx-android

## Use Android NDK: android-ndk-r15c
export ANDROID_NDK=/opt/android/android-ndk-r15c

## setup the required libvpx; default "libvpx-1.8.0" or change LIB_GIT in ./init_libvpx.sh
./init_libvpx.sh

## use one of the following to build libvpx i.e.
#a. for all the ABI's defined in _settings.sh
./build-libvpx4android.sh

#b. for a specific <ABI>
./build-libvpx4android.sh <ABI> 
```

Copy the static libs and includes in `./output/android/<ABI>` directories to the android project
jni directory i.e. aTalk/jni/vpx/android/\<ABI>

Note: All information given below is for reference only. See aTalk/jni for its implementation.

##### ============================================
#### Android.mk makefile - for other project
Add libvpx include path to `jni/Android.mk`. 

```
#Android.mk

include $(CLEAR_VARS)
LOCAL_MODULE := libvpx
LOCAL_SRC_FILES := Your libVPX Library Path/$(TARGET_ARCH_ABI)/libvpx.a
include $(PREBUILT_STATIC_LIBRARY)

LOCAL_C_INCLUDES := $(LOCAL_PATH)/Your libvpx Include Path/libvpx
LOCAL_STATIC_LIBRARIES := libvpx
LOCAL_LDLIBS := -lz
	
```

License
-------

    ffmpeg, android static library for aTalk VoIP and Instant Messaging client
    
    Copyright 2016 Eng Chong Meng
        
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at
    
       http://www.apache.org/licenses/LICENSE-2.0
    
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.




