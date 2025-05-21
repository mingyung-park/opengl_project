cmake_minimum_required(VERSION 3.14)
# define FMT_UNICODE 0
# 프로젝트 이름 및 C++ 표준 설정
set(PROJECT_NAME opengl_project)
set(CMAKE_CXX_STANDARD 17)

# 프로젝트 버전 설정
project(${PROJECT_NAME})
add_executable(${PROJECT_NAME} src/main.cpp)

# ExternalProject 관련 명령어 셋 추가
include(ExternalProject)

# Dependency 관련 변수 설정
set(DEP_INSTALL_DIR ${PROJECT_BINARY_DIR}/install)
set(DEP_INCLUDE_DIR ${DEP_INSTALL_DIR}/include)
set(DEP_LIB_DIR ${DEP_INSTALL_DIR}/lib)

# ===========================
# MSVC 전용 설정
if(MSVC)
    # 메인 프로젝트 및 외부 라이브러리에 /utf-8 플래그 적용
    add_compile_options("$<$<CXX_COMPILER_ID:MSVC>:/utf-8>")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /utf-8")
endif()

# ===========================
# Dependency 리스트 및 라이브러리 파일 리스트 초기화
# spdlog: fast logger library
ExternalProject_Add(
    dep-spdlog # 해당 프로젝트의 이름 지정
    GIT_REPOSITORY "https://github.com/gabime/spdlog.git"
    GIT_TAG "v1.x"
    GIT_SHALLOW 1
    UPDATE_COMMAND ""
    PATCH_COMMAND ""
    CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${DEP_INSTALL_DIR}
        # -DCMAKE_CXX_FLAGS="/utf-8"
    TEST_COMMAND ""
)

# Dependency 리스트 및 라이브러리 파일 리스트 추가
set(DEP_LIST ${DEP_LIST} dep-spdlog)
set(DEP_LIBS ${DEP_LIBS} spdlog$<$<CONFIG:Debug>:d>)


# 이 라이브러리를 프로젝트에서 사용
# 우리 프로젝트에 include / lib 관련 옵션 추가
target_include_directories(${PROJECT_NAME} PUBLIC ${DEP_INCLUDE_DIR})
target_link_directories(${PROJECT_NAME} PUBLIC ${DEP_LIB_DIR})
target_link_libraries(${PROJECT_NAME} PUBLIC ${DEP_LIBS})

# Dependency들이 먼저 build 될 수 있게 관계 설정
add_dependencies(${PROJECT_NAME} ${DEP_LIST})