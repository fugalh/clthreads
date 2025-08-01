# ------------------------------------------------------------------------------
#
#  Copyright (C) 2003-2018 Fons Adriaensen <fons@kokkinizita.net>
#
#  This program is free software; you can redistribute it and/or modify
#  it under the terms of the GNU General Public License as published by
#  the Free Software Foundation; either version 2 of the License, or
#  (at your option) any later version.
#
#  This program is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU General Public License for more details.
#
#  You should have received a copy of the GNU General Public License
#  along with this program.  If not, see <http:#www.gnu.org/licenses/>.
#
# ------------------------------------------------------------------------------


# Modify as required.
#
SUFFIX := $(shell uname -m | sed -e 's/^unknown/$//' -e 's/^i.86/$//' -e 's/^x86_64/$/64/')
PREFIX ?= /usr/local
INCDIR ?= $(PREFIX)/include
LIBDIR ?= $(PREFIX)/lib$(SUFFIX)


MAJVERS = 2
MINVERS = 4.2
VERSION = $(MAJVERS).$(MINVERS)


CPPFLAGS += -DVERSION=\"$(VERSION)\" -D_REENTRANT -D_POSIX_PTHREAD_SEMANTICS -I.  
CXXFLAGS += -Wall -O2 -fPIC
CXXFLAGS += -march=native
LDLFAGS += 
LDLIBS +=


CLTHREADS_SO = libclthreads.so
CLTHREADS_MAJ = $(CLTHREADS_SO).$(MAJVERS)
CLTHREADS_MIN = $(CLTHREADS_MAJ).$(MINVERS)
CLTHREADS_DEP = -lpthread
CLTHREADS_O = p_thread.o a_thread.o itc_mesg.o itc_ip1q.o itc_ctrl.o textmsg.o
CLTHREADS_H = clthreads.h


$(CLTHREADS_MIN): $(CLTHREADS_O)
	$(CXX) -shared $(LDFLAGS) -Wl,-soname,$(CLTHREADS_MAJ) -o $(CLTHREADS_MIN) $(CLTHREADS_O) $(CLTHREADS_DEP)


install:	$(CLTHREADS_MIN)
	install -d $(DESTDIR)$(PREFIX)/$(INCDIR)
	install -d $(DESTDIR)$(PREFIX)/$(LIBDIR)
	install -m 644 $(CLTHREADS_H) $(DESTDIR)$(INCDIR)
	install -m 755 $(CLTHREADS_MIN) $(DESTDIR)$(LIBDIR)
	ldconfig
	ln -sf $(CLTHREADS_MIN) $(DESTDIR)$(LIBDIR)/$(CLTHREADS_SO)

uninstall:
	rm -rf $(DESTDIR)$(INCDIR)/$(CLTHREADS_H)
	rm -rf $(DESTDIR)$(LIBDIR)/libclthreads*

clean:
	/bin/rm -f *~ *.o *.a *.d *.so.*

