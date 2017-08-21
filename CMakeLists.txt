cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB mercoin)

include_directories(${CMAKE_BINARY_DIR}
    src
    mercoin/external
    mercoin/include
    mercoin/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "1.1.1.1")
configure_file("mercoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  mercoin/external/miniupnpc/connecthostport.c
  mercoin/external/miniupnpc/igd_desc_parse.c
  mercoin/external/miniupnpc/minisoap.c
  mercoin/external/miniupnpc/miniupnpc.c
  mercoin/external/miniupnpc/miniwget.c
  mercoin/external/miniupnpc/minixml.c
  mercoin/external/miniupnpc/portlistingparse.c
  mercoin/external/miniupnpc/receivedata.c
  mercoin/external/miniupnpc/upnpcommands.c
  mercoin/external/miniupnpc/upnpreplyparse.c
  mercoin/src/Common/Base58.cpp
  mercoin/src/Common/CommandLine.cpp
  mercoin/src/Common/Util.cpp
  mercoin/src/Common/StringTools.cpp
  mercoin/src/Common/JsonValue.cpp
  mercoin/src/Common/ConsoleTools.cpp
  mercoin/src/Common/MemoryInputStream.cpp
  mercoin/src/Common/PathTools.cpp
  mercoin/src/Common/StdInputStream.cpp
  mercoin/src/Common/StdOutputStream.cpp
  mercoin/src/Common/StreamTools.cpp
  mercoin/src/Common/StringOutputStream.cpp
  mercoin/src/Common/StringView.cpp
  mercoin/src/Common/VectorOutputStream.cpp
  mercoin/src/crypto/blake256.c
  mercoin/src/crypto/chacha8.c
  mercoin/src/crypto/crypto-ops-data.c
  mercoin/src/crypto/crypto-ops.c
  mercoin/src/crypto/crypto.cpp
  mercoin/src/crypto/groestl.c
  mercoin/src/crypto/hash-extra-blake.c
  mercoin/src/crypto/hash-extra-groestl.c
  mercoin/src/crypto/hash-extra-jh.c
  mercoin/src/crypto/hash-extra-skein.c
  mercoin/src/crypto/hash.c
  mercoin/src/crypto/jh.c
  mercoin/src/crypto/keccak.c
  mercoin/src/crypto/oaes_lib.c
  mercoin/src/crypto/random.c
  mercoin/src/crypto/skein.c
  mercoin/src/crypto/slow-hash.c
  mercoin/src/crypto/slow-hash.cpp
  mercoin/src/crypto/tree-hash.c
  mercoin/src/CryptoNoteCore/BlockchainIndices.cpp
  mercoin/src/CryptoNoteCore/BlockchainMessages.cpp
  mercoin/src/CryptoNoteCore/BlockIndex.cpp
  mercoin/src/CryptoNoteCore/CoreConfig.cpp
  mercoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  mercoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  mercoin/src/CryptoNoteCore/Currency.cpp
  mercoin/src/CryptoNoteCore/MinerConfig.cpp
  mercoin/src/CryptoNoteCore/Transaction.cpp
  mercoin/src/CryptoNoteCore/Account.cpp
  mercoin/src/CryptoNoteCore/Blockchain.cpp
  mercoin/src/CryptoNoteCore/Checkpoints.cpp
  mercoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  mercoin/src/CryptoNoteCore/Core.cpp
  mercoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  mercoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  mercoin/src/CryptoNoteCore/Difficulty.cpp
  mercoin/src/CryptoNoteCore/IBlock.cpp
  mercoin/src/CryptoNoteCore/Miner.cpp
  mercoin/src/CryptoNoteCore/TransactionExtra.cpp
  mercoin/src/CryptoNoteCore/TransactionPool.cpp
  mercoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  mercoin/src/CryptoNoteCore/TransactionUtils.cpp
  mercoin/src/InProcessNode/InProcessNode.cpp
  mercoin/src/InProcessNode/InProcessNodeErrors.cpp
  mercoin/src/NodeRpcProxy/NodeErrors.cpp
  mercoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  mercoin/src/P2p/NetNodeConfig.cpp
  mercoin/src/Serialization/BinaryInputStreamSerializer.cpp
  mercoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  mercoin/src/Serialization/JsonInputValueSerializer.cpp
  mercoin/src/Serialization/JsonOutputStreamSerializer.cpp
  mercoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  mercoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  mercoin/src/Serialization/SerializationOverloads.cpp
  mercoin/src/Transfers/BlockchainSynchronizer.cpp
  mercoin/src/Transfers/SynchronizationState.cpp
  mercoin/src/Transfers/TransfersConsumer.cpp
  mercoin/src/Transfers/TransfersContainer.cpp
  mercoin/src/Transfers/TransfersSubscription.cpp
  mercoin/src/Transfers/TransfersSynchronizer.cpp
  mercoin/src/Wallet/LegacyKeysImporter.cpp
  mercoin/src/Wallet/WalletAsyncContextCounter.cpp
  mercoin/src/Wallet/WalletErrors.cpp
  mercoin/src/WalletLegacy/KeysStorage.cpp
  mercoin/src/WalletLegacy/WalletLegacy.cpp
  mercoin/src/WalletLegacy/WalletHelper.cpp
  mercoin/src/WalletLegacy/WalletLegacySerializer.cpp
  mercoin/src/WalletLegacy/WalletLegacySerialization.cpp
  mercoin/src/WalletLegacy/WalletTransactionSender.cpp
  mercoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  mercoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  mercoin/src/System/ContextGroup.cpp
  mercoin/src/System/Event.cpp
  mercoin/src/System/EventLock.cpp
  mercoin/src/System/InterruptedException.cpp
  mercoin/src/System/Ipv4Address.cpp
  mercoin/src/System/TcpStream.cpp
  mercoin/src/HTTP/HttpRequest.cpp
  mercoin/src/HTTP/HttpParser.cpp
  mercoin/src/HTTP/HttpParserErrorCodes.cpp
  mercoin/src/HTTP/HttpResponse.cpp
  mercoin/src/Rpc/HttpClient.cpp
  mercoin/src/Rpc/JsonRpc.cpp
  mercoin/src/P2p/NetNode.cpp
  mercoin/src/P2p/LevinProtocol.cpp
  mercoin/src/P2p/NetNodeConfig.cpp
  mercoin/src/P2p/PeerListManager.cpp
  mercoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  mercoin/src/Logging/ILogger.cpp
  mercoin/src/Logging/LoggerGroup.cpp
  mercoin/src/Logging/CommonLogger.cpp
  mercoin/src/Logging/LoggerManager.cpp
  mercoin/src/Logging/FileLogger.cpp
  mercoin/src/Logging/StreamLogger.cpp
  mercoin/src/Logging/ConsoleLogger.cpp
  mercoin/src/Logging/LoggerMessage.cpp
  mercoin/src/Logging/LoggerRef.cpp
  mercoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  mercoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  mercoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(mercoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} mercoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "mercoin GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME mercoinWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})

    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(mercoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES mercoin/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
