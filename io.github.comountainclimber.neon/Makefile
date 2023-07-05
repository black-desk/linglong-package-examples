.PHONY: all
all: desktop binary

BINNAME ?= neon
APP_PREFIX ?= comountainclimber/neon
.PHONY: binary
binary: $(BINNAME)
$(BINNAME):
	echo "#!/usr/bin/env bash" > $(BINNAME)
	echo "cd /$(PREFIX)/lib/$(APP_PREFIX) && ./AppRun $$@" >> $(BINNAME)

DESKTOP_FILE ?= neon.desktop
.PHONY: desktop
desktop: extract $(DESKTOP_FILE)
$(DESKTOP_FILE):
	cp squashfs-root/$(DESKTOP_FILE) .
	sed -i \
		"s/Exec=.*/Exec=\/$(subst /,\/,$(PREFIX))\/bin\/$(BINNAME)/" \
		$(DESKTOP_FILE)

APPIMAGE ?= Neon.2.17.10.AppImage
.PHONY: extract
extract: squashfs-root
squashfs-root: $(APPIMAGE)
	chmod +x $(APPIMAGE)
	./$(APPIMAGE) --appimage-extract

APPIMAGE_URL ?= https://github.com/CityOfZion/neon-wallet/releases/download/v2.17.10/Neon.2.17.10.AppImage
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
