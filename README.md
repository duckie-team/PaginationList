# PaginationList

Custom LazyList with Pagination added to Jetpack Compose LazyList.

---

## Why?

The existing Paging3 library is cumbersome to use, and integration with Jetpack Compose is not smooth. Therefore, we made a LazyList that simply includes Pagination.

## API Design

```kotlin
PaginationList()
```

### Support

1. PagingSource
2. insertSeparators/shouldSeparate
3. header/footer
4. offline caching with Room (RemoteMediator)
5. in-memory caching
6. retry and exception handling method
7. auto remove duplicate request
8. PagingSource.getRefreshKey()
9. online + offline page load

# TODO

---

## License

```
will be MIT
```

## Happy Coding :)
