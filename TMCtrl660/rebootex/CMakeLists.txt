add_executable(reboot)

target_sources(reboot PRIVATE 
   ${BOOTLOADEX}/crt0.S
   ${BOOTLOADEX}/main.c
   ${BOOTLOADEX}/lib.c
   ${BOOTLOADEX}/pspbtcnf.c
   ${BOOTLOADEX}/scanner.c
   ${BOOTLOADEX}/psp_rebootex/patches.c)

target_compile_definitions(reboot PRIVATE REBOOTEX MS_IPL FAT_DEBUG)
target_include_directories(reboot PRIVATE ${BOOTLOADEX})
target_link_libraries(reboot PRIVATE iplsdk)
target_link_options(reboot PRIVATE -nostdlib -T${BOOTLOADEX}/linkfile.l)

if(DEBUG)
    target_compile_definitions(reboot PRIVATE -DDEBUG=${DEBUG})
    target_link_libraries(reboot PRIVATE colordebugger)
endif()

add_custom_command(
    TARGET reboot
    POST_BUILD
    COMMAND psp-strip -s reboot
    COMMAND psp-objcopy -O binary reboot reboot.bin
    COMMAND python3 "${ARKSDK}/build-tools/gz/gz.py" reboot.bin reboot.bin.gz
)
