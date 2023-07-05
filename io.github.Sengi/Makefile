.PHONY: all
all: desktop binary

BINNAME ?= Sengi
#  ?=表示如果没有被赋值过，那么就赋右边的值
APP_PREFIX ?= Sengi
.PHONY: binary
binary: $(BINNAME)
$(BINNAME):
	echo "#!/usr/bin/env bash" > $(BINNAME)
	echo "unset LD_LIBRARY_PATH" >> $(BINNAME)
	echo "cd $(PREFIX)/lib/$(APP_PREFIX) && ./AppRun $$@" >> $(BINNAME)

DESKTOP_FILE ?= sengi.desktop
.PHONY: desktop
desktop: extract $(DESKTOP_FILE)
$(DESKTOP_FILE):
	cp squashfs-root/$(DESKTOP_FILE) .
	sed -i \
		"s@Exec=.*@Exec=/$(PREFIX)/bin/$(BINNAME)@" \
		$(DESKTOP_FILE)

APPIMAGE ?= Sengi-1.2.0-linux.AppImage
.PHONY: extract
extract: squashfs-root
squashfs-root: $(APPIMAGE)
	chmod +x $(APPIMAGE)
	./$(APPIMAGE) --appimage-extract

APPIMAGE_URL ?= https://github.com/NicolasConstant/sengi/releases/download/1.2.0/Sengi-1.2.0-linux.AppImage
$(APPIMAGE):
	wget $(APPIMAGE_URL) -O $(APPIMAGE)

PREFIX ?= opt/$(APP_PREFIX)
DESTDIR ?=
.PHONY: install
#-exec: find命令对匹配的⽂文件执⾏行该参数所给出的shell命令
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
