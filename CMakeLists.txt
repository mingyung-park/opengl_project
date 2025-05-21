cmake_minimum_required(VERSION 3.14)
# define FMT_UNICODE 0
# 프로젝트 이름 및 C++ 표준 설정
set(PROJECT_NAME opengl_project)
set(CMAKE_CXX_STANDARD 17)

#==================================================================
# GLFW - Windows 설정
set(WINDOW_NAME "Hello, OpenGL!")
set(WINDOW_WIDTH 640)
set(WINDOW_HEIGHT 480) 

#==================================================================
# 프로젝트 소스코드 설정
project(${PROJECT_NAME})
add_executable(${PROJECT_NAME} src/main.cpp)

# 프롲겍트 외부 Dependency 설정
include(Dependency.cmake)

#==================================================================
# 이 라이브러리를 프로젝트에서 사용
# 프로젝트에 include / lib 관련 옵션 추가
target_include_directories(${PROJECT_NAME} PUBLIC ${DEP_INCLUDE_DIR})
target_link_directories(${PROJECT_NAME} PUBLIC ${DEP_LIB_DIR})
target_link_libraries(${PROJECT_NAME} PUBLIC ${DEP_LIBS})

#==================================================================
# GLFW - predefined macro 로 추가
target_compile_definitions(${PROJECT_NAME} PUBLIC
  WINDOW_NAME="${WINDOW_NAME}"
  WINDOW_WIDTH=${WINDOW_WIDTH}
  WINDOW_HEIGHT=${WINDOW_HEIGHT}
  )

# Dependency들이 먼저 build 될 수 있게 관계 설정
add_dependencies(${PROJECT_NAME} ${DEP_LIST})