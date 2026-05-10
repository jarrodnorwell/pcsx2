cmake_minimum_required(VERSION 3.16...3.25)

project(Alune C CXX OBJC Swift)

set(TEAM "Z3ZEABYY6P" CACHE STRING "IDENTIFIER")

set(SWIFT_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Actors/GamesManager.swift"

    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Classes/BaseGame.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Classes/Cell.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Classes/Game.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Classes/GameDetails.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Classes/ThumbstickView.swift"

    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Controllers/EmulationController.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Controllers/GamesController.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Controllers/SettingsController.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Controllers/TabController.swift"

    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Extensions/RangeReplaceableCollection.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Extensions/Sequence.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Extensions/UIButton.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Extensions/UIView.swift"

    "${CMAKE_CURRENT_SOURCE_DIR}/swift/ISOKit/ISO9660.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/ISOKit/ISOKit.swift"

    "${CMAKE_CURRENT_SOURCE_DIR}/swift/SettingsKit/SettingDelegate.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/SettingsKit/SettingHeader.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/SettingsKit/SettingsKit.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/SettingsKit/SettingTypes.swift"

    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Structs/CellManager.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/Structs/LayoutManager.swift"

    "${CMAKE_CURRENT_SOURCE_DIR}/swift/AluneBridgeSwift.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/AppDelegate.swift"
    "${CMAKE_CURRENT_SOURCE_DIR}/swift/SceneDelegate.swift"
)

set(OBJC_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/objcpp/AluneBridge.mm"
    "${CMAKE_CURRENT_SOURCE_DIR}/objcpp/AudioStream.mm"
    "${CMAKE_CURRENT_SOURCE_DIR}/objcpp/CocoaTools.mm"
    "${CMAKE_CURRENT_SOURCE_DIR}/objcpp/Common.mm"
    "${CMAKE_CURRENT_SOURCE_DIR}/objcpp/FileSystem.mm"
    "${CMAKE_CURRENT_SOURCE_DIR}/objcpp/Global.mm"
    "${CMAKE_CURRENT_SOURCE_DIR}/objcpp/Host.mm"
    "${CMAKE_CURRENT_SOURCE_DIR}/objcpp/HTTPDownloader.mm"
    "${CMAKE_CURRENT_SOURCE_DIR}/objcpp/InputManager.mm"
    "${CMAKE_CURRENT_SOURCE_DIR}/objcpp/IOCtlSrc.mm"
    "${CMAKE_CURRENT_SOURCE_DIR}/objcpp/PCAPAdapter.mm"
)

add_executable(Alune ${SWIFT_SOURCES} ${OBJC_SOURCES})

# configure_file(Info.plist "${CMAKE_BINARY_DIR}/Info.plist" @ONLY)

set_target_properties(Alune PROPERTIES
    MACOSX_BUNDLE TRUE
    MACOSX_BUNDLE_GUI_IDENTIFIER "com.antique.Alune"
    MACOSX_BUNDLE_BUNDLE_NAME "Alune"
    MACOSX_BUNDLE_BUNDLE_VERSION "2"
    MACOSX_BUNDLE_SHORT_VERSION_STRING "1.0"
    MACOSX_BUNDLE_INFO_PLIST "${CMAKE_CURRENT_SOURCE_DIR}/Info.plist"
    MACOSX_BUNDLE_EXECUTABLE_NAME "Alune"
    XCODE_ATTRIBUTE_PRODUCT_BUNDLE_IDENTIFIER "com.antique.Alune"
    XCODE_ATTRIBUTE_TARGETED_DEVICE_FAMILY "1,2" # 1=iPhone, 2=iPad
    XCODE_ATTRIBUTE_IPHONEOS_DEPLOYMENT_TARGET "16.0"
    XCODE_ATTRIBUTE_SDKROOT "${CMAKE_OSX_SYSROOT}"
    #XCODE_ATTRIBUTE_CODE_SIGNING_REQUIRED "NO"
    XCODE_ATTRIBUTE_CODE_SIGN_STYLE "Automatic"
    #XCODE_ATTRIBUTE_CODE_SIGN_IDENTITY ""
    XCODE_ATTRIBUTE_DEVELOPMENT_TEAM "${TEAM}"
    # XCODE_ATTRIBUTE_PROVISIONING_PROFILE_SPECIFIER "Alune"
    XCODE_ATTRIBUTE_CODE_SIGN_ENTITLEMENTS "${CMAKE_CURRENT_SOURCE_DIR}/Alune.entitlements"
    XCODE_ATTRIBUTE_SWIFT_OBJC_BRIDGING_HEADER "${CMAKE_CURRENT_SOURCE_DIR}/objcpp/Alune-Bridging-Header.h"
    XCODE_ATTRIBUTE_SWIFT_VERSION "5.0"
    XCODE_ATTRIBUTE_CLANG_ENABLE_MODULES "YES"

    XCODE_ATTRIBUTE_MTL_HEADER_SEARCH_PATHS "${CMAKE_SOURCE_DIR}/pcsx2/GS/Renderers/Metal ${CMAKE_SOURCE_DIR}/bin"
    XCODE_ATTRIBUTE_ASSETCATALOG_COMPILER_APPICON_NAME "App Icon"
)

set_target_properties(Alune PROPERTIES
    XCODE_ATTRIBUTE_OTHER_SWIFT_FLAGS ""
)

target_compile_options(Alune PRIVATE $<$<COMPILE_LANGUAGE:C,CXX,OBJC,OBJCXX>:-pipe>)

target_link_libraries(Alune PRIVATE
    PCSX2
    SDL3::SDL3-static

    "-framework UIKit"
    "-framework Metal"
    "-framework MetalKit"
    "-framework QuartzCore"
    "-framework CoreText"
    "-framework CoreGraphics"
)

set_target_properties(Alune PROPERTIES
    XCODE_ATTRIBUTE_LD_RUNPATH_SEARCH_PATHS "@executable_path/Frameworks"
)

target_compile_definitions(Alune PRIVATE
    PCSX2_NO_PCAP=1
)

# include_dir...
target_include_directories(Alune PRIVATE
    ${CMAKE_SOURCE_DIR}
    ${CMAKE_SOURCE_DIR}/common
    ${CMAKE_SOURCE_DIR}/pcsx2
    ${CMAKE_SOURCE_DIR}/pcsx2/GS/Renderers/Metal
    ${CMAKE_SOURCE_DIR}/3rdparty/fmt/include
    ${CMAKE_SOURCE_DIR}/3rdparty/fast_float/include
    ${CMAKE_SOURCE_DIR}/3rdparty/vixl/include
    ${CMAKE_SOURCE_DIR}/3rdparty/include
    ${CMAKE_SOURCE_DIR}/3rdparty/vulkan/include
    ${CMAKE_BINARY_DIR}/common/include
    ${CMAKE_BINARY_DIR}/pcsx2
    ${CMAKE_SOURCE_DIR}/3rdparty/simpleini/include
    ${CMAKE_SOURCE_DIR}/3rdparty/imgui/include
    ${CMAKE_SOURCE_DIR}/3rdparty/cpuinfo/include
    ${CMAKE_SOURCE_DIR}/3rdparty/libchdr/include
    ${CMAKE_SOURCE_DIR}/3rdparty/libzip/lib
    ${CMAKE_BINARY_DIR}/3rdparty/libzip
    ${CMAKE_SOURCE_DIR}/3rdparty/cubeb/include
    ${CMAKE_SOURCE_DIR}/3rdparty/rcheevos/include
    ${CMAKE_SOURCE_DIR}/3rdparty/freesurround/include
    ${CMAKE_SOURCE_DIR}/3rdparty/soundtouch/soundtouch
    ${CMAKE_SOURCE_DIR}/3rdparty/lzma/include
    ${CMAKE_SOURCE_DIR}/3rdparty/demangler/include
    ${CMAKE_SOURCE_DIR}/3rdparty/ccc/src
)

file(GLOB METAL_SHADERS "${CMAKE_SOURCE_DIR}/pcsx2/GS/Renderers/Metal/*.metal")

source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}/objcpp" PREFIX "ObjC" FILES ${OBJC_SOURCES})
source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}/swift" PREFIX "Swift" FILES ${SWIFT_SOURCES})

target_sources(Alune PRIVATE
    "${CMAKE_CURRENT_SOURCE_DIR}/App Icon.icon"
    #"${CMAKE_CURRENT_SOURCE_DIR}/Info.plist"
    "${CMAKE_CURRENT_SOURCE_DIR}/LaunchScreen.storyboard"
    ${METAL_SHADERS}
    "${CMAKE_SOURCE_DIR}/bin/resources.zip"
)

set_source_files_properties(${METAL_SHADERS} PROPERTIES 
    XCODE_EXPLICIT_FILE_TYPE "sourcecode.metal"
    MACOSX_PACKAGE_LOCATION Resources
)

set_source_files_properties("${CMAKE_CURRENT_SOURCE_DIR}/App Icon.icon" PROPERTIES
    MACOSX_PACKAGE_LOCATION Resources
)

#set_source_files_properties("${CMAKE_CURRENT_SOURCE_DIR}/Info.plist" PROPERTIES
#    MACOSX_PACKAGE_LOCATION Resources
#)

set_source_files_properties("${CMAKE_CURRENT_SOURCE_DIR}/LaunchScreen.storyboard" PROPERTIES 
    MACOSX_PACKAGE_LOCATION Resources
)

set_source_files_properties("${CMAKE_SOURCE_DIR}/bin/resources.zip" PROPERTIES 
    MACOSX_PACKAGE_LOCATION Resources
)