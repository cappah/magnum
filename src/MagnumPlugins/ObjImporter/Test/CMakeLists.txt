#
#   This file is part of Magnum.
#
#   Copyright © 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017
#             Vladimír Vondruš <mosra@centrum.cz>
#
#   Permission is hereby granted, free of charge, to any person obtaining a
#   copy of this software and associated documentation files (the "Software"),
#   to deal in the Software without restriction, including without limitation
#   the rights to use, copy, modify, merge, publish, distribute, sublicense,
#   and/or sell copies of the Software, and to permit persons to whom the
#   Software is furnished to do so, subject to the following conditions:
#
#   The above copyright notice and this permission notice shall be included
#   in all copies or substantial portions of the Software.
#
#   THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
#   IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
#   FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
#   THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
#   LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
#   FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
#   DEALINGS IN THE SOFTWARE.
#

if(CORRADE_TARGET_EMSCRIPTEN OR CORRADE_TARGET_ANDROID)
    set(OBJIMPORTER_TEST_DIR ".")
else()
    set(OBJIMPORTER_TEST_DIR ${CMAKE_CURRENT_SOURCE_DIR})
endif()

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/configure.h.cmake
               ${CMAKE_CURRENT_BINARY_DIR}/configure.h)

corrade_add_test(ObjImporterTest Test.cpp
    LIBRARIES MagnumObjImporterTestLib
    FILES
        emptyFile.obj
        keywords.obj
        lineMesh.obj
        missingData.obj
        mixedPrimitives.obj
        moreMeshes.obj
        namedMesh.obj
        normals.obj
        optionalCoordinates.obj
        pointMesh.obj
        textureCoordinatesNormals.obj
        textureCoordinates.obj
        triangleMesh.obj
        unnamedFirstMesh.obj
        wrongIndexCount.obj
        wrongNumberCount.obj
        wrongNumbers.obj)
target_include_directories(ObjImporterTest PRIVATE ${CMAKE_CURRENT_BINARY_DIR})
set_target_properties(ObjImporterTest PROPERTIES FOLDER "MagnumPlugins/ObjImporter/Test")
# On Win32 we need to avoid dllimporting ObjImporter symbols, because it would
# search for the symbols in some DLL even though they were linked statically.
# However it apparently doesn't matter that they were dllexported when building
# the static library. EH.
if(WIN32)
    target_compile_definitions(ObjImporterTest PRIVATE "MAGNUM_OBJIMPORTER_BUILD_STATIC")
endif()
