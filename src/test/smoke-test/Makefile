
CANONICALIZE=readlink -m -n -q
TOPDIR=$(shell $(CANONICALIZE) $(shell pwd)/../../..)
include $(TOPDIR)/Makefile.base

HEADERS=            \
  TestBase.hh       

SOURCES=

COMMON_SOURCES=              \
  CppUnitTestRunnerLocal.cc  \
  TestBase.cc

TESTS=                       \
  test_backstore             \
  test_cache                 \
  test_dyn_growth            \
  test_large_obj             \
  test_lock   


MYLIBS =  -L$(TOPDIR)/src/lib/$(OBJDIR) -lmdbm
MYLIBS += -lpthread -lrt -lstdc++ -lreadline -lcppunit 
LD_EXTRA += $(MYLIBS) -lcppunit $(LOCAL_LDADD)
CXXFLAGS += -I$(TOPDIR)/src/lib

TESTEXES= $(patsubst %, $(OBJDIR)/%, $(TESTS))

# include locally built libaries in search path...
LD_LIB_PATH=LD_LIBRARY_PATH=$(TOPDIR)/src/lib/$(OBJDIR):$(LD_LIBRARY_PATH)

# generate xml test results...
RUN_ARGS= -tall -x $(1).xml 

default-make-target: $(TESTEXES)

# Create the per-test targets.
$(foreach test, $(TESTS), $(eval $(call CppExeTargetRules,$(test),,$(LD_EXTRA))))

test:: $(TESTEXES)
	@echo "RUNNING SMOKE TESTS MODE=$(MODE), FAST=$(FAST) !!!!"
	$(foreach test, $(TESTS), make run-$(test) && ) /bin/true

valg:: $(TESTEXES)
	$(foreach test, $(TESTS), make valg-$(test) && ) /bin/true

clean :: clean-objs $(foreach exe, $(TESTS), clean-exe-$(exe))
#clean :
#	rm -f *.d $(OBJDIR)/*.o $(OBJDIR)/*.lib $(OBJDIR)/*.so *.xml 
#	rm -f $(TESTEXES) $(OBJDIR)/CppUnitTestRunner


