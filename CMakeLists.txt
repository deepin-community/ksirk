cmake_minimum_required (VERSION 3.5 FATAL_ERROR)

# KDE Application Version, managed by release script
set(RELEASE_SERVICE_VERSION_MAJOR "21")
set(RELEASE_SERVICE_VERSION_MINOR "08")
set(RELEASE_SERVICE_VERSION_MICRO "0")
set(RELEASE_SERVICE_VERSION "${RELEASE_SERVICE_VERSION_MAJOR}.${RELEASE_SERVICE_VERSION_MINOR}.${RELEASE_SERVICE_VERSION_MICRO}")

project(ksirk VERSION ${RELEASE_SERVICE_VERSION})

set (QT_MIN_VERSION "5.12.0")
set (KF5_MIN_VERSION "5.68.0")

find_package(ECM ${KF5_MIN_VERSION} REQUIRED CONFIG)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${ECM_MODULE_PATH} )

find_package(Qt5 ${QT_MIN_VERSION} REQUIRED NO_MODULE COMPONENTS Widgets Svg Test)
find_package(KF5 ${KF5_MIN_VERSION} REQUIRED COMPONENTS
    Completion
    Config
    ConfigWidgets
    CoreAddons
    Crash
    DocTools
    I18n
    KIO
    NewStuff
    Wallet
    WidgetsAddons
    XmlGui
)

find_package(KF5KDEGames 4.9.0 REQUIRED)
find_package(Phonon4Qt5 REQUIRED)

include(FeatureSummary)
include(ECMAddAppIcon)
include(ECMInstallIcons)
include(ECMSetupVersion)
include(KDEInstallDirs)
include(KDECompilerSettings NO_POLICY_SCOPE)
include(KDECMakeSettings)
include(ECMQtDeclareLoggingCategory)

add_definitions(-DQT_USE_FAST_CONCATENATION -DQT_USE_FAST_OPERATOR_PLUS)
add_definitions(-DQT_NO_URL_CAST_FROM_STRING)

find_package(Phonon4Qt5 CONFIG REQUIRED)
find_package(Qca-qt5 2.1.0)
set_package_properties(Qca-qt5 PROPERTIES DESCRIPTION "Support for encryption"
                       URL "https://download.kde.org/stable/qca-qt5"
                       TYPE REQUIRED)

kde_enable_exceptions()

remove_definitions(-DQT_NO_STL) 

ecm_setup_version(${RELEASE_SERVICE_VERSION} VARIABLE_PREFIX KSIRK VERSION_HEADER ksirk_version.h)

add_subdirectory(ksirk)
add_subdirectory(ksirkskineditor)
add_subdirectory(doc)
ecm_qt_install_logging_categories(
        EXPORT KSIRK
        FILE ksirk.categories
        DESTINATION ${KDE_INSTALL_LOGGINGCATEGORIESDIR}
        )
feature_summary(WHAT ALL INCLUDE_QUIET_PACKAGES FATAL_ON_MISSING_REQUIRED_PACKAGES)
ki18n_install(po)
if (KF5DocTools_FOUND)
 kdoctools_install(po)
endif()
