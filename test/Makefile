nullstring :=
space := $(nullstring) # a space at the end
commaspace := ,$(space)

NAME=yaws_security_test
OBJDIR=./obj
EBINDIR=$(OBJDIR)/$(NAME)/ebin
SRCS += $(shell find src/*.erl)
OBJS = $(patsubst src/%.erl,$(EBINDIR)/%.beam,$(SRCS))
WSMODULES = $(patsubst src/%.erl,%, $(SRCS))
MODULES = $(subst $(space),$(commaspace), $(WSMODULES))
INCLUDES += -I/usr/lib
INCLUDES += -I./include
PKGS += -pa /usr/lib/yaws/ebin
PKGS += -pa $(EBINDIR)
PKGS += -pa ../$(OBJDIR)/yaws_security/ebin

all: application release

application: $(OBJS) $(EBINDIR)/$(NAME).app Makefile

release: $(OBJDIR)/$(NAME).boot Makefile

$(EBINDIR)/$(NAME).app: $(NAME).app Makefile
	@echo "Compiling (APPSPEC) $< to $@"
	@cat $< | sed "s/__MODULES__/$(MODULES)/" | sed 's/\[, /\[/' > $@ 

$(OBJS): $(SRCS) Makefile

$(EBINDIR)/%.beam: src/%.erl Makefile
	@touch $(NAME).app
	@echo "Compiling (Erlang) $< to $@"
	@mkdir -p $(EBINDIR)
	@erlc $(INCLUDES) -o $(EBINDIR) $<

$(OBJDIR)/$(NAME).boot: $(NAME).rel application
	@echo "Compiling (Release) $< to $@"
	@mkdir -p $(OBJDIR)
	@erlc $(PKGS) -o $(OBJDIR) $<

clean: 
	@rm -f *~
	@rm -f ./src/*~
	@rm -rf obj
	@rm -f *.dump


