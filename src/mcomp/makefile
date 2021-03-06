SRC = mcomp.cpp types.cpp stats.cpp lut.cpp hmm.cpp fisher_exact_test.cpp fexact.c fet2x2.cpp bbf.cpp
PRG= mcomp

##The corresponding files are $(MOABS_ROOT)/src/bsmap/boost/boost/*hpp
ifndef BOOSTINC
	BOOSTINC=$(MOABS_ROOT)/src/bsmap/boost
endif

##The corresponding files are $(MOABS_ROOT)/src/bsmap/boost/lib/libboost*
ifndef BOOSTLIB
	BOOSTLIB=$(MOABS_ROOT)/src/bsmap/boost/lib
endif


EIGEN ?= lib/eigen_3.2.0

R_HOME :=		$(shell R RHOME)

## compiler etc settings used in default make rules
CXX := 			$(shell $(R_HOME)/bin/R CMD config CXX)

CPPFLAGS := 		-Wall -w $(shell $(R_HOME)/bin/R CMD config CPPFLAGS) -Ilib  -isystem $(EIGEN) -I${BOOSTINC} -DBOOST_RESULT_OF_USE_TR1 -DBOOST_NO_DECLTYPE -DBOOST_DISABLE_ASSERTS   -Wno-unused-function

CXXFLAGS := 		-O3 -pipe

LDLIBS := 		-L${BOOSTLIB} -lboost_program_options  -Wl,-rpath=${BOOSTLIB}  -L${BOOSTLIB} -lboost_system -L${BOOSTLIB} -lboost_filesystem -L${BOOSTLIB} -lboost_thread -lrt -Llib -lstan

all:	$(PRG)

OBJ=mcomp.o types.o stats.o lut.o hmm.o fisher_exact_test.o fexact.o fet2x2.o bbf.o

mcomp: $(OBJ)
	$(CXX) -o $(PRG) $(OBJ) $(CPPFLAGS) $(CXXFLAGS) $(LDLIBS) 
	rm -f *.o

mcomp.o: mcomp.cpp
	$(CXX) -c mcomp.cpp $(CPPFLAGS) $(CXXFLAGS)   

types.o: types.cpp
	$(CXX) -c types.cpp $(CPPFLAGS) $(CXXFLAGS) 

stats.o: stats.cpp
	$(CXX) -c stats.cpp $(CPPFLAGS) $(CXXFLAGS) 

fet2x2.o: fet2x2.cpp
	g++ -c fet2x2.cpp -Wall -O3 -ffloat-store

lut.o: lut.cpp
	$(CXX) -c lut.cpp $(CPPFLAGS) $(CXXFLAGS) 

hmm.o: hmm.cpp
	$(CXX) -c hmm.cpp $(CPPFLAGS) $(CXXFLAGS)  

fisher_exact_test.o: fisher_exact_test.cpp
	g++ -c fisher_exact_test.cpp -Wall -O3 -ffloat-store

fexact.o: fexact.c
	gcc -c fexact.c -Wall -O3 -ffloat-store

bbf.o: bbf.cpp
	(cd lib; ./build_stan.sh )
	$(CXX) -c bbf.cpp $(CPPFLAGS) $(CXXFLAGS)   


clean:
	rm -f *.o mcomp

install: $(PRG)
	@mkdir -p $(SRC_ROOT)/bin
	@install -m 755 $(PRG) $(SRC_ROOT)/bin

