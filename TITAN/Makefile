VPATH = ../SRC
CXX = g++
CXXFLAGS = -Wall -O2
CXXFLAGS += -march=core2 -mavx
CXXFLAGS += -fopenmp
CXXFLAGS += -I /usr/local/cuda/include -I/usr/local/cuda/samples/common/inc
NVCC = /usr/local/cuda/bin/nvcc
NVFLAGS = -O -arch=sm_35 -Xptxas -v -Xcompiler "-O2 -march=core2 -mavx"
LIBCUDA = -L /usr/local/cuda/lib64 -lcudart

all: gpu-4th gpu-6th gpu-8th

touch:
	touch TOUCH

ptx: hermite4-titan.ptx hermite6-titan.ptx hermite8-titan.ptx

cubin: hermite4-titan.cubin hermite6-titan.cubin hermite8-titan.cubin

sass: hermite4-titan.sass hermite6-titan.sass hermite8-titan.sass

parameter.o: parameter.cpp parameter.h
	$(CXX) $(CXXFLAGS) -c $< -o $@

gpu-4th: compile.cpp nbodysystem.h hermite4.h hermite4-titan.h parameter.o hermite4-titan.o TOUCH
	$(CXX) $(CXXFLAGS) -DFOURTH -DCUDA_TITAN $< parameter.o hermite4-titan.o $(LIBCUDA) -o $@
	cp $@ ../RUN

gpu-6th: compile.cpp nbodysystem.h hermite6.h hermite6-titan.h parameter.o hermite6-titan.o TOUCH
	$(CXX) $(CXXFLAGS) -DSIXTH -DCUDA_TITAN $< parameter.o hermite6-titan.o $(LIBCUDA) -o $@
	cp $@ ../RUN

gpu-8th: compile.cpp nbodysystem.h hermite8.h hermite8-titan.h parameter.o hermite8-titan.o TOUCH
	$(CXX) $(CXXFLAGS) -DEIGHTH -DCUDA_TITAN $< parameter.o hermite8-titan.o $(LIBCUDA) -o $@
	cp $@ ../RUN

hermite4-titan.o: hermite4-titan.cu hermite4-titan.h hermite4.h TOUCH
	$(NVCC) $(NVFLAGS) -c $<

hermite6-titan.o: hermite6-titan.cu hermite6-titan.h hermite6.h TOUCH
	$(NVCC) $(NVFLAGS) -c $<

hermite8-titan.o: hermite8-titan.cu hermite6-titan.h hermite6.h TOUCH
	$(NVCC) $(NVFLAGS) -c $<

hermite4-titan.s: hermite4-titan.o
	objdump -d $< | c++filt > $@

hermite6-titan.s: hermite6-titan.o
	objdump -d $< | c++filt > $@

hermite8-titan.s: hermite8-titan.o
	objdump -d $< | c++filt > $@

hermite4-titan.ptx: hermite4-titan.cu hermite4-titan.h hermite4.h
	$(NVCC) $(NVFLAGS) -ptx $<

hermite6-titan.ptx: hermite6-titan.cu hermite6-titan.h hermite6.h
	$(NVCC) $(NVFLAGS) -ptx $<

hermite8-titan.ptx: hermite8-titan.cu hermite8-titan.h hermite8.h
	$(NVCC) $(NVFLAGS) -ptx $<

hermite4-titan.cubin: hermite4-titan.cu hermite4-titan.h hermite4.h
	$(NVCC) $(NVFLAGS) -cubin $< |& c++filt

hermite6-titan.cubin: hermite6-titan.cu hermite6-titan.h hermite6.h
	$(NVCC) $(NVFLAGS) -cubin $< |& c++filt

hermite8-titan.cubin: hermite8-titan.cu hermite8-titan.h hermite8.h
	$(NVCC) $(NVFLAGS) -cubin $< |& c++filt

hermite4-titan.sass: hermite4-titan.cubin
	/usr/local/cuda/bin/cuobjdump -sass $< | c++filt > $@

hermite6-titan.sass: hermite6-titan.cubin
	/usr/local/cuda/bin/cuobjdump -sass $< | c++filt > $@

hermite8-titan.sass: hermite8-titan.cubin
	/usr/local/cuda/bin/cuobjdump -sass $< | c++filt > $@


