add_executable(arc4_test
    test.c)

target_link_libraries(arc4_test
    PRIVATE
        arc4)

add_test(
    NAME arc4_test
    COMMAND arc4_test)
