cmake_minimum_required(VERSION 2.8)
project(libco)

enable_language(C ASM)

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -fno-strict-aliasing -O2 -Wall -export-dynamic -pipe  -D_GNU_SOURCE -D_REENTRANT -fPIC -Wno-deprecated -m64")

set_source_files_properties(coctx_swap.S PROPERTIES COMPILE_FLAGS "-x assembler-with-cpp")
set(DIR_SRCS co_epoll.cpp co_routine.cpp co_hook_sys_call.cpp coctx_swap.S coctx.cpp)

set(LINK_LIBS dl)
if(CMAKE_THREAD_LIBS_INIT)
    set(LINK_LIBS ${LINK_LIBS} "${CMAKE_THREAD_LIBS_INIT}")
endif()

add_library(colib ${DIR_SRCS})
if(THREADS_HAVE_PTHREAD_ARG)
    target_compile_options(colib PUBLIC "-pthread")
endif()
target_link_libraries(colib ${LINK_LIBS})

add_executable(example_echosvr example_echosvr.cpp)
if(THREADS_HAVE_PTHREAD_ARG)
    target_compile_options(example_echosvr PUBLIC "-pthread")
endif()
target_link_libraries(example_echosvr colib ${LINK_LIBS})

add_executable(example_echocli example_echocli.cpp)
if(THREADS_HAVE_PTHREAD_ARG)
    target_compile_options(example_echocli PUBLIC "-pthread")
endif()
target_link_libraries(example_echocli colib ${LINK_LIBS})

add_executable(example_thread example_thread.cpp)
if(THREADS_HAVE_PTHREAD_ARG)
    target_compile_options(example_thread PUBLIC "-pthread")
endif()
target_link_libraries(example_thread colib ${LINK_LIBS})

add_executable(example_poll example_poll.cpp)
if(THREADS_HAVE_PTHREAD_ARG)
    target_compile_options(example_poll PUBLIC "-pthread")
endif()
target_link_libraries(example_poll colib ${LINK_LIBS})

add_executable(example_cond example_cond.cpp)
if(THREADS_HAVE_PTHREAD_ARG)
    target_compile_options(example_cond PUBLIC "-pthread")
endif()
target_link_libraries(example_cond colib ${LINK_LIBS})

add_executable(example_specific example_specific.cpp)
if(THREADS_HAVE_PTHREAD_ARG)
    target_compile_options(example_specific PUBLIC "-pthread")
endif()
target_link_libraries(example_specific colib ${LINK_LIBS})

add_executable(example_copystack example_copystack.cpp)
if(THREADS_HAVE_PTHREAD_ARG)
    target_compile_options(example_copystack PUBLIC "-pthread")
endif()
target_link_libraries(example_copystack colib ${LINK_LIBS})

add_executable(example_setenv example_setenv.cpp)
if(THREADS_HAVE_PTHREAD_ARG)
    target_compile_options(example_setenv PUBLIC "-pthread")
endif()
target_link_libraries(example_setenv colib ${LINK_LIBS})

add_executable(example_closure example_closure.cpp)
if(THREADS_HAVE_PTHREAD_ARG)
    target_compile_options(example_closure PUBLIC "-pthread")
endif()
target_link_libraries(example_closure colib ${LINK_LIBS})