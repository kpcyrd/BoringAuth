NAME    = boringauth-test.bin
CHANNEL = debug
LIBDIR  = ../target/$(CHANNEL)/
INCDIR  = ../include/
SRC     = test_boringauth.c \
		test_hotp.c \
		test_pass.c \
		test_totp.c
OBJ     = $(SRC:.c=.o)
CC      = clang
CFLAGS  = -std=c11 -g -Wall -Wextra -Wpedantic -Wshadow -Werror -Wstrict-overflow -fno-strict-aliasing -I$(INCDIR)
LDFLAGS = -L$(LIBDIR) -lboringauth
PYSRC   = boringauth_test


all: $(NAME)

$(NAME): $(OBJ)
	$(CC) -o $(NAME) $(OBJ) $(LDFLAGS)

test: cc_version valgrind_version $(NAME)
	@(hash valgrind 2>/dev/null && LD_LIBRARY_PATH=$(LIBDIR) valgrind ./$(NAME))
	@LD_LIBRARY_PATH=$(LIBDIR) ./$(NAME)
	@(hash python2 2>/dev/null && python2 --version && python2 -m unittest $(PYSRC))
	@(hash python3 2>/dev/null && python3 --version && python3 -m unittest $(PYSRC))

tests: test

clean:
	@rm -rf $(NAME) $(OBJ) '__pycache__' *.pyc

cc_version:
	$(CC) --version

valgrind_version:
	valgrind --version

re: clean all

.PHONY: all test tests clean cc_version valgrind_version re
