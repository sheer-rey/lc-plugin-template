cmake_minimum_required(VERSION 3.20)
project(cpp_template)

set(CMAKE_CXX_STANDARD 17)

# commonly used source files
file(GLOB_RECURSE COMMON_SOURCES "leetcode/editor/common/*.cpp")

# create a static library for common sources
add_library(common STATIC ${COMMON_SOURCES})

# glob all source files, normally its singale file with main function
file(GLOB_RECURSE MAIN_SOURCES "leetcode/editor/cn/*.cpp" "leetcode/editor/en/*.cpp")

# generate executables for each main source file
foreach(source_file ${MAIN_SOURCES})
    # generate a unique target name based on the file path
    get_filename_component(target_name ${source_file} NAME_WE)  # extract filename without extension
    get_filename_component(dir ${source_file} DIRECTORY)        # extract directory path
    file(RELATIVE_PATH relative_dir ${CMAKE_SOURCE_DIR}/leetcode ${dir}) # get relative path from leetcode directory
    string(REPLACE "/" "_" unique_prefix "${relative_dir}")     # replace / with _ to form a unique prefix
    set(target_name "${unique_prefix}_${target_name}")          # combine to form the final target name

    # generate the executable
    add_executable(${target_name} ${source_file})
    target_link_libraries(${target_name} common)
endforeach()
