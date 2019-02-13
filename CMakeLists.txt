cmake_minimum_required(VERSION 3.0)
project(anonymous)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_C_STANDARD 11)
add_definitions(-DBCN -DXMR)
option(BCN "Build the library of crpto from  bcn" ON)
option(BETTER_DEBUG "Disables optimizations. We do not use standard debug/realease configurations because they change too much" OFF)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -g -Wall -Wextra -Werror=return-type -Wno-unused-parameter")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -g -Wall -Wextra -Werror=return-type -Wno-unused-parameter")

if(BETTER_DEBUG)
	message(STATUS "Using better debug: " ${BETTER_DEBUG})
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O0")
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O0")
else()
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2")
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2")
endif(BETTER_DEBUG)


if(BCN)
	get_filename_component(PARENT_DIR ${CMAKE_SOURCE_DIR} DIRECTORY)
	if(CMAKE_SIZEOF_VOID_P EQUAL 8)
		set(OPENSSL_ROOT ${PARENT_DIR}/openssl)
	else()
		set(OPENSSL_ROOT ${PARENT_DIR}/openssl32)
	endif()

	set(LINK_OPENSSL ssl crypto)
	add_definitions(-Dplatform_USE_SSL=1)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -g -Wall -Wextra -Werror=return-type -Wno-unused-parameter")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -g -Wall -Wextra -Werror=return-type -Wno-unused-parameter")



	file(GLOB BCN_CRYPTO_SRC
		bcn_crypto/*.cpp bcn_crypto/*.hpp
		bcn_crypto/*.c bcn_crypto/*.h
		bcn_crypto/bernstein/*.h bcn_crypto/bernstein/*.c
		bcn_crypto/blake/*.h bcn_crypto/blake/*.c
		bcn_crypto/groestl/*.h bcn_crypto/groestl/*.c
		bcn_crypto/jh/*.h bcn_crypto/jh/*.c
		bcn_crypto/keccak/*.c
		bcn_crypto/oaes/*.h bcn_crypto/oaes/*.c
		bcn_crypto/skein/*.h bcn_crypto/skein/*.c
		)

	set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/bin")
	set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/libs")
	add_library(bcn-crypto ${BCN_CRYPTO_SRC})

endif(BCN)

