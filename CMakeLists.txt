cmake_minimum_required(VERSION 2.8)

project(seq2mid C CXX)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(MSVC)
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)

    option(STATIC_CRT "Use static CRT libraries" OFF)

    # Rewrite command line flags to use /MT if necessary
    if(STATIC_CRT)
        foreach(flag_var
                CMAKE_CXX_FLAGS CMAKE_CXX_FLAGS_DEBUG CMAKE_CXX_FLAGS_RELEASE
                CMAKE_CXX_FLAGS_MINSIZEREL CMAKE_CXX_FLAGS_RELWITHDEBINFO)
            if(${flag_var} MATCHES "/MD")
                string(REGEX REPLACE "/MD" "/MT" ${flag_var} "${${flag_var}}")
            endif(${flag_var} MATCHES "/MD")
        endforeach(flag_var)
    endif()

    # Allow for wildcards in command-line path arguments
    # http://msdn.microsoft.com/en-us/library/8bch7bkk.aspx
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} setargv.obj")
endif()


#============================================================================
# seq2mid
#============================================================================

set(SEQ2MID_SRCS
    src/common/cutils.h
)

set(SEQ2MID_HDRS
    src/common/cioutils.h
    src/common/cutils.c
    src/seq2mid/seq2mid.c
)

add_executable(seq2mid ${SEQ2MID_SRCS} ${SEQ2MID_HDRS})

#============================================================================
# seqq2mid
#============================================================================

set(SEQQ2MID_SRCS
    src/common/cutils.h
)

set(SEQQ2MID_HDRS
    src/common/cioutils.h
    src/common/cutils.c
    src/seqq2mid/seqq2mid.c
)

add_executable(seqq2mid ${SEQQ2MID_SRCS} ${SEQQ2MID_HDRS})
