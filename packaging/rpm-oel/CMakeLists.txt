# Copyright (c) 2012, 2015, Oracle and/or its affiliates. All rights reserved.
# 
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; version 2 of the License.
# 
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# 
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301 USA


IF(UNIX)
  SET(prefix ${CMAKE_INSTALL_PREFIX})

  SET(SPECFILENAME "myblockchain.spec")
  IF("${VERSION}" MATCHES "-ndb-")
    STRING(REGEX REPLACE "^.*-ndb-" "" NDBVERSION "${VERSION}")
    SET(SPECFILENAME "myblockchain-cluster-${NDBVERSION}.spec")
  ENDIF()

  # Left in current directory, to be taken during build
  CONFIGURE_FILE(myblockchain.spec.in ${CMAKE_CURRENT_BINARY_DIR}/${SPECFILENAME} @ONLY)

  FOREACH(fedfile my.cnf my_config.h myblockchain_config.sh
          myblockchain.init filter-requires.sh filter-provides.sh
          myblockchain-5.7-libmyblockchainclient-symbols.patch)
    CONFIGURE_FILE(${fedfile} ${CMAKE_CURRENT_BINARY_DIR}/${fedfile} COPYONLY)
  ENDFOREACH()
ENDIF()

