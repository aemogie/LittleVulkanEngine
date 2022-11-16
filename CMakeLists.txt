include(./.env.cmake OPTIONAL RESULT_VARIABLE LOCAL_ENV) # for specifying VULKAN_SDK_PATH
cmake_minimum_required(VERSION 3.11.0)

project("LittleVulkanEngine")

file(GLOB_RECURSE SRC CONFIGURE_DEPENDS "${CMAKE_SOURCE_DIR}/src/*")
add_executable (${PROJECT_NAME} ${SRC})
target_include_directories(${PROJECT_NAME} PUBLIC "${CMAKE_SOURCE_DIR}/src/")


set("VENDOR" "${CMAKE_SOURCE_DIR}/vendor")

#[[ VULKAN ]]
if (DEFINED VULKAN_SDK_PATH)
	set(Vulkan_INCLUDE_DIRS "${VULKAN_SDK_PATH}/Include")
	set(Vulkan_LIBRARIES "${VULKAN_SDK_PATH}/Lib")
	set(Vulkan_FOUND "True")
else()
	find_package("Vulkan" REQUIRED)
endif()
target_include_directories(${PROJECT_NAME} PUBLIC ${Vulkan_INCLUDE_DIRS})
target_link_libraries(${PROJECT_NAME} ${Vulkan_LIBRARIES})

#[[ GLFW ]]
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
add_subdirectory("${VENDOR}/glfw")
target_link_libraries(${PROJECT_NAME} "glfw")

#[[ GLM ]]
add_subdirectory("${VENDOR}/glm")
target_link_libraries(${PROJECT_NAME} "glm")

#[[ TINYOBJLOADER ]]
add_subdirectory("${VENDOR}/tinyobjloader")
target_include_directories(${PROJECT_NAME} PUBLIC "${VENDOR}/tinyobjloader")
target_link_libraries(${PROJECT_NAME} "tinyobjloader")
