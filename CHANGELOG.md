# Changelog
All notable changes to this project will be documented in this file.
Documented changes are those specific to Splice. 
We use `django-redis` version `4.12.1` as the base.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
### Added
- Dependencies specified in `requirements.txt`:
    - We use our modified, Splice-aware
      [`fakeredis`](https://github.com/michael-hahn/fakeredis) as 
      one server option, in addition to the standard redis server.
      

### Changed
- Dependencies specified in `requirements.txt`:
    - We use our modified, Splice-aware 
      [`redis-py`](https://github.com/michael-hahn/redis-py), instead of the
      original package. See its `CHANGELOG` for more information.
    - We use our [`django`](https://github.com/michael-hahn/django-1/tree/splice)
      package (instead of the original `django`) to introduce taint-aware 
      Splice data types.
      
- We support Splice-aware `DefaultClient`:
    - The `connect` method can establish a connection to `fakeredis` if `django`'s
    `CACHES` setting specifies it, like this:
  ```python
    CACHES = {
      'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': "fakeredis",
        'OPTIONS': {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        }
      },
    }
  ```
  
    - The `get` method is modified to comply with the fact that `fakeredis`
      returns `b''` when a key does not exist, instead of `None`.