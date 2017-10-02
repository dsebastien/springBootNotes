# Caching

## About

* automatically configured by Spring Boot is @EnableCaching is added
* applies caching to methods
* @Cacheable\("nameOfCacheEntry"\) public int compute\(int i\)
  * will look for cache with "i" in the configured cache entry
  * or use JSR-107 \(JCache\) =&gt; @CacheResult
* Spring Boot uses a simple provider \(concurrent maps in memory\)
  * NOK for production!
* if cache enabled on beans that are not interface based, add proxyTargetClass property to @EnableCaching
* supported cache providers
  * Generic
  * JCache \(EhCache 3, Hazelcast, ...\)
  * EhCache 2
  * Hazelcast
  * Redis
  * ...
* force cache provider to use via spring.cache.type \(useful to disable caching in test environments\)



