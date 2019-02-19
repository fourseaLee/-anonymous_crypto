cmake_minimum_required(VERSION 3.0)
project(anonymous)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_C_STANDARD 11)

add_definitions(-DHAVE_CONFIG_H)
include_directories( ./ )

option(ZERO_PARAMS_ACTIVATE  "zero params activate" OFF)
option(SPENDMETA_ACTIVATE  "spend meta activate"  OFF)

if(ZERO_PARAMS_ACTIVATE)
    add_definitions(-DZERO_PARAMS_ACTIVATE)
    message(STATUS "zero param activate: " ${ZERO_PARAMS_ACTIVATE})
else()
    message(STATUS "zero param activate(ZERO_PARAMS_ACTIVATE): " ${ZERO_PARAMS_ACTIVATE})
endif(ZERO_PARAMS_ACTIVATE)

if(SPENDMETA_ACTIVATE)
    add_definitions(-DSPENDMETA_ACTIVATE)
    message(STATUS "spend meta activate: " ${SPENDMETA_ACTIVATE})
else()
    message(STATUS "spend meta activate(SPENDMETA_ACTIVATE): " ${SPENDMETA_ACTIVATE})
endif(SPENDMETA_ACTIVATE)


file(GLOB BASE_SRC 
     *.h  *.cpp   
     )

file(GLOB COMPAT_SRC
       compat/*.h compat/*.cpp
    )

#file(GLOB CONSENSUS_SRC
#        consensus/*.h consensus/*.cpp)

file(GLOB CRYPTO_SRC
       crypto/*.h  crypto/*.cpp
       crypto/ctaes/*.h crypto/ctaes/*.c
    )

#file(GLOB PRIMITIVES_SRC
#        primitives/*.h primitives/*.cpp    
#    )

#file(GLOB SCRIPT_SRC
#        script/*.h script/*.cpp
#    )

file(GLOB SUPPORT_SRC
        support/*.cpp support/*.h 
        support/allocators/*.h
    )

#file(GLOB WALLET_SRC
#        wallet/*.cpp wallet/*.h    
#    )

file(GLOB ZEROCOIN_SRC
	libzerocoin/*.cpp libzerocoin/*.h
	libzerocoin/bitcoin_bignum/*.h
	)

add_library(zerocoin ${BASE_SRC} ${COMPAT_SRC}  ${ZEROCOIN_SRC} ${SUPPORT_SRC})


