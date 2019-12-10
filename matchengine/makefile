TARGET  := matchengine.exe
INCS = -I ../network -I ../utils
LIBS = -L ../utils -lutils -L ../network -lrdkafka -lnetwork -Wl,-Bstatic -lev -ljansson -lmpdec -lz -lssl -lcrypto -lhiredis -Wl,-Bdynamic -lm -lpthread -ldl -lmysqlclient
include ../makefile.inc