AUTOMAKE_OPTIONS = subdir-objects

SUBDIRS = . testutils tests benchmarks

dhcp_data_dir = @localstatedir@/@PACKAGE@
kea_lfc_location = @prefix@/sbin/kea-lfc

AM_CPPFLAGS = -I$(top_builddir)/src/lib -I$(top_srcdir)/src/lib -DDHCP_DATA_DIR="\"$(dhcp_data_dir)\""
AM_CPPFLAGS += -DTOP_BUILDDIR="\"$(top_builddir)\""
# Set location of the kea-lfc binary.
AM_CPPFLAGS += -DKEA_LFC_EXECUTABLE="\"$(kea_lfc_location)\""
AM_CPPFLAGS += $(BOOST_INCLUDES)
if HAVE_MYSQL
AM_CPPFLAGS += $(MYSQL_CPPFLAGS)
endif
if HAVE_PGSQL
AM_CPPFLAGS += $(PGSQL_CPPFLAGS)
endif
if HAVE_CQL
AM_CPPFLAGS += $(CQL_CPPFLAGS)
endif

AM_CXXFLAGS = $(KEA_CXXFLAGS)

# The files in the subfolder must be explicitly specified here so
# as they are copied to the distribution. The other option would
# be to specify a whole 'parsers' folder here but that would also
# copy all other files, e.g. gitignore, .git etc.
# Whenever new file is added to the parsers folder, it must be
# added here.
EXTRA_DIST =
EXTRA_DIST += parsers/client_class_def_parser.cc
EXTRA_DIST += parsers/client_class_def_parser.h
EXTRA_DIST += parsers/dbaccess_parser.cc
EXTRA_DIST += parsers/dbaccess_parser.h
EXTRA_DIST += parsers/dhcp_parsers.cc
EXTRA_DIST += parsers/dhcp_parsers.h
EXTRA_DIST += parsers/expiration_config_parser.cc
EXTRA_DIST += parsers/expiration_config_parser.h
EXTRA_DIST += parsers/host_reservation_parser.cc
EXTRA_DIST += parsers/host_reservation_parser.h
EXTRA_DIST += parsers/host_reservations_list_parser.h
EXTRA_DIST += parsers/ifaces_config_parser.cc
EXTRA_DIST += parsers/ifaces_config_parser.h
EXTRA_DIST += parsers/option_data_parser.h
EXTRA_DIST += parsers/sanity_checks_parser.cc
EXTRA_DIST += parsers/sanity_checks_parser.h
EXTRA_DIST += parsers/simple_parser4.cc
EXTRA_DIST += parsers/simple_parser4.h
EXTRA_DIST += parsers/simple_parser6.cc
EXTRA_DIST += parsers/simple_parser6.h

# Devel guide diagrams
EXTRA_DIST += images/pgsql_host_data_source.svg

# Define rule to build logging source files from message file
alloc_engine_messages.h alloc_engine_messages.cc dhcpsrv_messages.h \
dhcpsrv_messages.cc hosts_messages.h hosts_messages.cc: s-messages

s-messages: alloc_engine_messages.mes dhcpsrv_messages.mes hosts_messages.mes
	$(top_builddir)/src/lib/log/compiler/kea-msg-compiler $(top_srcdir)/src/lib/dhcpsrv/alloc_engine_messages.mes
	touch $@
	$(top_builddir)/src/lib/log/compiler/kea-msg-compiler $(top_srcdir)/src/lib/dhcpsrv/dhcpsrv_messages.mes
	touch $@
	$(top_builddir)/src/lib/log/compiler/kea-msg-compiler $(top_srcdir)/src/lib/dhcpsrv/hosts_messages.mes
	touch $@

# Tell Automake that the {alloc_engine,dhcpsrv,hosts}_messages.{cc,h} source files
# are created in the build process, so it must create these before doing anything
# else. Although they are a dependency of the library (so will be created from the
# message file anyway), there is no guarantee as to exactly _when_ in the build
# they will be created.  As the .h file is included in other sources file (so
# must be present when they are compiled), the safest option is to create it
# first.
BUILT_SOURCES = alloc_engine_messages.h alloc_engine_messages.cc
BUILT_SOURCES += dhcpsrv_messages.h dhcpsrv_messages.cc
BUILT_SOURCES += hosts_messages.h hosts_messages.cc

# Make sure the generated files are deleted in a "clean" operation
CLEANFILES = *.gcno *.gcda
CLEANFILES += alloc_engine_messages.h alloc_engine_messages.cc
CLEANFILES += dhcpsrv_messages.h dhcpsrv_messages.cc
CLEANFILES += hosts_messages.h hosts_messages.cc s-messages
# Remove CSV files created by the CSVLeaseFile6 and CSVLeaseFile4 unit tests.
CLEANFILES += *.csv

lib_LTLIBRARIES = libkea-dhcpsrv.la
libkea_dhcpsrv_la_SOURCES  =
libkea_dhcpsrv_la_SOURCES += addr_utilities.cc addr_utilities.h
libkea_dhcpsrv_la_SOURCES += alloc_engine.cc alloc_engine.h
libkea_dhcpsrv_la_SOURCES += alloc_engine_log.cc alloc_engine_log.h
libkea_dhcpsrv_la_SOURCES += assignable_network.h
libkea_dhcpsrv_la_SOURCES += base_host_data_source.h
libkea_dhcpsrv_la_SOURCES += cache_host_data_source.h
libkea_dhcpsrv_la_SOURCES += callout_handle_store.h
libkea_dhcpsrv_la_SOURCES += cfg_4o6.cc cfg_4o6.h
libkea_dhcpsrv_la_SOURCES += cfg_consistency.cc cfg_consistency.h
libkea_dhcpsrv_la_SOURCES += cfg_db_access.cc cfg_db_access.h
libkea_dhcpsrv_la_SOURCES += cfg_duid.cc cfg_duid.h
libkea_dhcpsrv_la_SOURCES += cfg_hosts.cc cfg_hosts.h
libkea_dhcpsrv_la_SOURCES += cfg_hosts_util.cc cfg_hosts_util.h
libkea_dhcpsrv_la_SOURCES += cfg_iface.cc cfg_iface.h
libkea_dhcpsrv_la_SOURCES += cfg_expiration.cc cfg_expiration.h
libkea_dhcpsrv_la_SOURCES += cfg_host_operations.cc cfg_host_operations.h
libkea_dhcpsrv_la_SOURCES += cfg_option.cc cfg_option.h
libkea_dhcpsrv_la_SOURCES += cfg_option_def.cc cfg_option_def.h
libkea_dhcpsrv_la_SOURCES += cfg_rsoo.cc cfg_rsoo.h
libkea_dhcpsrv_la_SOURCES += cfg_shared_networks.cc cfg_shared_networks.h
libkea_dhcpsrv_la_SOURCES += cfg_subnets4.cc cfg_subnets4.h
libkea_dhcpsrv_la_SOURCES += cfg_subnets6.cc cfg_subnets6.h
libkea_dhcpsrv_la_SOURCES += cfg_mac_source.cc cfg_mac_source.h
libkea_dhcpsrv_la_SOURCES += cfgmgr.cc cfgmgr.h
libkea_dhcpsrv_la_SOURCES += client_class_def.cc client_class_def.h
libkea_dhcpsrv_la_SOURCES += csv_lease_file4.cc csv_lease_file4.h
libkea_dhcpsrv_la_SOURCES += csv_lease_file6.cc csv_lease_file6.h
libkea_dhcpsrv_la_SOURCES += d2_client_cfg.cc d2_client_cfg.h
libkea_dhcpsrv_la_SOURCES += d2_client_mgr.cc d2_client_mgr.h
libkea_dhcpsrv_la_SOURCES += daemon.cc daemon.h
libkea_dhcpsrv_la_SOURCES += database_connection.cc database_connection.h
libkea_dhcpsrv_la_SOURCES += db_exceptions.h
libkea_dhcpsrv_la_SOURCES += db_log.cc db_log.h
libkea_dhcpsrv_la_SOURCES += db_type.h
libkea_dhcpsrv_la_SOURCES += dhcp4o6_ipc.cc dhcp4o6_ipc.h
libkea_dhcpsrv_la_SOURCES += dhcpsrv_log.cc dhcpsrv_log.h
libkea_dhcpsrv_la_SOURCES += dhcpsrv_db_log.cc dhcpsrv_db_log.h
libkea_dhcpsrv_la_SOURCES += host.cc host.h
libkea_dhcpsrv_la_SOURCES += host_container.h
libkea_dhcpsrv_la_SOURCES += host_data_source_factory.cc host_data_source_factory.h
libkea_dhcpsrv_la_SOURCES += host_mgr.cc host_mgr.h
libkea_dhcpsrv_la_SOURCES += hosts_log.cc hosts_log.h
libkea_dhcpsrv_la_SOURCES += key_from_key.h
libkea_dhcpsrv_la_SOURCES += lease.cc lease.h
libkea_dhcpsrv_la_SOURCES += lease_file_loader.h
libkea_dhcpsrv_la_SOURCES += lease_file_stats.h
libkea_dhcpsrv_la_SOURCES += lease_mgr.cc lease_mgr.h
libkea_dhcpsrv_la_SOURCES += lease_mgr_factory.cc lease_mgr_factory.h
libkea_dhcpsrv_la_SOURCES += logging.cc logging.h
libkea_dhcpsrv_la_SOURCES += logging_info.cc logging_info.h
libkea_dhcpsrv_la_SOURCES += memfile_lease_mgr.cc memfile_lease_mgr.h
libkea_dhcpsrv_la_SOURCES += memfile_lease_storage.h
libkea_dhcpsrv_la_SOURCES += sql_common.h

if HAVE_MYSQL
libkea_dhcpsrv_la_SOURCES += mysql_lease_mgr.cc mysql_lease_mgr.h
libkea_dhcpsrv_la_SOURCES += mysql_connection.cc mysql_connection.h
libkea_dhcpsrv_la_SOURCES += mysql_host_data_source.cc mysql_host_data_source.h
endif

libkea_dhcpsrv_la_SOURCES += ncr_generator.cc ncr_generator.h
libkea_dhcpsrv_la_SOURCES += network.cc network.h
libkea_dhcpsrv_la_SOURCES += network_state.cc network_state.h

if HAVE_PGSQL
libkea_dhcpsrv_la_SOURCES += pgsql_connection.cc pgsql_connection.h
libkea_dhcpsrv_la_SOURCES += pgsql_exchange.cc pgsql_exchange.h
libkea_dhcpsrv_la_SOURCES += pgsql_host_data_source.cc pgsql_host_data_source.h
libkea_dhcpsrv_la_SOURCES += pgsql_lease_mgr.cc pgsql_lease_mgr.h
endif

if HAVE_CQL
libkea_dhcpsrv_la_SOURCES += cql_connection.cc cql_connection.h
libkea_dhcpsrv_la_SOURCES += cql_exchange.cc cql_exchange.h
libkea_dhcpsrv_la_SOURCES += cql_host_data_source.cc cql_host_data_source.h
libkea_dhcpsrv_la_SOURCES += cql_lease_mgr.cc cql_lease_mgr.h
endif

libkea_dhcpsrv_la_SOURCES += pool.cc pool.h
libkea_dhcpsrv_la_SOURCES += sanity_checker.cc sanity_checker.h
libkea_dhcpsrv_la_SOURCES += shared_network.cc shared_network.h
libkea_dhcpsrv_la_SOURCES += srv_config.cc srv_config.h
libkea_dhcpsrv_la_SOURCES += subnet.cc subnet.h
libkea_dhcpsrv_la_SOURCES += subnet_id.h
libkea_dhcpsrv_la_SOURCES += subnet_selector.h
libkea_dhcpsrv_la_SOURCES += timer_mgr.cc timer_mgr.h
libkea_dhcpsrv_la_SOURCES += triplet.h
libkea_dhcpsrv_la_SOURCES += utils.h
libkea_dhcpsrv_la_SOURCES += writable_host_data_source.h

# Configuration parsers
libkea_dhcpsrv_la_SOURCES += parsers/client_class_def_parser.cc
libkea_dhcpsrv_la_SOURCES += parsers/client_class_def_parser.h
libkea_dhcpsrv_la_SOURCES += parsers/dbaccess_parser.cc
libkea_dhcpsrv_la_SOURCES += parsers/dbaccess_parser.h
libkea_dhcpsrv_la_SOURCES += parsers/dhcp_parsers.cc
libkea_dhcpsrv_la_SOURCES += parsers/dhcp_parsers.h
libkea_dhcpsrv_la_SOURCES += parsers/duid_config_parser.cc
libkea_dhcpsrv_la_SOURCES += parsers/duid_config_parser.h
libkea_dhcpsrv_la_SOURCES += parsers/expiration_config_parser.cc
libkea_dhcpsrv_la_SOURCES += parsers/expiration_config_parser.h
libkea_dhcpsrv_la_SOURCES += parsers/host_reservation_parser.cc
libkea_dhcpsrv_la_SOURCES += parsers/host_reservation_parser.h
libkea_dhcpsrv_la_SOURCES += parsers/host_reservations_list_parser.h
libkea_dhcpsrv_la_SOURCES += parsers/ifaces_config_parser.cc
libkea_dhcpsrv_la_SOURCES += parsers/ifaces_config_parser.h
libkea_dhcpsrv_la_SOURCES += parsers/option_data_parser.cc
libkea_dhcpsrv_la_SOURCES += parsers/option_data_parser.h
libkea_dhcpsrv_la_SOURCES += parsers/sanity_checks_parser.cc
libkea_dhcpsrv_la_SOURCES += parsers/sanity_checks_parser.h
libkea_dhcpsrv_la_SOURCES += parsers/shared_network_parser.cc
libkea_dhcpsrv_la_SOURCES += parsers/shared_network_parser.h
libkea_dhcpsrv_la_SOURCES += parsers/shared_networks_list_parser.h
libkea_dhcpsrv_la_SOURCES += parsers/simple_parser4.cc
libkea_dhcpsrv_la_SOURCES += parsers/simple_parser4.h
libkea_dhcpsrv_la_SOURCES += parsers/simple_parser6.cc
libkea_dhcpsrv_la_SOURCES += parsers/simple_parser6.h

nodist_libkea_dhcpsrv_la_SOURCES = alloc_engine_messages.h
nodist_libkea_dhcpsrv_la_SOURCES += alloc_engine_messages.cc
nodist_libkea_dhcpsrv_la_SOURCES += dhcpsrv_messages.h dhcpsrv_messages.cc
nodist_libkea_dhcpsrv_la_SOURCES += hosts_messages.h hosts_messages.cc

libkea_dhcpsrv_la_CXXFLAGS = $(AM_CXXFLAGS)
libkea_dhcpsrv_la_CPPFLAGS = $(AM_CPPFLAGS)
libkea_dhcpsrv_la_LIBADD  = $(top_builddir)/src/lib/eval/libkea-eval.la
libkea_dhcpsrv_la_LIBADD  += $(top_builddir)/src/lib/dhcp_ddns/libkea-dhcp_ddns.la
libkea_dhcpsrv_la_LIBADD  += $(top_builddir)/src/lib/stats/libkea-stats.la
libkea_dhcpsrv_la_LIBADD  += $(top_builddir)/src/lib/config/libkea-cfgclient.la
libkea_dhcpsrv_la_LIBADD  += $(top_builddir)/src/lib/dhcp/libkea-dhcp++.la
libkea_dhcpsrv_la_LIBADD  += $(top_builddir)/src/lib/asiolink/libkea-asiolink.la
libkea_dhcpsrv_la_LIBADD  += $(top_builddir)/src/lib/cc/libkea-cc.la
libkea_dhcpsrv_la_LIBADD  += $(top_builddir)/src/lib/dns/libkea-dns++.la
libkea_dhcpsrv_la_LIBADD  += $(top_builddir)/src/lib/cryptolink/libkea-cryptolink.la
libkea_dhcpsrv_la_LIBADD  += $(top_builddir)/src/lib/hooks/libkea-hooks.la
libkea_dhcpsrv_la_LIBADD  += $(top_builddir)/src/lib/log/libkea-log.la
libkea_dhcpsrv_la_LIBADD  += $(top_builddir)/src/lib/util/threads/libkea-threads.la
libkea_dhcpsrv_la_LIBADD  += $(top_builddir)/src/lib/util/libkea-util.la
libkea_dhcpsrv_la_LIBADD  += $(top_builddir)/src/lib/exceptions/libkea-exceptions.la
libkea_dhcpsrv_la_LIBADD  += $(LOG4CPLUS_LIBS) $(CRYPTO_LIBS) $(BOOST_LIBS)

libkea_dhcpsrv_la_LDFLAGS  = -no-undefined -version-info 11:0:0
libkea_dhcpsrv_la_LDFLAGS += $(CRYPTO_LDFLAGS)
if HAVE_MYSQL
libkea_dhcpsrv_la_LDFLAGS += $(MYSQL_LIBS)
endif
if HAVE_PGSQL
libkea_dhcpsrv_la_LDFLAGS += $(PGSQL_LIBS)
endif
if HAVE_CQL
libkea_dhcpsrv_la_LDFLAGS += $(CQL_LIBS)
endif

# The message file should be in the distribution
EXTRA_DIST += alloc_engine_messages.mes
EXTRA_DIST += dhcpsrv_messages.mes
EXTRA_DIST += hosts_messages.mes

# Distribute backend documentation
# Database schema creation script moved to src/bin/admin
EXTRA_DIST += database_backends.dox libdhcpsrv.dox

# Specify the headers for copying into the installation directory tree.
libkea_dhcpsrv_includedir = $(pkgincludedir)/dhcpsrv
libkea_dhcpsrv_include_HEADERS = \
	addr_utilities.h \
	alloc_engine.h \
	alloc_engine_log.h \
	assignable_network.h \
	base_host_data_source.h \
	cache_host_data_source.h \
	callout_handle_store.h \
	cfg_4o6.h \
	cfg_db_access.h \
	cfg_duid.h \
	cfg_expiration.h \
	cfg_host_operations.h \
	cfg_hosts.h \
	cfg_hosts_util.h \
	cfg_iface.h \
	cfg_mac_source.h \
	cfg_option.h \
	cfg_option_def.h \
	cfg_rsoo.h \
	cfg_shared_networks.h \
	cfg_subnets4.h \
	cfg_subnets6.h \
	cfgmgr.h \
	client_class_def.h \
	csv_lease_file4.h \
	csv_lease_file6.h \
	d2_client_cfg.h \
	d2_client_mgr.h \
	daemon.h \
	database_connection.h \
	db_exceptions.h \
	db_log.h \
	db_type.h \
	dhcp4o6_ipc.h \
	dhcpsrv_db_log.h \
	dhcpsrv_log.h \
	host.h \
	host_container.h \
	host_data_source_factory.h \
	host_mgr.h \
	hosts_log.h \
	key_from_key.h \
	lease.h \
	lease_file_loader.h \
	lease_file_stats.h \
	lease_mgr.h \
	lease_mgr_factory.h \
	logging.h \
	logging_info.h \
	memfile_lease_mgr.h \
	memfile_lease_storage.h \
	ncr_generator.h \
	network.h \
	network_state.h \
	pool.h \
	shared_network.h \
	sql_common.h \
	srv_config.h \
	subnet.h \
	subnet_id.h \
	subnet_selector.h \
	timer_mgr.h \
	triplet.h \
	utils.h \
	writable_host_data_source.h

if HAVE_CQL
libkea_dhcpsrv_include_HEADERS += \
	cql_connection.h \
	cql_exchange.h \
	cql_host_data_source.h \
	cql_lease_mgr.h
endif

if HAVE_MYSQL
libkea_dhcpsrv_include_HEADERS += \
	mysql_connection.h \
	mysql_host_data_source.h \
	mysql_lease_mgr.h
endif

if HAVE_PGSQL
libkea_dhcpsrv_include_HEADERS += \
	pgsql_connection.h \
	pgsql_exchange.h \
	pgsql_host_data_source.h \
	pgsql_lease_mgr.h
endif

# Specify parsers' headers for copying into installation directory tree.
libkea_dhcpsrv_parsers_includedir = $(pkgincludedir)/dhcpsrv/parsers
libkea_dhcpsrv_parsers_include_HEADERS = \
	parsers/client_class_def_parser.h \
	parsers/dbaccess_parser.h \
	parsers/dhcp_parsers.h \
	parsers/duid_config_parser.h \
	parsers/expiration_config_parser.h \
	parsers/host_reservation_parser.h \
	parsers/host_reservations_list_parser.h \
	parsers/ifaces_config_parser.h \
	parsers/option_data_parser.h \
	parsers/shared_network_parser.h \
	parsers/shared_networks_list_parser.h \
	parsers/simple_parser4.h \
	parsers/simple_parser6.h


install-data-local:
	$(mkinstalldirs) $(DESTDIR)$(dhcp_data_dir)
