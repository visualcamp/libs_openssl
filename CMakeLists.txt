cmake_minimum_required(VERSION 3.3)
project(openssl)

message("============================================================")
message(">>>                 Configuring OpenSSL                  <<<")

add_library(openssl INTERFACE)

set (OPENSSL_PATH ${CMAKE_CURRENT_SOURCE_DIR})

if (ANDROID)
  set(OPENSSL_LIB_PATH "${OPENSSL_PATH}/lib/android/${CMAKE_ANDROID_ARCH_ABI}/libssl.a"   )
  set(CRYPTO_LIB_PATH  "${OPENSSL_PATH}/lib/android/${CMAKE_ANDROID_ARCH_ABI}/libcrypto.a")

elseif(EMSCRIPTEN)
  set (OPENSSL_LIB_PATH "${OPENSSL_PATH}/lib/emscripten/libssl.a"   )
  set (CRYPTO_LIB_PATH  "${OPENSSL_PATH}/lib/emscripten/libcrypto.a")

elseif(WIN32)
  set (OPENSSL_LIB_PATH "${OPENSSL_PATH}/lib/windows/libssl.lib")
  set (CRYPTO_LIB_PATH  "${OPENSSL_PATH}/lib/windows/libcrypto.lib")

elseif (${CMAKE_SYSTEM_NAME} MATCHES "iOS")
  set (OPENSSL_LIB_PATH "${OPENSSL_PATH}/lib/ios/libssl.a")
  set (CRYPTO_LIB_PATH  "${OPENSSL_PATH}/lib/ios/libcrypto.a")
  
elseif (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  set (OPENSSL_LIB_PATH "${OPENSSL_PATH}/lib/macos/libssl.a")
  set (CRYPTO_LIB_PATH  "${OPENSSL_PATH}/lib/macos/libcrypto.a")

elseif (UNIX)
  find_package(OpenSSL REQUIRED)
  set (OPENSSL_LIB_PATH "")
  set (CRYPTO_LIB_PATH  "")

else()
  message(FATAL_ERROR "Unknown platform")
endif()

set (OPENSSL_INCLUDE_PATH "${OPENSSL_PATH}/include")

message(" - OpenSSL library path: ${OPENSSL_LIB_PATH}")
message(" - Crypto  library path: ${CRYPTO_LIB_PATH}")
message(" - OpenSSL include path: ${OPENSSL_INCLUDE_PATH}")
message(">>>                                                      <<<")
message("============================================================")

target_link_libraries(openssl INTERFACE ${OPENSSL_LIB_PATH} ${CRYPTO_LIB_PATH})
target_include_directories(openssl INTERFACE ${OPENSSL_INCLUDE_PATH})
