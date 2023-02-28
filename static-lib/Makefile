target = foo
cflags = -g
cc = gcc

src = $(shell ls src/*.c)
obj = $(src:.c=.o)
tests=$(shell ls tests/*.c)
tests_bin=$(tests:.c=.bin)

all: lib$(target).a

lib$(target).a: $(obj)
	ar cr $@ $^

test: $(tests_bin)
	@echo
	@echo "Run tests:"
	@scripts/runall.sh $^

$(obj):%.o:%.c
	$(cc) -c $(cflags) $< -MD -MF $@.d -o $@

$(tests_bin):%.bin:%.c lib$(target).a
	$(cc) -Isrc/ $(cflags) $< lib$(target).a -MD -MF $@.d -o $@

clean:
	-rm $(shell find tests/ -name '*.bin')
	-rm $(shell find . -name '*.o' -or -name '*.a' -or -name '*.d')

fmt:
	sh scripts/format.sh

DEPS := $(shell find . -name *.d)
ifneq ($(DEPS),)
include $(DEPS)
endif
