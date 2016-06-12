cmake_minimum_required(VERSION 2.6.3)

project(cogimon-scenarios)

# configure env file to adapt to install prefix
configure_file (
  "${PROJECT_SOURCE_DIR}/setup-cogimon-env.sh.in"
  "${PROJECT_SOURCE_DIR}/setup-cogimon-env.sh"
  )

file(GLOB DEPLOY_FILES_AND_DIRS "${PROJECT_SOURCE_DIR}/*")
foreach(ITEM ${DEPLOY_FILES_AND_DIRS})
   get_filename_component(tmpName ${ITEM} NAME)
   SET(fullName "${PROJECT_SOURCE_DIR}/${tmpName}")
   
   IF( IS_DIRECTORY "${ITEM}" )
      IF( NOT ((${ITEM} MATCHES "build") OR (${ITEM} MATCHES ".git")))
        message("${ITEM}")
        LIST( APPEND DIRS_TO_DEPLOY "${ITEM}" )
      ENDIF()
   ELSE()
      # TODO improve this selection and use PROJECT_BINARY_DIR
      IF( NOT ((${ITEM} MATCHES "CMakeLists.txt") OR (${ITEM} MATCHES "README.md") OR (${ITEM} MATCHES ".gitignore") OR (${ITEM} MATCHES "rsb.conf") OR (${ITEM} MATCHES "setup-cogimon-env.sh") OR (${ITEM} MATCHES "setup-cogimon-env.sh.in")))
        LIST( APPEND FILES_TO_DEPLOY "${ITEM}" )
      ENDIF()
   ENDIF()
endforeach()

INSTALL( FILES ${FILES_TO_DEPLOY} DESTINATION "etc/${PROJECT_NAME}")
INSTALL( FILES "rsb.conf" DESTINATION "etc/")
INSTALL( FILES "setup-cogimon-env.sh" DESTINATION "bin/")
INSTALL( DIRECTORY ${DIRS_TO_DEPLOY} DESTINATION "etc/${PROJECT_NAME}")
