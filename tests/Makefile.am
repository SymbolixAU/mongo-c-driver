noinst_PROGRAMS += test-load
noinst_PROGRAMS += test-secondary
noinst_PROGRAMS += test-replica-set
noinst_PROGRAMS += test-sharded-cluster
noinst_PROGRAMS += test-libmongoc
if ENABLE_SSL
noinst_PROGRAMS += test-replica-set-ssl
endif


TEST_PROGS = test-libmongoc


TEST_CFLAGS =

if OS_SOLARIS
TEST_CFLAGS += -D_REENTRANT
endif

TEST_CFLAGS += \
	-Wno-deprecated-declarations \
	-DMONGOC_COMPILATION \
	$(BSON_CFLAGS) \
	$(SSL_CFLAGS) \
	$(SASL_CFLAGS) \
	-I$(top_srcdir)/src/mongoc \
	-I$(top_builddir)/src/mongoc

TEST_LIBS = \
	libmongoc-priv.la \
	$(PTHREAD_LIBS) \
	$(SHM_LIB) \
	$(SASL_LIBS) \
	$(SSL_LIBS)
if EXPLICIT_LIBS
TEST_LIBS += $(BSON_LIBS)
endif
if OS_WIN32
TEST_LIBS += -lshlwapi
endif

test_load_SOURCES = \
	tests/test-load.c \
	tests/mongoc-tests.c
test_load_CFLAGS = $(TEST_CFLAGS)
test_load_LDADD = $(TEST_LIBS)


test_secondary_SOURCES = \
	tests/test-secondary.c \
	tests/mongoc-tests.c
test_secondary_CFLAGS = $(TEST_CFLAGS)
test_secondary_LDADD = $(TEST_LIBS)

test_replica_set_SOURCES = \
	tests/test-replica-set.c \
	tests/ha-test.c \
	tests/ha-test.h \
	tests/mongoc-tests.c
test_replica_set_CFLAGS = $(TEST_CFLAGS)
test_replica_set_LDADD = $(TEST_LIBS)


test_replica_set_ssl_SOURCES = \
	tests/test-replica-set-ssl.c \
	tests/ha-test.c \
	tests/ha-test.h \
	tests/mongoc-tests.c
test_replica_set_ssl_CFLAGS = $(TEST_CFLAGS)
test_replica_set_ssl_LDADD = $(TEST_LIBS)


test_libmongoc_SOURCES = \
	tests/debug-stream.c \
	tests/json-test.c \
	tests/json-test.h \
	tests/mock_server/future.c \
	tests/mock_server/future.h \
	tests/mock_server/future-functions.c \
	tests/mock_server/future-functions.h \
	tests/mock_server/future-value.c \
	tests/mock_server/future-value.h \
	tests/mock_server/mock-server.c \
	tests/mock_server/mock-server.h \
	tests/mock_server/mock-rs.c \
	tests/mock_server/mock-rs.h \
	tests/mock_server/request.c \
	tests/mock_server/request.h \
	tests/mock_server/sync-queue.c \
	tests/mock_server/sync-queue.h \
	tests/test-libmongoc.c \
	tests/test-bulk.c \
	tests/test-conveniences.c \
	tests/test-conveniences.h \
	tests/test-mongoc-array.c \
	tests/test-mongoc-async.c \
	tests/test-mongoc-buffer.c \
	tests/test-mongoc-client.c \
	tests/test-mongoc-client-pool.c \
	tests/test-mongoc-cluster.c \
	tests/test-mongoc-collection.c \
	tests/test-mongoc-collection-find.c \
	tests/test-mongoc-command-monitoring.c \
	tests/test-mongoc-cursor.c \
	tests/test-mongoc-database.c \
	tests/test-mongoc-error.c \
	tests/test-mongoc-exhaust.c \
	tests/test-mongoc-find-and-modify.c \
	tests/test-mongoc-gridfs.c \
	tests/test-mongoc-gridfs-file-page.c \
	tests/test-mongoc-log.c \
	tests/test-mongoc-list.c \
	tests/test-mongoc-matcher.c \
	tests/test-mongoc-queue.c \
	tests/test-mongoc-read-prefs.c \
	tests/test-mongoc-rpc.c \
	tests/test-mongoc-socket.c \
	tests/test-mongoc-sdam.c \
	tests/test-mongoc-server-selection.c \
	tests/test-mongoc-server-selection-errors.c \
	tests/test-mongoc-set.c \
	tests/test-mongoc-stream.c \
	tests/test-mongoc-thread.c \
	tests/test-mongoc-topology-reconcile.c \
	tests/test-mongoc-topology-scanner.c \
	tests/test-mongoc-topology.c \
	tests/test-mongoc-uri.c \
	tests/test-mongoc-usleep.c \
	tests/test-mongoc-util.c \
	tests/test-mongoc-version.c \
	tests/test-mongoc-write-concern.c \
	tests/test-libmongoc.h \
	tests/test-sasl.c \
	tests/test-write-commands.c \
	tests/TestSuite.c \
	tests/TestSuite.h
if ENABLE_SSL
test_libmongoc_SOURCES += \
	tests/test-x509.c \
	tests/test-mongoc-stream-tls.c \
	tests/test-mongoc-stream-tls-error.c \
	tests/ssl-test.c \
	tests/ssl-test.h
endif
test_libmongoc_CFLAGS = $(TEST_CFLAGS)
test_libmongoc_LDADD = $(TEST_LIBS)


test_sharded_cluster_SOURCES = \
	tests/test-sharded-cluster.c \
	tests/ha-test.c \
	tests/ha-test.h \
	tests/mongoc-tests.c
test_sharded_cluster_CFLAGS = $(TEST_CFLAGS)
test_sharded_cluster_LDADD = $(TEST_LIBS)

check: test abicheck

TEST_ARGS = "--no-fork"

test: $(TEST_PROGS)
	@ for TEST_PROG in $(TEST_PROGS) ; do \
		./$$TEST_PROG $(TEST_ARGS) -F test-results.json; \
	done

valgrind: $(TEST_PROGS)
	MONGOC_TEST_VALGRIND=on $(LIBTOOL) --mode=execute valgrind --leak-check=full --suppressions=$(srcdir)/valgrind.suppressions ./test-libmongoc $(TEST_ARGS)

if OS_LINUX
abicheck:
	@ $(srcdir)/tests/abicheck.sh "$(srcdir)/src/libmongoc.symbols"
else
abicheck:
endif

# TODO: run tests that do not need to talk to a server
local-check:

DISTCLEANFILES += \
	test-results.json

.PHONY: valgrind debug

EXTRA_DIST += \
	tests/abicheck.sh \
	tests/binary/delete1.dat \
	tests/binary/empty.dat \
	tests/binary/get_more1.dat \
	tests/binary/gridfs.dat \
	tests/binary/gridfs-large.dat \
	tests/binary/insert1.dat \
	tests/binary/kill_cursors1.dat \
	tests/binary/msg1.dat \
	tests/binary/query1.dat \
	tests/binary/query2.dat \
	tests/binary/reply1.dat \
	tests/binary/reply2.dat \
	tests/binary/update1.dat \
	tests/mock_server/future-value.h \
	tests/mock_server/future.c \
	tests/mock_server/future.h \
	tests/mock_server/mock-server.c \
	tests/mock_server/mock-server.h \
	tests/mock_server/mock-rs.c \
	tests/mock_server/mock-rs.h \
	tests/mock_server/request.c \
	tests/mock_server/request.h \
	tests/mock_server/sync-queue.c \
	tests/mock_server/sync-queue.h \
	tests/mongoc-tests.c \
	tests/mongoc-tests.h \
	tests/x509gen/7750279a.0 \
	$(wildcard tests/x509gen/*.pem) \
	$(wildcard tests/json/command_monitoring/*.json) \
	$(wildcard tests/json/server_discovery_and_monitoring/*/*.json) \
	$(wildcard tests/json/server_selection/rtt/*.json) \
	$(wildcard tests/json/server_selection/server_selection/*/*/*.json)

if OS_DARWIN
DEBUGGER = lldb --
else
DEBUGGER = gdb --args
endif

debug: test-libmongoc
	$(LIBTOOL) --mode=execute $(DEBUGGER) test-libmongoc $(TEST_ARGS)
