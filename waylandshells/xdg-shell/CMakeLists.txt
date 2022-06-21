# This file is part of Desktop App Toolkit,
# a set of libraries for developing nice desktop applications.
#
# For license and copyright information please follow this link:
# https://github.com/desktop-app/legal/blob/master/LEGAL

add_library(lib_waylandshells_xdg_shell STATIC)
add_library(desktop-app::lib_waylandshells::xdg_shell ALIAS lib_waylandshells_xdg_shell)
init_target(lib_waylandshells_xdg_shell)

get_filename_component(src_loc . REALPATH)
set_target_properties(lib_waylandshells_xdg_shell PROPERTIES AUTOMOC ON)

if (DESKTOP_APP_QT6)
    qt6_generate_wayland_protocol_client_sources(lib_waylandshells_xdg_shell
    FILES
        ${WaylandProtocols_DATADIR}/stable/xdg-shell/xdg-shell.xml
        ${WaylandProtocols_DATADIR}/unstable/xdg-decoration/xdg-decoration-unstable-v1.xml
        ${WaylandProtocols_DATADIR}/staging/xdg-activation/xdg-activation-v1.xml
    )
else()
    ecm_add_qtwayland_client_protocol(lib_waylandshells_xdg_shell
        PROTOCOL ${WaylandProtocols_DATADIR}/stable/xdg-shell/xdg-shell.xml
        BASENAME xdg-shell
    )
    ecm_add_qtwayland_client_protocol(lib_waylandshells_xdg_shell
        PROTOCOL ${WaylandProtocols_DATADIR}/unstable/xdg-decoration/xdg-decoration-unstable-v1.xml
        BASENAME xdg-decoration-unstable-v1
    )
    ecm_add_qtwayland_client_protocol(lib_waylandshells_xdg_shell
        PROTOCOL ${WaylandProtocols_DATADIR}/staging/xdg-activation/xdg-activation-v1.xml
        BASENAME xdg-activation-v1
    )
    target_include_directories(lib_waylandshells_xdg_shell PRIVATE ${CMAKE_CURRENT_BINARY_DIR})
endif()

nice_target_sources(lib_waylandshells_xdg_shell ${src_loc}
PRIVATE
    main.cpp
    qwaylandxdgactivationv1.cpp
    qwaylandxdgactivationv1_p.h
    qwaylandxdgdecorationv1.cpp
    qwaylandxdgdecorationv1_p.h
    qwaylandxdgshell.cpp
    qwaylandxdgshell_p.h
    qwaylandxdgshellintegration.cpp
    qwaylandxdgshellintegration_p.h
)

target_include_directories(lib_waylandshells_xdg_shell
PRIVATE
    ${src_loc}
)

target_compile_options(lib_waylandshells_xdg_shell
PRIVATE
    -UQT_NO_KEYWORDS
)

target_compile_definitions(lib_waylandshells_xdg_shell
PRIVATE
    QT_STATICPLUGIN
)

target_link_libraries(lib_waylandshells_xdg_shell
PRIVATE
    desktop-app::external_qt
    desktop-app::external_wayland_client
)
