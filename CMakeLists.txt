cmake_minimum_required(VERSION 3.14)
project(concorde LANGUAGES C)

list(INSERT CMAKE_MODULE_PATH 0 ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

set(WARNING_OPTIONS             -Wall -Wextra -Werror)
set(RELEASE_OPTIONS             -O3)
set(DEBUG_OPTIONS               -O0)
set(GDB_DEBUG_OPTIONS           -ggdb3)
set(LINKER_OPTIONS              -flto -Wl,--no-as-needed)

find_package(Cplex REQUIRED)

add_compile_options(${WARNING_OPTIONS})
add_compile_options($<$<CONFIG:RELEASE>:${RELEASE_OPTIONS}>)
add_compile_options($<$<CONFIG:DEBUG>:${DEBUG_OPTIONS}>)
add_compile_options($<$<AND:$<CONFIG:DEBUG>,$<CXX_COMPILER_ID:GNU>>:${GDB_DEBUG_OPTIONS}>)
include_directories("src/INCLUDE")

add_library(concorde_utils
        src/UTIL/allocrus.c
        src/UTIL/util.c
        src/UTIL/edgelen.c
        src/UTIL/zeit.c
        src/UTIL/urandom.c
        src/UTIL/signal.c
        src/UTIL/getdata.c
        src/UTIL/edgeutil.c
        src/UTIL/bgetopt.c
        src/UTIL/safe_io.c
        src/UTIL/sortrus.c
        src/UTIL/genhash.c
        src/UTIL/edgemap.c
        src/UTIL/priority.c
        src/UTIL/dheaps_i.c
        src/UTIL/fastread.c)
target_compile_features(    concorde_utils PUBLIC c_std_11)
target_link_libraries(      concorde_utils PUBLIC ${LINKER_OPTIONS})
target_link_libraries(      concorde_utils PRIVATE dl)
target_compile_options(     concorde_utils PRIVATE -Wno-unused-function)

add_library(concorde_lp
        src/LP/lpcplex8.c)
target_compile_features(    concorde_lp PUBLIC c_std_11)
target_link_libraries(      concorde_lp PUBLIC ${LINKER_OPTIONS})
target_link_libraries(      concorde_lp PUBLIC Cplex::cplex)

add_library(concorde_bigguy
        src/BIGGUY/bigguy.c)
target_compile_features(    concorde_bigguy PUBLIC c_std_11)
target_link_libraries(      concorde_bigguy PUBLIC ${LINKER_OPTIONS})

add_library(concorde_heldkarp
        src/HELDKARP/heldkarp.c)
target_compile_features(    concorde_heldkarp PUBLIC c_std_11)
target_link_libraries(      concorde_heldkarp PUBLIC ${LINKER_OPTIONS})

add_library(concorde_edgegen
        src/EDGEGEN/edgegen.c
        src/EDGEGEN/xnear.c
        src/EDGEGEN/delaunay.c
        src/EDGEGEN/mlinkern.c)
target_compile_features(    concorde_edgegen PUBLIC c_std_11)
target_link_libraries(      concorde_edgegen PUBLIC ${LINKER_OPTIONS})
target_compile_options(     concorde_edgegen PRIVATE -Wno-unused-function)

add_library(concorde_linkern
        src/LINKERN/linkern.c
        src/LINKERN/flip_two.c)
target_compile_features(    concorde_linkern PUBLIC c_std_11)
target_link_libraries(      concorde_linkern PUBLIC ${LINKER_OPTIONS})

add_library(concorde_localcut
        src/LOCALCUT/first.c
        src/LOCALCUT/localcut.c
        src/LOCALCUT/separate.c
        src/LOCALCUT/tsporacl.c
        src/LOCALCUT/chunks.c
        src/LOCALCUT/lift.c
        src/LOCALCUT/intmat.c)
target_compile_features(    concorde_localcut PUBLIC c_std_11)
target_link_libraries(      concorde_localcut PUBLIC ${LINKER_OPTIONS})

add_library(concorde_cut
        src/CUT/shrink.c
        src/CUT/connect.c
        src/CUT/segments.c
        src/CUT/mincut.c
        src/CUT/gomoryhu.c
        src/CUT/cut_st.c)
target_compile_features(    concorde_cut PUBLIC c_std_11)
target_link_libraries(      concorde_cut PUBLIC ${LINKER_OPTIONS})

add_library(concorde_pq
        src/PQ/cuttree.c
        src/PQ/consec1.c
        src/PQ/necklace.c
        src/PQ/pq.c)
target_compile_features(    concorde_pq PUBLIC c_std_11)
target_link_libraries(      concorde_pq PUBLIC ${LINKER_OPTIONS})
target_compile_options(     concorde_pq PRIVATE -Wno-unused-function)

add_library(concorde_fmatch
        src/FMATCH/fmatch.c)
target_compile_features(    concorde_fmatch PUBLIC c_std_11)
target_link_libraries(      concorde_fmatch PUBLIC ${LINKER_OPTIONS})

add_library(concorde_kdtree
        src/KDTREE/kdbuild.c
        src/KDTREE/kdnear.c
        src/KDTREE/kdspan.c
        src/KDTREE/kdtwoopt.c)
target_compile_features(    concorde_kdtree PUBLIC c_std_11)
target_link_libraries(      concorde_kdtree PUBLIC ${LINKER_OPTIONS})

add_library(concorde_combs
        src/COMBS/dngreedy.c
        src/COMBS/block.c)
target_compile_features(    concorde_combs PUBLIC c_std_11)
target_link_libraries(      concorde_combs PUBLIC ${LINKER_OPTIONS})

add_library(concorde_verify
        src/VERIFY/verify.c)
target_compile_features(    concorde_verify PUBLIC c_std_11)
target_link_libraries(      concorde_verify PUBLIC ${LINKER_OPTIONS})

add_library(concorde_tiny
        src/TINY/bnbmsp.c
        src/TINY/tinytsp.c)
target_compile_features(    concorde_tiny PUBLIC c_std_11)
target_link_libraries(      concorde_tiny PUBLIC ${LINKER_OPTIONS})

add_executable(concorde
        src/TSP/concorde.c
        src/TSP/control.c
        src/TSP/prob_io.c
        src/TSP/branch.c
        src/TSP/cutpool.c
        src/TSP/bcontrol.c
        src/TSP/tsp_lp.c
        src/TSP/ex_price.c
        src/TSP/control.c
        src/TSP/generate.c
        src/TSP/cutcall.c
        src/TSP/blossom.c
        src/TSP/blkcomb.c
        src/TSP/growcomb.c
        src/TSP/prclique.c
        src/TSP/xtour.c
        src/TSP/cliqwork.c
        src/TSP/skeleton.c
        src/TSP/cliqhash.c
        src/TSP/qsparse.c
        src/TSP/tighten.c
        src/TSP/cutcall.c
        src/TSP/teething.c
        src/TSP/ddecker.c
        src/TSP/combcliq.c)
target_compile_features(    concorde PUBLIC c_std_11)
target_link_libraries(      concorde PUBLIC ${LINKER_OPTIONS})
target_link_libraries(      concorde PUBLIC concorde_utils)
target_link_libraries(      concorde PUBLIC concorde_lp)
target_link_libraries(      concorde PUBLIC concorde_bigguy)
target_link_libraries(      concorde PUBLIC concorde_heldkarp)
target_link_libraries(      concorde PUBLIC concorde_edgegen)
target_link_libraries(      concorde PUBLIC concorde_linkern)
target_link_libraries(      concorde PUBLIC concorde_localcut)
target_link_libraries(      concorde PUBLIC concorde_pq)
target_link_libraries(      concorde PUBLIC concorde_cut)
target_link_libraries(      concorde PUBLIC concorde_fmatch)
target_link_libraries(      concorde PUBLIC concorde_kdtree)
target_link_libraries(      concorde PUBLIC concorde_combs)
target_link_libraries(      concorde PUBLIC concorde_verify)
target_link_libraries(      concorde PUBLIC concorde_tiny)
target_link_libraries(      concorde PUBLIC m)
target_link_libraries(      concorde PUBLIC pthread)
target_compile_options(     concorde PRIVATE -Wno-unused-function)
target_compile_options(     concorde PRIVATE -Wno-stringop-truncation)
target_compile_options(     concorde PRIVATE -Wno-stringop-overflow)

add_custom_command(TARGET concorde POST_BUILD
        COMMAND ar -x libconcorde_utils.a
        COMMAND ar -x libconcorde_lp.a
        COMMAND ar -x libconcorde_bigguy.a
        COMMAND ar -x libconcorde_heldkarp.a
        COMMAND ar -x libconcorde_edgegen.a
        COMMAND ar -x libconcorde_linkern.a
        COMMAND ar -x libconcorde_localcut.a
        COMMAND ar -x libconcorde_pq.a
        COMMAND ar -x libconcorde_cut.a
        COMMAND ar -x libconcorde_fmatch.a
        COMMAND ar -x libconcorde_kdtree.a
        COMMAND ar -x libconcorde_combs.a
        COMMAND ar -x libconcorde_verify.a
        COMMAND ar -x libconcorde_tiny.a
        COMMAND ar -qc libconcorde.a *.o
        COMMAND rm *.o
        COMMAND rm libconcorde_utils.a
        COMMAND rm libconcorde_lp.a
        COMMAND rm libconcorde_bigguy.a
        COMMAND rm libconcorde_heldkarp.a
        COMMAND rm libconcorde_edgegen.a
        COMMAND rm libconcorde_linkern.a
        COMMAND rm libconcorde_localcut.a
        COMMAND rm libconcorde_pq.a
        COMMAND rm libconcorde_cut.a
        COMMAND rm libconcorde_fmatch.a
        COMMAND rm libconcorde_kdtree.a
        COMMAND rm libconcorde_combs.a
        COMMAND rm libconcorde_verify.a
        COMMAND rm libconcorde_tiny.a
        WORKING_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}"
        COMMENT "Crating libconcorde.a"
        )

add_custom_command(TARGET concorde POST_BUILD
        COMMAND cat config.h machdefs.h util.h bigguy.h combs.h cut.h delaunay.h edgegen.h fmatch.h heldkarp.h kdtree.h linkern.h lp.h tsp.h consec1.h localcut.h macrorus.h mlinkern.h necklace.h pqsets.h pq.h cuttree.h tinytsp.h tsp.h verify.h | grep -v '\#include \"' > ${CMAKE_CURRENT_BINARY_DIR}/concorde.h
        WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/src/INCLUDE"
        COMMENT "Creating concorde.h"
        )