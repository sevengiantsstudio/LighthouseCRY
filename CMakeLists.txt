cmake_minimum_required (VERSION 3.14)
set(CRYENGINE_DIR "C:/Program Files (x86)/Crytek/CRYENGINE Launcher/Crytek/CRYENGINE_5.6" CACHE STRING "CRYENGINE root directory.")
set(TOOLS_CMAKE_DIR "${CRYENGINE_DIR}/Tools/CMake")

set(PROJECT_BUILD 1)
set(PROJECT_DIR "${CMAKE_SOURCE_DIR}/../")

include("${TOOLS_CMAKE_DIR}/InitialSetup.cmake")
include("${TOOLS_CMAKE_DIR}/CommonOptions.cmake")

add_subdirectory("${CRYENGINE_DIR}" "${CMAKE_CURRENT_BINARY_DIR}/CRYENGINE")

include("${TOOLS_CMAKE_DIR}/Configure.cmake")
start_sources()

sources_platform(ALL)
add_sources("Code_uber.cpp"
    PROJECTS Game
    SOURCE_GROUP "Root"
		"GamePlugin.cpp"
		"StdAfx.cpp"
		"GamePlugin.h"
		"StdAfx.h"
)
add_sources("BehaviorTrees_uber.cpp"
    PROJECTS Game
    SOURCE_GROUP "BehaviorTrees"
		"BehaviorTrees/BehaviorTreeNodes_Assistant.cpp"
		"BehaviorTrees/BehaviorTreeNodes_Assistant.h"
)
add_sources("Components_uber.cpp"
    PROJECTS Game
    SOURCE_GROUP "Components"
		"Components/AreaTrigger.cpp"
		"Components/Assistant.cpp"
		"Components/CinematicTrigger.cpp"
		"Components/ClearAllComponents.cpp"
		"Components/SpawnPoint.cpp"
		"Components/Explosion.cpp"
		"Components/AreaTrigger.h"
		"Components/Assistant.h"
		"Components/CinematicTrigger.h"
		"Components/ClearAllComponents.h"
		"Components/Item.h"
		"Components/SpawnPoint.h"
		"Components/Explosion.h"
)

add_sources("Weapons_uber.cpp"
    PROJECTS Game
    SOURCE_GROUP "Components\\\\Weapons"
		"Components/Weapons/Projectile.cpp"
		"Components/Weapons/Weapon.cpp"
		"Components/Weapons/WeaponAttachment.cpp"
		"Components/Weapons/Projectile.h"
		"Components/Weapons/Weapon.h"
		"Components/Weapons/WeaponAttachment.h"
)

add_sources("Enemies_uber.cpp"
    PROJECTS Game
    SOURCE_GROUP "Components\\\\Enemies"
		"Components/Enemies/Turret.cpp"
		"Components/Enemies/Turret.h"
)

add_sources("DRS_uber.cpp"
    PROJECTS Game
    SOURCE_GROUP "DRS"
		"DRS/ActionShowGameSubtitle.cpp"
		"DRS/ActionShowGameSubtitle.h"
)
add_sources("External_uber.cpp"
    PROJECTS Game
    SOURCE_GROUP "External"
)
add_sources("ObserverManager_uber.cpp"
    PROJECTS Game
    SOURCE_GROUP "External\\\\ObserverManager"
		"External/ObserverManager/BaseObserverProtocol.cpp"
		"External/ObserverManager/ObserverManager.cpp"
		"External/ObserverManager/BaseObserverProtocol.h"
		"External/ObserverManager/ObserverContainer.h"
		"External/ObserverManager/ObserverManager.h"
)
add_sources("FlowNodes_uber.cpp"
    PROJECTS Game
    SOURCE_GROUP "FlowNodes"
		"FlowNodes/EntityFlowNode.cpp"
		"FlowNodes/FlowCameraNodes.cpp"
		"FlowNodes/FlowLocalizationNodes.cpp"
		"FlowNodes/FlowMovieCustomNodes.cpp"
		"FlowNodes/FlowPlayerNodes.cpp"
		"FlowNodes/EntityFlowNode.h"
)
add_sources("Framework_uber.cpp"
    PROJECTS Game
    SOURCE_GROUP "Framework"
		"Framework/PlayerController.cpp"
        "Framework/CameraController.cpp"
		"Framework/PlayerInput.cpp"
		"Framework/UIManager.cpp"
		"Framework/VehicleController.cpp"
		"Framework/IPawn.h"
		"Framework/PlayerController.h"
        "Framework/CameraController.h"
		"Framework/PlayerInput.h"
		"Framework/UIManager.h"
		"Framework/VehicleController.h"
)

add_sources("Events_uber.cpp"
    PROJECTS Game
    SOURCE_GROUP "Framework\\\\Events"
		"Framework/Events/PawnEvents.cpp"
		"Framework/Events/PawnEvents.h"
)

add_sources("Pawns_uber.cpp"
    PROJECTS Game
    SOURCE_GROUP "Pawns"
		"Pawns/Alex.cpp"
		"Pawns/Car.cpp"
		"Pawns/Alex.h"
		"Pawns/Car.h"
)

if(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/CVarOverrides.h")
    add_sources("NoUberFile"
        PROJECTS Game
        SOURCE_GROUP "Root"
            "CVarOverrides.h"
    )
endif()

if(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/EngineDefineOverrides.h")
    add_sources("NoUberFile"
        PROJECTS Game
        SOURCE_GROUP "Root"
            "EngineDefineOverrides.h"
    )
endif()
end_sources()

CryGameModule(Game FORCE_SHARED PCH "StdAfx.cpp" SOLUTION_FOLDER "Project")

target_include_directories(${THIS_PROJECT}
PRIVATE
    "${CRYENGINE_DIR}/Code/CryEngine/CryCommon"
    "${CRYENGINE_DIR}/Code/CryEngine/CryAction"
    "${CRYENGINE_DIR}/Code/CryEngine/CrySchematyc/Core/Interface"
    "${CRYENGINE_DIR}/Code/CryPlugins/CryDefaultEntities/Module"
)

if(CMAKE_CXX_COMPILER_ID MATCHES "[Cc]lang")
    target_compile_options(${THIS_PROJECT} PRIVATE
        -Wno-unused-variable
        -Wno-reorder
        -Wno-unknown-pragmas
        -Wno-parentheses
        -Wno-switch
        -Wno-format
        -Wno-dynamic-class-memaccess
        -Wno-unused-private-field
        -Wno-unused-value
        -Wno-invalid-offsetof
        -Wno-multichar
        -Wno-char-subscripts
        -Wno-null-conversion
        -Wno-empty-body
        -Wno-unused-lambda-capture
        -Wno-unused-function
        -Wno-tautological-constant-out-of-range-compare
    )
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    target_compile_options(${THIS_PROJECT} PRIVATE
        -Wno-unused-function
        -Wno-unused-value
        -Wno-unused-variable
        -Wno-aligned-new
        -Wno-int-in-bool-context
        -Wno-invalid-offsetof
        -Wno-comment
        -Wno-sign-compare
        -Wno-unused-but-set-variable
        -Wno-maybe-uninitialized
        -Wno-misleading-indentation
        -Wno-unused-result
        -Wno-unknown-pragmas
        -Wno-multichar
        -Wno-strict-aliasing
        -Wno-char-subscripts
        -Wno-conversion-null
        -Wno-reorder
        -Wno-parentheses
        -Wno-format
        -Wno-switch
    )
endif()


if(OPTION_ENGINE)
    if(NOT EXISTS "${CRYENGINE_DIR}/Code/Sandbox/EditorQt")
        add_library(Editor STATIC "${CRYENGINE_DIR}/Code/CryEngine/CryCommon/CryCore/Platform/platform.h")
        set_target_properties(Editor PROPERTIES LINKER_LANGUAGE CXX)
        if (WIN32)
            set_visual_studio_debugger_command(Editor "${CRYENGINE_DIR}/bin/win_x64/Sandbox.exe" "-project \"${PROJECT_DIR}/Game.cryproject\"")
        endif()
    endif()
else()
    add_library(GameLauncher STATIC "${CRYENGINE_DIR}/Code/CryEngine/CryCommon/CryCore/Platform/platform.h")
    set_target_properties(GameLauncher PROPERTIES LINKER_LANGUAGE CXX)
    if (WIN32)
        set_visual_studio_debugger_command(GameLauncher "${CRYENGINE_DIR}/bin/win_x64/GameLauncher.exe" "-project \"${PROJECT_DIR}/Game.cryproject\"")
    endif()

    add_library(Editor STATIC "${CRYENGINE_DIR}/Code/CryEngine/CryCommon/CryCore/Platform/platform.h")
    set_target_properties(Editor PROPERTIES LINKER_LANGUAGE CXX)
    if (WIN32)
        set_visual_studio_debugger_command(Editor "${CRYENGINE_DIR}/bin/win_x64/Sandbox.exe" "-project \"${PROJECT_DIR}/Game.cryproject\"")
    endif()

    add_library(GameServer STATIC "${CRYENGINE_DIR}/Code/CryEngine/CryCommon/CryCore/Platform/platform.h")
    set_target_properties(GameServer PROPERTIES LINKER_LANGUAGE CXX)
    if (WIN32)
        set_visual_studio_debugger_command(GameServer "${CRYENGINE_DIR}/bin/win_x64/Game_Server.exe" "-project \"${PROJECT_DIR}/Game.cryproject\"")
    endif()
endif()

# Set StartUp project in Visual Studio
set_solution_startup_target(GameLauncher)

if (WIN32)
    set_visual_studio_debugger_command( ${THIS_PROJECT} "${CRYENGINE_DIR}/bin/win_x64/GameLauncher.exe" "-project \"${PROJECT_DIR}/Game.cryproject\"" )
endif()

#BEGIN-CUSTOM
# Make any custom changes here, modifications outside of the block will be discarded on regeneration.
#END-CUSTOM