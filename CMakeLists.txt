# # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # #
#                                                                                 #
#  Copyright (c) 2019, De Graef Group, Carnegie Mellon University                 #
#  All rights reserved.                                                           #
#                                                                                 #
#  Author William C. Lenthe                                                       #
#                                                                                 #
#  Redistribution and use in source and binary forms, with or without             #
#  modification, are permitted provided that the following conditions are met:    #
#                                                                                 #
#  1. Redistributions of source code must retain the above copyright notice, this #
#     list of conditions and the following disclaimer.                            #
#                                                                                 #
#  2. Redistributions in binary form must reproduce the above copyright notice,   #
#     this list of conditions and the following disclaimer in the documentation   #
#     and/or other materials provided with the distribution.                      #
#                                                                                 #
#  3. Neither the name of the copyright holder nor the names of its               #
#     contributors may be used to endorse or promote products derived from        #
#     this software without specific prior written permission.                    #
#                                                                                 #
#  THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"    #
#  AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE      #
#  IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE #
#  DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE   #
#  FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL     #
#  DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR     #
#  SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER     #
#  CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,  #
#  OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE  #
#  OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.           #
#                                                                                 #
# # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # #

project(SHTfile)
cmake_minimum_required(VERSION 3.14)

set(CMAKE_CXX_STANDARD 11) # apparently you still need to explicitly request 2011 conformance in 2019...

################################
#       file versioning        #
################################

# find git
find_package(Git REQUIRED)

# try to get abbreviated hash
execute_process( # get abbreviated hash
	COMMAND ${GIT_EXECUTABLE} rev-parse --short HEAD
	WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
	OUTPUT_VARIABLE SHT_GIT_HASH
	OUTPUT_STRIP_TRAILING_WHITESPACE
)

# make sure we actually got a hash
if("${SHT_GIT_HASH}" STREQUAL "")
	message(FATAL_ERROR "failed to determine git hash)")
endif()
string(CONCAT SHT_FILE_VERS "v" ${SHT_GIT_HASH}) # hash is only 7 characters, make 8 by prepending with v
string(LENGTH ${SHT_FILE_VERS} STR_LEN)
if(NOT 8 EQUAL ${STR_LEN})
	message(FATAL_ERROR "Git Hash must be 8 characters long. Computed was: ${SHT_FILE_VERS}")
endif()

# configure file using version string and include 
configure_file(${SHTfile_SOURCE_DIR}/sht_file.in.hpp ${SHTfile_BINARY_DIR}/sht_file.hpp)

