.PHONY: all
all: desktop binary

BINNAME ?= tipitaka_pali_reader
APP_PREFIX ?= github/tipitaka_pali_reader
.PHONY: binary
binary: $(BINNAME)
$(BINNAME):
	echo "#!/usr/bin/env bash" > $(BINNAME)
	echo "unset LD_LIBRARY_PATH" >> $(BINNAME)
	echo "cd /$(PREFIX)/lib/$(APP_PREFIX) && ./AppRun $$@" >> $(BINNAME)

DESKTOP_FILE ?= tipitaka_pali_reader.desktop
.PHONY: desktop
desktop: extract $(DESKTOP_FILE)
$(DESKTOP_FILE): squashfs-root/$(DESKTOP_FILE)
	cp squashfs-root/$(DESKTOP_FILE) .
	sed -i \
		"s/Exec=.*/Exec=\/$(subst /,\/,$(PREFIX))\/bin\/$(BINNAME)/" \
		$(DESKTOP_FILE)

APPIMAGE ?= tipitaka_pali_reader.AppImage
.PHONY: extract
extract: squashfs-root
squashfs-root: $(APPIMAGE)
	chmod +x $(APPIMAGE)
	./$(APPIMAGE) --appimage-extract

APPIMAGE_URL ?= https://github.com/bksubhuti/tipitaka-pali-reader/releases/download/2.3.5%2B55/tipitaka_pali_reader.AppImage
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