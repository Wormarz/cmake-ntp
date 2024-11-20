UNSET(LOCAL_OPENSSL_LIB_DIR CACHE)

SET(OPENSSL_LIB_NAME "openssl-src")

FetchContent_Declare(
  openssl
  URL      https://github.com/openssl/openssl/releases/download/OpenSSL_1_0_2u/openssl-1.0.2u.tar.gz
  URL_HASH SHA256=ecd0c6ffb493dd06707d38b14bb4d8c2288bb7033735606569d8f90f89669d16
)

FetchContent_MakeAvailable(openssl)

FIND_FILE(LOCAL_OPENSSL_LIB_DIR
        NAMES ${OPENSSL_LIB_NAME}
        PATHS ${CMAKE_BINARY_DIR}/_deps/
        NO_DEFAULT_PATH
)

if(NOT LOCAL_OPENSSL_LIB_DIR)
    message(FATAL_ERROR "OpenSSL not found")
endif()

add_custom_target(libopenssl)

add_custom_command(
       TARGET libopenssl
       COMMAND ./Configure --prefix=${CMAKE_INSTALL_PREFIX}/openssl 
       --openssldir=${CMAKE_INSTALL_PREFIX}/openssl 
       no-shared --cross-compile-prefix=${CMAKE_TOOLCHIANS_PREFIX}- ${OPENSSL_ARCH_OPT} &&
       make -j16 && make install
       WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/_deps/${OPENSSL_LIB_NAME}
       COMMENT "build openssl"
       VERBATIM
)