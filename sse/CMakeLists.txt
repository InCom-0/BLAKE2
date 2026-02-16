set(B2_COMPILE_OPTIONS -O3 ${COMMON_C_FLAGS})

add_compile_options(${B2_COMPILE_OPTIONS})

# Build executables with check definition and target for running them if -DCHECK_BUILD=1 is passed
if(BUILD_TESTING)
    include_directories(../testvectors)
endif()

foreach(BLAKE2_TARGET IN LISTS BLAKE2_TARGETS)
    if(BUILD_TESTING)
        add_executable(${BLAKE2_TARGET} "")
        string(TOUPPER "${BLAKE2_TARGET}" BLAKE2_TARGET_UPPER)
        target_compile_definitions(${BLAKE2_TARGET} PRIVATE ${BLAKE2_TARGET_UPPER}_SELFTEST)
    else()
        add_library(${BLAKE2_TARGET} "")
    endif()

    target_include_directories(${BLAKE2_TARGET}
        PUBLIC
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
    )

    target_sources(${BLAKE2_TARGET}
        PRIVATE
        FILE_SET priv_headers
        TYPE HEADERS
        BASE_DIRS
        private_inc)

    target_sources(${BLAKE2_TARGET}
        PUBLIC
        FILE_SET public_headers
        TYPE HEADERS
        BASE_DIRS
        public_inc)
endforeach()


target_sources(blake2b PRIVATE
    blake2b.c
)

target_sources(blake2s PRIVATE
    blake2s.c
)

target_sources(blake2sp PRIVATE
    blake2sp.c
    blake2s.c
)

target_sources(blake2xb PRIVATE
    blake2xb.c
    blake2b.c
)

target_sources(blake2bp PRIVATE
    blake2bp.c
    blake2b.c
)

target_sources(blake2xs PRIVATE
    blake2xs.c
    blake2s.c
)
