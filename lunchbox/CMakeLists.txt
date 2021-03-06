
# Copyright (c) 2012-2016 Stefan Eilemann <eile@eyescale.ch>

set(LUNCHBOX_PUBLIC_HEADERS
  algorithm.h
  any.h
  anySerialization.h
  array.h
  atomic.h
  bitOperation.h
  buffer.h
  buffer.ipp
  clock.h
  compiler.h
  condition.h
  daemon.h
  debug.h
  dso.h
  file.h
  future.h
  futureFunction.h
  hash.h
  indexIterator.h
  init.h
  launcher.h
  lfQueue.h
  lfQueue.ipp
  lfVector.h
  lfVector.ipp
  lfVectorIterator.h
  lock.h
  lockable.h
  log.h
  memoryMap.h
  monitor.h
  mpi.h
  mtQueue.h
  mtQueue.ipp
  omp.h
  os.h
  perThread.h
  perThread.ipp
  perThreadRef.h
  persistentMap.h
  plugin.h
  pluginFactory.h
  pluginFactory.ipp
  pluginRegisterer.h
  pool.h
  readyFuture.h
  refPtr.h
  referenced.h
  request.h
  requestHandler.h
  result.h
  rng.h
  scopedMutex.h
  serializable.h
  sleep.h
  spinLock.h
  stdExt.h
  thread.h
  threadID.h
  timedLock.h
  tls.h
  types.h
  unorderedIntervalSet.h
  unorderedIntervalSet.ipp
  visitorResult.h
  )
if(NOT LUNCHBOX_BUILD_V2_API)
  list(APPEND LUNCHBOX_PUBLIC_HEADERS
    servus.h
    uint128_t.h
    uri.h
    )
endif()

set(LUNCHBOX_HEADERS
  detail/threadID.h
  leveldb/persistentMap.h
  skv/persistentMap.h
  time.h
  test.h
)

set(LUNCHBOX_SOURCES
  any.cpp
  atomic.cpp
  clock.cpp
  condition.cpp
  condition_w32.ipp
  debug.cpp
  dso.cpp
  file.cpp
  init.cpp
  launcher.cpp
  lock.cpp
  log.cpp
  memoryMap.cpp
  mpi.cpp
  omp.cpp
  os.cpp
  persistentMap.cpp
  referenced.cpp
  requestHandler.cpp
  rng.cpp
  sleep.cpp
  spinLock.cpp
  thread.cpp
  threadID.cpp
  timedLock.cpp
  tls.cpp
  )

set(LUNCHBOX_PUBLIC_INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIRS})
list(APPEND LUNCHBOX_LINK_LIBRARIES PUBLIC ${Boost_LIBRARIES} Servus
                                    PRIVATE ${PTHREAD_LIBRARIES})

if(WIN32)
  list(APPEND LUNCHBOX_LINK_LIBRARIES ws2_32 mswsock imagehlp)
endif(WIN32)
if(LINUX)
  list(APPEND LUNCHBOX_LINK_LIBRARIES dl rt)
endif()
if(HWLOC_FOUND)
  list(APPEND LUNCHBOX_LINK_LIBRARIES ${hwloc_LIBRARIES})
endif()
if(DNSSD_FOUND)
  list(APPEND LUNCHBOX_LINK_LIBRARIES ${DNSSD_LIBRARIES})
endif()
if(AVAHI-CLIENT_FOUND)
  list(APPEND LUNCHBOX_LINK_LIBRARIES ${avahi-client_LIBRARIES})
endif()
if(MPI_FOUND)
  list(APPEND LUNCHBOX_LINK_LIBRARIES ${MPI_LIBRARIES})
endif()
if(LEVELDB_FOUND)
  list(APPEND LUNCHBOX_LINK_LIBRARIES ${LEVELDB_LIBRARIES})
endif()
if(LIBMEMCACHED_FOUND)
  list(APPEND LUNCHBOX_LINK_LIBRARIES ${libmemcached_LIBRARIES})
endif()
if(SKV_FOUND)
  list(APPEND LUNCHBOX_LINK_LIBRARIES skv_client fxlogger)
endif()

common_library(Lunchbox)
install_files(include/lunchbox FILES test.h COMPONENT dev)
