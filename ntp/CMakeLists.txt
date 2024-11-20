UNSET(LOCAL_NTP_SRC_DIR CACHE)

SET(NTP_SRC_NAME "ntp_src-src")

FetchContent_Declare(
  ntp_src
  URL      https://downloads.nwtime.org/ntp/4.2.8/ntp-4.2.8p16.tar.gz
  URL_HASH MD5=6a66099e2c9a89957da27527e3527ddb
)

FetchContent_MakeAvailable(ntp_src)

FIND_FILE(LOCAL_NTP_SRC_DIR
       NAMES ${NTP_SRC_NAME}
       PATHS ${CMAKE_BINARY_DIR}/_deps/
       NO_DEFAULT_PATH
)

if(NOT LOCAL_NTP_SRC_DIR)
    message(FATAL_ERROR "No ${LOCAL_NTP_SRC_DIR}")
endif ()

add_custom_target(ntp)

add_custom_command(
       TARGET ntp
       COMMAND ./configure --prefix=${CMAKE_INSTALL_PREFIX}/ntp --exec-prefix=${CMAKE_INSTALL_PREFIX}/ntp 
       --host=${CMAKE_TOOLCHIANS_PREFIX}
       --with-pic=sntp
       --with-pic=libevent
       --with-yielding-select=yes 
       --with-openssl-incdir=${CMAKE_INSTALL_PREFIX}/openssl/include
       --with-openssl-libdir=${CMAKE_INSTALL_PREFIX}/openssl/
       --with-threads=no
       --without-sntp
       --with-ntpsnmpd=no &&
       make -j16 && make install
       WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/_deps/${NTP_SRC_NAME}
       COMMENT "build ntp"
       VERBATIM
)

add_dependencies(ntp libopenssl)
