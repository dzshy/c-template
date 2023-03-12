target = foo
cflags = -g
ldflags = -g
cc = gcc

src = $(shell ls src/*.c)
obj = $(src:.c=.o)
tests=$(shell ls tests/*.c)
tests_bin=$(tests:.c=.bin)

all: lib$(target).so

lib$(target).so: $(obj)
	$(cc) -shared -fPIC $(ldflags) -o $@ $^ -lc

test: $(tests_bin)
	@echo
	@echo "Run tests:"
	@scripts/runall.sh $^

$(obj):%.o:%.c
	$(cc) -c -fPIC $(cflags) $< -MD -MF $@.d -o $@

$(tests_bin):%.bin:%.c $(obj)
	$(cc) -Isrc/ $(cflags) $< $(obj) -MD -MF $@.d -o $@

clean:
	-rm $(shell find tests/ -name '*.bin')
	-rm $(shell find . -name '*.o' -or -name '*.so' -or -name '*.d')

fmt:
	sh scripts/format.sh

DEPS := $(shell find . -name *.d)
ifneq ($(DEPS),)
include $(DEPS)
endif
