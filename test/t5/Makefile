include ../support/include.mk

.PHONY: all test debug clean

#
all: conf setup app_test.beam emptytest.beam streamtest.beam smalltest.beam bigtest.beam
	@echo "all ok"


# invoke as
# TEST=test3 make test
# or just make test to run all

ULIMIT = 768

test: all start
	touch ../../www/0.txt
	dd if=/dev/zero of=../../www/1000.txt bs=1024 count=1000 >/dev/null 2>&1
	dd if=/dev/zero of=../../www/3000.txt bs=1024 count=3000 >/dev/null 2>&1
	dd if=/dev/zero of=../../www/10000.txt bs=1024 count=10000 >/dev/null 2>&1
	gzip -c ../../www/10000.txt > ../../www/10000.txt.gz
	cp ../../www/10000.txt.gz ../../www/10000.txt.old.gz
	sleep 1
	gunzip -c ../../www/10000.txt.old.gz > ../../www/10000.txt.old
	ul=`ulimit -n` ; \
	val=`expr $$ul '<' $(ULIMIT)` ; \
	if [ $$val = 1 ] ; then \
		echo trying to raise "ulimit -n" for the test... ; \
		set -e ; \
		ulimit -n $(ULIMIT) ; \
	fi ; \
	$(ERL) -noinput $(PA) -s tftest
	$(MAKE) stop

conf: deflateconf

debug:
	$(ERL) $(PA)

clean: tclean
	-rm -f ../../www/0.txt ../../www/1000.txt ../../www/10000.txt ../../www/10000.txt.old
	-rm -f ../../www/10000.txt.gz ../../www/10000.txt.old.gz
	-rm -rf localhost:8000 logs yaws.conf
