

# for this makefile to work, you'll need these:
#
# sudo apt-get install libboost-all-dev
# sudo apt-get install libcgal-dev
#


# Add .d to Make's recognized suffixes.
SUFFIXES += .d

CXX = g++

CFLAGS = -O3 -I. -std=c++14
LFLAGS = -lCGAL -lgmp -lboost_program_options

TARGETS = delaunay_mst_3d

all: $(TARGETS)

delaunay_mst_3d: delaunay_mst_3d.o


$(TARGETS):
	$(CXX) -o $@ $^  $(LFLAGS)


%.o: %.cxx
	$(CXX) -M $(CFLAGS) $< > $*.d
	$(CXX) -c $(CFLAGS) $< -o $@

clean:
	rm -f *.o $(TARGETS) *.d

test:
	./generate_random_pts_3d.pl 1000 > points1k.vtk
	./delaunay3d --filename points1k.vtk > delaunay3d1k.vtk
	./generate_random_pts_3d.pl 100000 > points100k.vtk
	./delaunay3d --filename points100k.vtk > delaunay3d100k.vtk

.PHONY: scale_series
scale_series: 
	mkdir -p scale_series
	rm scale_series/*
	for scale in `LANG=en_US seq 0.01 0.01 1`; do ./delaunay_mst_3d --filename points1k.vtk --scale $$scale > scale_series/mst1k_$$scale.vtk; done
# pull in dependency info for *existing* .o files
# got this hint here: http://scottmcpeak.com/autodepend/autodepend.html
# except I'm using wildcard instead of a hard list of obj/d files.
# it's important for this to be after the first rule. Otherwise,
# make with no arguments will use one of these.
-include $(wildcard *.d)
