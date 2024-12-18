# Default mirrors
pkgdata_MIRRORS = default

# Mirrors in Asia (excl. Chinese_Mainland and Russia)
pkgdata_ASIA_MIRRORS = mirror.icst.buildtool

# Removed, renamed or moved mirrors
# Mirrors files are marked as conffiles and they are manually deleted
# in preinst step of package upgrade as conffiles removed in a newer
# version of the package are only deleted if package is purged and
# would otherwise be left as is after package upgrades and would still
# get used during mirror selection.
pkgdata_MIRRORS_REMOVED = \
asia/packages.nscdn.top \
china/mirrors.dgut.edu.cn \
china/mirrors.hit.edu.cn \
china/mirrors.njupt.edu.cn \
china/mirrors.scau.edu.cn \
china/mirror.iscas.ac.cn \
china/mirror.nyist.edu.cn \
china/mirror.sjtu.edu.cn \
china/mirrors.aliyun.com \
china/mirrors.bfsu.edu.cn \
china/mirrors.cqupt.edu.cn \
china/mirrors.nju.edu.cn \
china/mirrors.pku.edu.cn \
china/mirrors.qvq.net.cn \
china/mirrors.sau.edu.cn \
china/mirrors.sdu.edu.cn \
china/mirrors.sustech.edu.cn \
china/mirrors.tuna.tsinghua.edu.cn \
china/mirrors.ustc.edu.cn \
china/mirrors.zju.edu.cn \
chinese_mainland/mirrors.qvq.net.cn \
europe/cdn.lumito.net \
europe/mirror.termux.dv \
europe/termux.sahilister.in \
europe/termux.astra.in.ua \
north_america/packages.termux.dev \
oceania/mirrors.wale.id.au \
russia/mirror.surf \
south_america/mirrors.rda.run \
asia/mirrors.saswata.xyz \
asia/mirrors.omsinchan.ac.th

define install-mirror-rule
install-$1:
	$$(MKDIR_P) $$(DESTDIR)$$(sysconfdir)/termux/mirrors/$1
	for f in $$(pkgdata_$2_MIRRORS); do \
		$(INSTALL) -m 644 $(srcdir)/$1/$$$$f \
		$(DESTDIR)$(sysconfdir)/termux/mirrors/$1; \
	done
endef

define uninstall-mirror-rule
uninstall-$1:
	for f in $$(pkgdata_$2_MIRRORS); do \
		rm -f $(DESTDIR)$(sysconfdir)/termux/mirrors/$1/$$$$f; \
	done
	-rmdir $(DESTDIR)$(sysconfdir)/termux/mirrors/$1
endef

$(eval $(call install-mirror-rule,asia,ASIA))

$(eval $(call uninstall-mirror-rule,asia,ASIA))

install-default: $(pkgdata_MIRRORS)
	$(MKDIR_P) $(DESTDIR)$(sysconfdir)/termux/mirrors
	for f in $(pkgdata_MIRRORS); do \
		$(INSTALL) -m 644 $(srcdir)/$$f \
		$(DESTDIR)$(sysconfdir)/termux/mirrors; \
	done

uninstall-default:
	for f in $(pkgdata_MIRRORS); do \
		rm -f $(DESTDIR)$(sysconfdir)/termux/mirrors/$$f; \
	done
	-rmdir $(DESTDIR)$(sysconfdir)/termux/mirrors



uninstall-removed-mirrors:
	for f in $(pkgdata_MIRRORS_REMOVED); do \
		rm -f $(DESTDIR)$(sysconfdir)/termux/mirrors/$$f; \
	done



create-deb-control-files:
	sh -c "cd $$(dirname $(DESTDIR)$(sysconfdir)); find $$(basename $(DESTDIR)$(sysconfdir))/termux/mirrors -type f" >> ../conffiles;
	printf "# Delete removed mirrors\nfor f in %s; do\n    if test -f \"%s/etc/termux/mirrors/\$$f\"; then\n        echo \"Deleting removed mirror: \$$f\"; rm -f \"%s/etc/termux/mirrors/\$$f\";\n    fi\ndone" "$(pkgdata_MIRRORS_REMOVED)" "$(termux_prefix)" "$(termux_prefix)" >> ../preinst;



install-data-local: uninstall-removed-mirrors install-default install-asia

uninstall-local: uninstall-removed-mirrors uninstall-default uninstall-asia

EXTRA_DIST = $(pkgdata_MIRRORS) $(pkgdata_ASIA_MIRRORS)