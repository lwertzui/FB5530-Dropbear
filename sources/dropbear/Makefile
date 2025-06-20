# This Makefile is for Dropbear SSH Server and Client
# Makefile.  Generated from Makefile.in by configure.

# invocation:
# make PROGRAMS="dropbear dbclient scp" MULTI=1 SCPPROGRESS=1
#
# to make a multiple-program binary "dropbearmulti".
# This example will include dropbear, scp, dropbearkey, dropbearconvert, and
# dbclient functionality, and includes the progress-bar functionality in scp.

ifndef PROGRAMS
	PROGRAMS=dropbear dbclient dropbearkey dropbearconvert
endif

srcdir=./src
VPATH=./src

STATIC_LTC=libtomcrypt/libtomcrypt.a
STATIC_LTM=libtommath/libtommath.a

LIBTOM_LIBS=

ifeq (1, 1)
	LIBTOM_DEPS=$(STATIC_LTC) $(STATIC_LTM)
	LIBTOM_CLEAN=ltc-clean ltm-clean
	CPPFLAGS+=-I$(srcdir)/../libtomcrypt/src/headers/
	LIBTOM_LIBS=$(STATIC_LTC) $(STATIC_LTM)
endif

ifneq ($(wildcard ./localoptions.h),)
	CPPFLAGS+=-DLOCALOPTIONS_H_EXISTS
endif
ifneq ($(wildcard $(srcdir)/distrooptions.h),)
	CPPFLAGS+=-DDISTROOPTIONS_H_EXISTS
endif

OBJ_DIR=./obj
MAN_DIR=./manpages

_COMMONOBJS=dbutil.o buffer.o dbhelpers.o \
		dss.o bignum.o \
		signkey.o rsa.o dbrandom.o \
		queue.o \
		atomicio.o compat.o fake-rfc2553.o \
		ltc_prng.o ecc.o ecdsa.o sk-ecdsa.o crypto_desc.o \
		curve25519.o ed25519.o sk-ed25519.o \
		dbmalloc.o \
		gensignkey.o gendss.o genrsa.o gened25519.o
COMMONOBJS = $(patsubst %,$(OBJ_DIR)/%,$(_COMMONOBJS))

_SVROBJS=svr-kex.o svr-auth.o sshpty.o \
		svr-authpasswd.o svr-authpubkey.o svr-authpubkeyoptions.o svr-session.o svr-service.o \
		svr-chansession.o svr-runopts.o svr-agentfwd.o svr-main.o svr-x11fwd.o\
		svr-tcpfwd.o svr-authpam.o
SVROBJS = $(patsubst %,$(OBJ_DIR)/%,$(_SVROBJS))

_CLIOBJS=cli-main.o cli-auth.o cli-authpasswd.o cli-kex.o \
		cli-session.o cli-runopts.o cli-chansession.o \
		cli-authpubkey.o cli-tcpfwd.o cli-channel.o cli-authinteract.o \
		cli-agentfwd.o cli-readconf.o
CLIOBJS = $(patsubst %,$(OBJ_DIR)/%,$(_CLIOBJS))

_CLISVROBJS=common-session.o packet.o common-algo.o common-kex.o \
		common-channel.o common-chansession.o termcodes.o loginrec.o \
		tcp-accept.o listener.o process-packet.o dh_groups.o \
		common-runopts.o circbuffer.o list.o netio.o chachapoly.o gcm.o \
		kex-x25519.o kex-dh.o kex-ecdh.o kex-pqhybrid.o \
		sntrup761.o mlkem768.o
CLISVROBJS = $(patsubst %,$(OBJ_DIR)/%,$(_CLISVROBJS))

_KEYOBJS=dropbearkey.o
KEYOBJS = $(patsubst %,$(OBJ_DIR)/%,$(_KEYOBJS))

_CONVERTOBJS=dropbearconvert.o keyimport.o signkey_ossh.o
CONVERTOBJS = $(patsubst %,$(OBJ_DIR)/%,$(_CONVERTOBJS))

_SCPOBJS=scp.o progressmeter.o atomicio.o scpmisc.o compat.o
SCPOBJS = $(patsubst %,$(OBJ_DIR)/%,$(_SCPOBJS))

ifeq (0, 1)
	allobjs = $(COMMONOBJS) fuzz/fuzz-common.o  fuzz/fuzz-wrapfd.o $(CLISVROBJS) $(CLIOBJS) $(SVROBJS) -lcrypt
	allobjs:=$(subst $(OBJ_DIR)/svr-main.o, ,$(allobjs))
	allobjs:=$(subst $(OBJ_DIR)/cli-main.o, ,$(allobjs))

	dropbearobjs=$(allobjs) $(OBJ_DIR)/svr-main.o
	dbclientobjs=$(allobjs) $(OBJ_DIR)/cli-main.o
	dropbearkeyobjs=$(allobjs) $(KEYOBJS)
	dropbearconvertobjs=$(allobjs) $(CONVERTOBJS)
	# CXX only set when fuzzing
	CXX=
	FUZZ_CLEAN=fuzz-clean
else
	dropbearobjs=$(COMMONOBJS) $(CLISVROBJS) $(SVROBJS)
	dbclientobjs=$(COMMONOBJS) $(CLISVROBJS) $(CLIOBJS)
	dropbearkeyobjs=$(COMMONOBJS) $(KEYOBJS)
	dropbearconvertobjs=$(COMMONOBJS) $(CONVERTOBJS)
	scpobjs=$(SCPOBJS)
endif

ifeq (0, 1)
	# rdynamic makes all the global symbols of dropbear available to all the loaded shared libraries
	# this allow a plugin to reuse existing crypto/utilities like base64_decode/base64_encode without
	# the need to rewrite them.
	PLUGIN_LIBS=-ldl -rdynamic
else
	PLUGIN_LIBS=
endif

prefix=/usr/local
exec_prefix=${prefix}
datarootdir = ${prefix}/share
bindir=${exec_prefix}/bin
sbindir=${exec_prefix}/sbin
mandir=${datarootdir}/man

.DELETE_ON_ERROR:

CC=gcc
AR=ar
RANLIB=ranlib
STRIP=strip
INSTALL=/bin/install -c
CPPFLAGS+= -I. -I$(srcdir)
CFLAGS+=-mindirect-branch=thunk -mfunction-return=thunk -D_FORTIFY_SOURCE=2 -fstack-protector-strong -fPIE -Wundef -fno-strict-overflow -Wno-pointer-sign -Os -W -Wall 
LIBS+=-lz 
LDFLAGS=-Wl,-z,now -Wl,-z,relro -Wl,-pie 

EXEEXT=

STATIC=0

# whether we're building client, server, or both for the common objects.
# evilness so we detect 'dropbear' by itself as a word
ifneq (,$(strip $(foreach prog, $(PROGRAMS), $(findstring ZdropbearZ, Z$(prog)Z))))
	CPPFLAGS+= -DDROPBEAR_SERVER
endif
ifneq (,$(strip $(foreach prog, $(PROGRAMS), $(findstring ZdbclientZ, Z$(prog)Z))))
	CPPFLAGS+= -DDROPBEAR_CLIENT
endif

# these are exported so that libtomcrypt's makefile will use them
export CC
export CFLAGS
export CPPFLAGS
export RANLIB AR STRIP

ifeq ($(STATIC), 1)
	LDFLAGS+=-static
endif

ifeq ($(MULTI), 1)
	TARGETS=dropbearmulti$(EXEEXT)
else
	TARGETS=$(PROGRAMS)
endif

# for the scp progress meter. The -D doesn't affect anything else.
ifeq ($(SCPPROGRESS), 1)
	CPPFLAGS+=-DPROGRESS_METER
endif

all: $(TARGETS)

# for simplicity assume all source depends on all headers
HEADERS=$(wildcard $(srcdir)/*.h *.h) default_options_guard.h

$(OBJ_DIR):
	mkdir -p $@

$(OBJ_DIR)/%.o: $(srcdir)/%.c $(HEADERS) | $(OBJ_DIR)
	$(CC) $(CFLAGS) $(CPPFLAGS) $< -o $@ -c

fuzz/%.o: $(srcdir)/../fuzz/%.c $(HEADERS)
	$(CC) $(CFLAGS) $(CPPFLAGS) $< -o $@ -c

default_options_guard.h: $(srcdir)/default_options.h
	@echo Creating $@
	@printf "/*\n > > > Do not edit this file (default_options_guard.h) < < <\nGenerated from "$^"\nLocal customisation goes in localoptions.h\n*/\n\n" > $@.tmp
	@$(srcdir)/ifndef_wrapper.sh < $^ >> $@.tmp
	mv -v $@.tmp $@
	pwd
	ls -l $@

strip: $(TARGETS)
	$(STRIP) $(addsuffix $(EXEEXT), $(TARGETS))

install: $(addprefix inst_, $(TARGETS))

insmultidropbear: dropbearmulti$(EXEEXT)
	$(INSTALL) -d $(DESTDIR)$(sbindir)
	-rm -f $(DESTDIR)$(sbindir)/dropbear$(EXEEXT)
	-ln -s $(bindir)/dropbearmulti$(EXEEXT) $(DESTDIR)$(sbindir)/dropbear$(EXEEXT)
	$(INSTALL) -d $(DESTDIR)$(mandir)/man8
	$(INSTALL) -m 644 $(MAN_DIR)/dropbear.8  $(DESTDIR)$(mandir)/man8/dropbear.8

insmulti%: dropbearmulti$(EXEEXT)
	$(INSTALL) -d $(DESTDIR)$(bindir)
	-rm -f $(DESTDIR)$(bindir)/$*$(EXEEXT)
	-ln -s $(bindir)/dropbearmulti$(EXEEXT) $(DESTDIR)$(bindir)/$*$(EXEEXT)
	$(INSTALL) -d $(DESTDIR)$(mandir)/man1
	if test -e $(MAN_DIR)/$*.1; then $(INSTALL) -m 644 $(MAN_DIR)/$*.1 $(DESTDIR)$(mandir)/man1/$*.1; fi

# dropbear should go in sbin, so it needs a separate rule
inst_dropbear: dropbear
	$(INSTALL) -d $(DESTDIR)$(sbindir)
	$(INSTALL) dropbear$(EXEEXT) $(DESTDIR)$(sbindir)
	$(INSTALL) -d $(DESTDIR)$(mandir)/man8
	$(INSTALL) -m 644 $(MAN_DIR)/dropbear.8 $(DESTDIR)$(mandir)/man8/dropbear.8

inst_%: %
	$(INSTALL) -d $(DESTDIR)$(bindir)
	$(INSTALL) $*$(EXEEXT) $(DESTDIR)$(bindir)
	$(INSTALL) -d $(DESTDIR)$(mandir)/man1
	if test -e $(MAN_DIR)/$*.1; then $(INSTALL) -m 644 $(MAN_DIR)/$*.1 $(DESTDIR)$(mandir)/man1/$*.1; fi

inst_dropbearmulti: $(addprefix insmulti, $(PROGRAMS))

# for some reason the rule further down doesn't like $($@objs) as a prereq.
dropbear: $(dropbearobjs)
dbclient: $(dbclientobjs)
dropbearkey: $(dropbearkeyobjs)
dropbearconvert: $(dropbearconvertobjs)

dropbear: $(HEADERS) $(LIBTOM_DEPS) Makefile
	$(CC) $(LDFLAGS) -o $@$(EXEEXT) $($@objs) $(LIBTOM_LIBS) $(LIBS) -lcrypt $(PLUGIN_LIBS)

dbclient: $(HEADERS) $(LIBTOM_DEPS) Makefile
	$(CC) $(LDFLAGS) -o $@$(EXEEXT) $($@objs) $(LIBTOM_LIBS) $(LIBS)

dropbearkey dropbearconvert: $(HEADERS) $(LIBTOM_DEPS) Makefile
	$(CC) $(LDFLAGS) -o $@$(EXEEXT) $($@objs) $(LIBTOM_LIBS) $(LIBS)

# scp doesn't use the libs so is special.
scp: $(SCPOBJS)  $(HEADERS) Makefile
	$(CC) $(LDFLAGS) -o $@$(EXEEXT) $(SCPOBJS)


# multi-binary compilation.
MULTIOBJS=
ifeq ($(MULTI),1)
	MULTIOBJS=$(OBJ_DIR)/dbmulti.o $(sort $(foreach prog, $(PROGRAMS), $($(prog)objs)))
	CPPFLAGS+=$(addprefix -DDBMULTI_, $(PROGRAMS)) -DDROPBEAR_MULTI
endif

dropbearmulti$(EXEEXT): $(HEADERS) $(MULTIOBJS) $(LIBTOM_DEPS) Makefile
	$(CC) $(LDFLAGS) -o $@ $(MULTIOBJS) $(LIBTOM_LIBS) $(LIBS) -lcrypt

multibinary: dropbearmulti$(EXEEXT)

multilink: multibinary $(addprefix link, $(PROGRAMS))

link%:
	-rm -f $*$(EXEEXT)
	-ln -s dropbearmulti$(EXEEXT) $*$(EXEEXT)

$(STATIC_LTC): $(HEADERS)
	$(MAKE) -C libtomcrypt

$(STATIC_LTM): $(HEADERS)
	$(MAKE) -C libtommath

.PHONY : clean sizes thisclean distclean tidy ltc-clean ltm-clean lint check

ltc-clean:
	$(MAKE) -C libtomcrypt clean

ltm-clean:
	$(MAKE) -C libtommath clean

sizes: dropbear
	objdump -t dropbear|grep ".text"|cut -d "." -f 2|sort -rn

clean: $(LIBTOM_CLEAN) $(FUZZ_CLEAN) thisclean

thisclean:
	-rm -f dropbear$(EXEEXT) dbclient$(EXEEXT) dropbearkey$(EXEEXT) \
			dropbearconvert$(EXEEXT) scp$(EXEEXT) scp-progress$(EXEEXT) \
			dropbearmulti$(EXEEXT) *.o *.da *.bb *.bbg *.prof \
			$(OBJ_DIR)/*

distclean: clean tidy
	-rm -f src/config.h config.status config.log
	-rm -f Makefile test/Makefile
	-rm -f default_options_guard.h

tidy:
	-rm -f *~ *.gcov */*~

lint:
	cd $(srcdir); ./dropbear_lint.sh

check: lint
	make -C test

## Fuzzing targets

# list of fuzz targets
FUZZ_TARGETS=fuzzer-preauth fuzzer-pubkey fuzzer-verify fuzzer-preauth_nomaths \
	fuzzer-kexdh fuzzer-kexecdh fuzzer-kexcurve25519 fuzzer-client fuzzer-client_nomaths \
	fuzzer-postauth_nomaths fuzzer-cliconf \
	fuzzer-kexsntrup-srv fuzzer-kexsntrup-cli \
	fuzzer-kexmlkem-srv fuzzer-kexmlkem-cli

FUZZER_OPTIONS = $(addsuffix .options, $(FUZZ_TARGETS))
FUZZ_OBJS = $(addprefix fuzz/,$(addsuffix .o,$(FUZZ_TARGETS))) \
	fuzz/fuzz-sshpacketmutator.o

list-fuzz-targets:
	@echo $(FUZZ_TARGETS)

# fuzzers that don't use libfuzzer, just a standalone harness that feeds inputs
fuzzstandalone: FUZZLIB=fuzz/fuzz-harness.o
fuzzstandalone: fuzz/fuzz-harness.o fuzz-targets

# Build all the fuzzers. Usually like
#   make fuzz-targets FUZZLIB=-lFuzzer.a
# the library provides main(). Otherwise
#   make fuzzstandalone
# provides a main in fuzz-harness.c
fuzz-targets: $(FUZZ_TARGETS) $(FUZZER_OPTIONS)

$(FUZZ_TARGETS): $(LIBTOM_DEPS) $(allobjs) $(FUZZ_OBJS)
	$(CXX) $(CXXFLAGS) fuzz/$@.o $(LDFLAGS) $(allobjs) -o $@$(EXEEXT) $(LIBTOM_LIBS) $(LIBS) $(FUZZLIB) -lcrypt

# fuzzers that use the custom mutator - these expect a SSH network stream
MUTATOR_FUZZERS=fuzzer-client fuzzer-client_nomaths \
	fuzzer-preauth fuzzer-preauth_nomaths fuzzer-postauth_nomaths

# Skip custom mutators for -fsanitize-memory since libfuzzer doesn't initialise memory
# Pending fix for it https://github.com/google/oss-fuzz/issues/4605
ifeq (,$(findstring fsanitize=memory, $(CFLAGS)))
$(MUTATOR_FUZZERS): allobjs += fuzz/fuzz-sshpacketmutator.o
endif

fuzzer-%.options: Makefile
	echo "[libfuzzer]"               > $@
	echo "max_len = 50000"          >> $@

# run this to update hardcoded hostkeys for for fuzzing.
# hostkeys.c is checked in to hg.
fuzz-hostkeys:
	dropbearkey -t rsa -f keyr
	dropbearkey -t dss -f keyd
	dropbearkey -t ecdsa -size 256 -f keye
	dropbearkey -t ed25519 -f keyed25519
	echo > hostkeys.c
	/usr/bin/xxd -i -a keyr >> hostkeys.c
	/usr/bin/xxd -i -a keye >> hostkeys.c
	/usr/bin/xxd -i -a keyd >> hostkeys.c
	/usr/bin/xxd -i -a keyed25519 >> hostkeys.c

fuzz-clean:
	-rm -f fuzz/*.o $(FUZZ_TARGETS) $(FUZZER_OPTIONS)
