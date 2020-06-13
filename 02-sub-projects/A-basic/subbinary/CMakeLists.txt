project(subbinary)

# Create the executable
add_executable(${PROJECT_NAME} main.cpp)

# 使用它的别名sub::lib1链接subproject1中的静态库
# 使用它的别名sub::lib2链接subproject2中的头文件。
# 这将导致该目标的include目录被添加到这个项目中。
target_link_libraries(${PROJECT_NAME}
    sub::lib1
    sub::lib2
)
