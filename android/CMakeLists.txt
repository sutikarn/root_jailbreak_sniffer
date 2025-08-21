# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

cmake_minimum_required(VERSION 3.4.1)

# ---- เพิ่มธงลิงก์เพื่อรองรับ 16KB page size ----
# ใช้ตัวแปร linker flags เพราะ CMake 3.4.1 ยังไม่มี target_link_options
# ใส่ทั้ง shared/module/exe เผื่อทุกกรณี
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wl,-z,max-page-size=16384 -fuse-ld=lld")
set(CMAKE_MODULE_LINKER_FLAGS "${CMAKE_MODULE_LINKER_FLAGS} -Wl,-z,max-page-size=16384 -fuse-ld=lld")
set(CMAKE_EXE_LINKER_FLAGS    "${CMAKE_EXE_LINKER_FLAGS}    -Wl,-z,max-page-size=16384 -fuse-ld=lld")
# -----------------------------------------------

# (ส่วน ASM/define ถูกคอมเมนต์ไว้ตามไฟล์เดิมของคุณ)
# set(can_use_assembler TRUE)
# enable_language(ASM)
# if(${ANDROID_ABI} STREQUAL "armeabi-v7a" OR ${ANDROID_ABI} STREQUAL "x86")
#     add_definitions("-D_32_BIT")
# elseif(${ANDROID_ABI} STREQUAL "arm64-v8a" OR ${ANDROID_ABI} STREQUAL "x86_64")
#     add_definitions("-D_64_BIT")
# endif()

add_library(
        native-lib
        SHARED
        ../android/src/main/c/native-lib.c
)

find_library(
        log-lib
        log
)

target_link_libraries(
        native-lib
        ${log-lib}
)
