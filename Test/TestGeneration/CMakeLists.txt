if(NOT BUILD_TESTING)
  return()
endif()

################################################################################
# Package: TestGeneration
################################################################################
gaudi_subdir(TestGeneration v1r0)

find_package(HepMC)

gaudi_depends_on_subdirs(GaudiAlg GaudiKernel FWCore Generation)

gaudi_add_module(TestGenerationPlugins
                 src/components/*.cpp
                 INCLUDE_DIRS FWCore HepMC FCCEDM
                 LINK_LIBRARIES GaudiKernel FWCore HepMC FCCEDM)

include(CTest)
gaudi_add_test(EDM2HepMCConverstion
               WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
               FRAMEWORK tests/options/edmconvert.py)

gaudi_add_test(GenMergeTest
               WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
               FRAMEWORK tests/options/merge.py)

gaudi_add_test(HepMCGraphProducer
               WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
               FRAMEWORK tests/options/HepMCGraphTest.py)

gaudi_add_test(HepMCGraphCheck
               WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
               COMMAND python Test/TestGeneration/tests/scripts/check_particle_graph.py
               ENVIRONMENT PYTHONPATH+=${PODIO_PYTHON_DIR}
               DEPENDS HepMCGraphProducer
               )
