include ../common.mak

CFLAGS = -Os -Wall $(EXTRACFLAGS)
CFLAGS += -I$(SRCBASE)/include -I$(TOP)/$(OPENSSLDIR)/include $(if $(TCONFIG_BCMARM),-I$(TOP)/openvpn/include,$(if $(TCONFIG_OPTIMIZE_SIZE_MORE),-I$(TOP)/openvpn-2.4/include,-I$(TOP)/openvpn-2.5/include))

ifeq ($(TCONFIG_BCMARM),y)
LDFLAGS = -L$(TOP)/nvram${BCMEX} -lnvram -L$(TOP)/$(OPENSSLDIR) -L$(TOP)/openvpn
else
LDFLAGS = -L$(TOP)/nvram -lnvram -L$(TOP)/$(OPENSSLDIR) $(if $(TCONFIG_OPTIMIZE_SIZE_MORE),-L$(TOP)/openvpn-2.4,-L$(TOP)/openvpn-2.5)
endif

ifeq ($(TCONFIG_BCMARM),y)
LIBS += -lgcc_s
endif

OBJS = openvpn_plugin_auth_nvram.o

all: openvpn_plugin_auth_nvram.so

openvpn_plugin_auth_nvram.so: $(OBJS)
	@echo " [openvpn_plugin_auth_nvram] CC -o $@"
	@$(CC) -shared $(LDFLAGS) -o $@ $(OBJS) $(LDFLAGS)

	$(SIZECHECK)
	$(CPTMP)

install: all
	install -D openvpn_plugin_auth_nvram.so $(INSTALLDIR)/lib/openvpn_plugin_auth_nvram.so
	$(STRIP) $(INSTALLDIR)/lib/openvpn_plugin_auth_nvram.so
	chmod 0500 $(INSTALLDIR)/lib/openvpn_plugin_auth_nvram.so

clean:
	rm -f openvpn_plugin_auth_nvram.so .*.depend *.o
	
%.o: %.c .%.depend
	@echo " [openvpn_plugin_auth_nvram] CC $@"
	@$(CC) $(CFLAGS) -c $<
	
.depend: $(OBJS:%.o=%.c)
	@$(CC) $(CFLAGS) -M $^ > .depend

.%.depend: %.c
	@$(CC) $(CFLAGS) -M $< > $@

-include $(OBJS:%.o=.%.depend)
