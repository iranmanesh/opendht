
set(CURRENT_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(CURRENT_BINARY_DIR ${CMAKE_CURRENT_BINARY_DIR})

configure_file(setup.py.in setup.py)
configure_file(pyproject.toml pyproject.toml COPYONLY)

add_custom_target(python ALL
    COMMAND python3 setup.py build
    DEPENDS opendht opendht_cpp.pxd opendht.pyx pyproject.toml)

add_custom_target(dist
    COMMAND python3 -m build
    DEPENDS opendht opendht_cpp.pxd opendht.pyx pyproject.toml)

install(CODE "execute_process(COMMAND python3 setup.py install --root=\$ENV{DESTDIR}/ WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})")
if (OPENDHT_TOOLS)
	install(PROGRAMS tools/dhtcluster.py DESTINATION ${CMAKE_INSTALL_BINDIR} RENAME dhtcluster)
endif()