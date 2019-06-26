cmake_minimum_required(VERSION 3.1)

project(libgpu)

set(HEADERS
        libgpu/opencl/device_info.h
        libgpu/opencl/engine.h
        libgpu/opencl/enum.h
        libgpu/opencl/utils.h
        libgpu/context.h
        libgpu/device.h
        libgpu/gold_helpers.h
        libgpu/shared_device_buffer.h
        libgpu/shared_host_buffer.h
        libgpu/utils.h
        libgpu/work_size.h
        )

set(SOURCES
        libgpu/opencl/device_info.cpp
        libgpu/opencl/engine.cpp
        libgpu/opencl/enum.cpp
        libgpu/opencl/utils.cpp
        libgpu/context.cpp
        libgpu/device.cpp
        libgpu/gold_helpers.cpp
        libgpu/shared_device_buffer.cpp
        libgpu/shared_host_buffer.cpp
        libgpu/utils.cpp
        )


set (LIBRARIES
        libclew
        libutils)

set(CMAKE_CXX_STANDARD 11)


    add_library(${PROJECT_NAME} ${SOURCES} ${HEADERS})

target_include_directories(${PROJECT_NAME} PUBLIC ${PROJECT_SOURCE_DIR})
target_link_libraries(${PROJECT_NAME} ${LIBRARIES})

add_executable(hexdumparray libgpu/hexdumparray.cpp)

function(convertIntoHeader sourceFile headerFile arrayName)
    add_custom_command(
            OUTPUT ${PROJECT_SOURCE_DIR}/${headerFile}

            COMMAND hexdumparray ${PROJECT_SOURCE_DIR}/${sourceFile} ${PROJECT_SOURCE_DIR}/${headerFile} ${arrayName}

            DEPENDS ${PROJECT_SOURCE_DIR}/${sourceFile} hexdumparray
    )
endfunction()
