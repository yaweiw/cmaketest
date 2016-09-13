add_library(MathFunctions MathFunctions.c)

install (TARGETS MathFunctions DESTINATION bin)
install (FILES MathFunctions.h DESTINATION inc)