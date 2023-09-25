# OCPP library
add_library(ocpp)
add_library(everest::ocpp ALIAS ocpp)

target_sources(ocpp
    PRIVATE
        # lib/lib/ocpp/common/websocket/websocket_base.cpp
        # lib/lib/ocpp/common/charging_station_base.cpp
        # # lib/lib/ocpp/common/database_handler.cpp
        # lib/lib/ocpp/common/message_queue.cpp
        # lib/lib/ocpp/common/ocpp_logging.cpp
        # lib/lib/ocpp/common/pki_handler.cpp
        # lib/lib/ocpp/common/schemas.cpp
        # lib/lib/ocpp/common/types.cpp
        # lib/lib/ocpp/v16/charge_point.cpp
        # lib/lib/ocpp/v16/smart_charging.cpp
        # lib/lib/ocpp/v16/charge_point_configuration.cpp
        # lib/lib/ocpp/v16/charge_point_state_machine.cpp
        # lib/lib/ocpp/v16/transaction.cpp
        # lib/lib/ocpp/v16/enums.cpp
        # lib/lib/ocpp/v16/ocpp_types.cpp
        # lib/lib/ocpp/v16/types.cpp
        # lib/lib/ocpp/v201/charge_point.cpp
        # # lib/lib/ocpp/v201/device_model_management.cpp
        # lib/lib/ocpp/v201/enums.cpp
        # lib/lib/ocpp/v201/ocpp_types.cpp
        # lib/lib/ocpp/v201/types.cpp
        # lib/lib/ocpp/v201/connector.cpp
        # lib/lib/ocpp/v201/evse.cpp
        lib/ocpp/common/database_handler_base.cpp
        lib/ocpp/common/websocket/websocket_plain.cpp
        lib/ocpp/common/websocket/websocket_tls.cpp
        lib/ocpp/common/websocket/websocket.cpp
        lib/ocpp/common/websocket/websocket_base.cpp
        lib/ocpp/common/call_types.cpp
        lib/ocpp/common/charging_station_base.cpp
        lib/ocpp/common/message_queue.cpp
        lib/ocpp/common/ocpp_logging.cpp
        lib/ocpp/common/pki_handler.cpp
        lib/ocpp/common/schemas.cpp
        lib/ocpp/common/types.cpp
        lib/ocpp/common/utils.cpp
        lib/ocpp/v16/charge_point.cpp
        lib/ocpp/v16/database_handler.cpp
        lib/ocpp/v16/charge_point_impl.cpp
        lib/ocpp/v16/smart_charging.cpp
        lib/ocpp/v16/charge_point_configuration.cpp
        lib/ocpp/v16/charge_point_state_machine.cpp
        lib/ocpp/v16/transaction.cpp
        lib/ocpp/v16/enums.cpp
        lib/ocpp/v16/ocpp_types.cpp
        lib/ocpp/v16/types.cpp
        lib/ocpp/v201/charge_point.cpp
        lib/ocpp/v201/database_handler.cpp
        lib/ocpp/v201/device_model_storage_sqlite.cpp
        lib/ocpp/v201/device_model.cpp
        lib/ocpp/v201/transaction.cpp
        lib/ocpp/v201/enums.cpp
        lib/ocpp/v201/ocpp_types.cpp
        lib/ocpp/v201/types.cpp
        lib/ocpp/v201/connector.cpp
        lib/ocpp/v201/evse.cpp
        lib/ocpp/v201/utils.cpp        
        #lib/ocpp/v201/device_model_management.cpp
        #lib/ocpp/v201/device_model_management_base.cpp
        lib/ocpp/v201/ctrlr_component_variables.cpp

)

#optimization flags
target_compile_options(ocpp
    PUBLIC
        # -fno-rtti
        -Os
        -fPIC
        -ffunction-sections 
        -fdata-sections
        -g0
        -s
        -Wl,--gc-sections
        -Wl,--strip-all

        
)
target_link_options(ocpp
    PRIVATE
        -ffunction-sections 
        -fdata-sections
        -Wl,--gc-sections
        -Wl,--strip-all
)


add_subdirectory(lib/ocpp/common/websocket)
add_subdirectory(lib/ocpp/v16/messages)
add_subdirectory(lib/ocpp/v201/messages)


target_include_directories(ocpp
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

target_link_libraries(ocpp
    PUBLIC
        everest::log
        everest::timer
        fsm::fsm
        websocketpp::websocketpp

    PRIVATE
        OpenSSL::SSL
        OpenSSL::Crypto
        sqlite3
        Threads::Threads
        nlohmann_json::nlohmann_json
        nlohmann_json_schema_validator
        date::date-tz
    
)

if(BOOSTFILESYSTEM) 
target_link_libraries(ocpp
    PRIVATE
  		Boost::filesystem
)
endif()

message("Here" ${CXX_STANDARD})

# FIXME (aw): right now nlohmann_json and boost::optional don't compile
#             with gcc 10.x and C++11/14, so we need to publish the
#             C++17 standard
target_compile_features(ocpp PUBLIC cxx_std_17)

#install into system directories
install(TARGETS ocpp
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION libocpp
    INCLUDES DESTINATION include
)