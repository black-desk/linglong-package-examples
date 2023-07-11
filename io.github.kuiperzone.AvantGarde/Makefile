.PHONY: all
all: extract desktop binary

BINNAME ?= AvantGarde
APP_PREFIX ?= kuiperzone/AvantGarde
APPIMAGE ?= AvantGarde.x86_64.AppImage
.PHONY: extract
extract: squashfs-root
squashfs-root: $(APPIMAGE)
	chmod +x $(APPIMAGE)
	./$(APPIMAGE) --appimage-extract

.PHONY: binary
binary: $(BINNAME)
$(BINNAME):
	echo "#!/usr/bin/env bash" > $(BINNAME)
	echo "unset LD_LIBRARY_PATH" >> $(BINNAME)
	echo "cd $(PREFIX)/lib/$(APP_PREFIX) && ./AppRun $$@" >> $(BINNAME)

DESKTOP_FILE ?= zone.kuiper.AvantGarde.desktop
.PHONY: desktop
desktop:  $(DESKTOP_FILE)
$(DESKTOP_FILE):
	cp squashfs-root/$(DESKTOP_FILE) .
	sed -i \
		"s@Exec=.*@Exec=/$(PREFIX)/bin/$(BINNAME)@" \
		$(DESKTOP_FILE)



APPIMAGE_URL ?= https://github.com/kuiperzone/AvantGarde/releases/download/v1.2.1/AvantGarde.x86_64.AppImage
$(APPIMAGE):
	wget $(APPIMAGE_URL) -O $(APPIMAGE)

PREFIX ?= opt/$(APP_PREFIX)
DESTDIR ?=
.PHONY: install
install: binary desktop
	( \
		cd squashfs-root && \
		find -type f -exec \
			install -D "{}" "$(DESTDIR)/$(PREFIX)/lib/$(APP_PREFIX)/{}" \; && \
		find -type l -exec bash -c " \
			ln -s \$$(readlink {}) \
				\"$(DESTDIR)/$(PREFIX)/lib/$(APP_PREFIX)/{}\" \
		" \; \
	)
	install -D $(BINNAME) \
		$(DESTDIR)/$(PREFIX)/bin/$(BINNAME)
	install -D $(DESKTOP_FILE) \
		$(DESTDIR)/$(PREFIX)/share/applications/$(DESKTOP_FILE)

.PHONY: clean
clean:
	rm -r squashfs-root || true
	rm $(DESKTOP_FILE) || true
	rm $(BINNAME) || true
