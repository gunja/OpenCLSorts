project(Sorters)

cmake_minimum_required(VERSION 2.8)

enable_testing()

set(bitness "x86")
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
  set(bitness "x64")
endif()
find_package(OpenCL)

add_definitions(-D_CRT_SECURE_NO_WARNINGS)
include_directories($ENV{INTELOCLSDKROOT}/include)
link_directories($ENV{INTELOCLSDKROOT}/lib/${bitness})
add_executable(Quicksort QuicksortMain.cpp ImageUtils.h OpenCLUtils.h Quicksort.h QuicksortKernels.cl)
target_link_libraries(Quicksort ${OpenCL_LIBRARY} )

add_executable( Bitonicsort basic.cpp BitonicSort.cpp BitonicSort.cl cmdparser.cpp oclobject.cpp )
target_link_libraries(Bitonicsort ${OpenCL_LIBRARY} )

set(IO_UTILS
        io_utils/buffer_reader.cpp
        io_utils/buffer_reader.h
        io_utils/buffer_writer.cpp
        io_utils/buffer_writer.h
        io_utils/file_reader.cpp
        io_utils/file_reader.h
        io_utils/file_writer.cpp
        io_utils/file_writer.h
)
add_subdirectory(libs)

convertIntoHeader(cl/merge_sort.cl cl/merge_sort_cl.h merge_sort_kernel)
add_executable(external_merge_sort main_merge_sorter.cpp cl/merge_sort_cl.h ${IO_UTILS})
target_link_libraries(external_merge_sort libclew libgpu libutils)

add_executable(input_generator main_generator.cpp ${IO_UTILS})
target_link_libraries(input_generator libclew libgpu libutils)

add_test(NAME Quicksort_smoke
         WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
         COMMAND Quicksort 100 gpu intel 2048  2048  how_cl)
