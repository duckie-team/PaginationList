# API Design

```kotlin
@Immutable
interface PaginationListState {
    val isPaging: Boolean // `isLoading` is better naming?
    val isRetrying: Boolean
    val exception: Exception?
    val isException get() = exception != null

    val prevPageNumber: Int
    val nowPageNumber: Int
    val nextPageNumber: Int

    fun retry()
    fun cancel()
    fun exception(exception: Throwable) // To notify the exception raised by the user to PaginationListState
}

@Immutable
interface PaginationListConfig {
    val pageSize: Int
    val initPageListSize: Int
    val prefetchPageListDistance: Int
    val enablePlaceholders: Boolean
    val loadedPageListMaxSize: Int
}

@Immutable
interface PageItemState {
    val isFirstItem: Boolean
    val isLastItem: Boolean
    val isPlaceholder: Boolean
    val isLoadedFromOffline: Boolean
}

@Immutable
interface PaginationListScope<T> {
    // can be footer, header, separator, item, etc...
    fun items(
        content: @Composable (
            item: T,
            state: PageItemState
        ) -> Unit
    )
}

/**
 * We first fetch the data from offline,
 * and if it fails or there is no data, we re-fetch it from online.
 */
@Immutable
interface PagingSource<T> {
    suspend fun saveToOffline(pageNumber: Int, items: List<T>): Boolean // return: result

    suspend fun loadFromOffline(pageNumber: Int): List<T>? // return: null if load failed

    suspend fun loadFromOnline(pageNumber: Int): List<T>? // return: null if load failed

    fun mustLoadFromOnline(item: T): Boolean
}

/**
 * ### We want to implement this:
 *
 * - separater
 * - header/footer
 * - retry and exception handling method
 * - auto remove duplicate request
 * - online + offline page load
 */
@Composable
fun <T> PaginationColumn(
    modifier: Modifier = Modifier,
    pagingConfig: PaginationListConfig,
    pagingSource: PagingSource<T>,
    contentPadding: PaddingValues = PaddingValues(0.dp),
    reverseLayout: Boolean = false,
    verticalArrangement: Arrangement.Vertical = when (reverseLayout) {
        true -> Arrangement.Bottom
        else -> Arrangement.Top
    },
    horizontalAlignment: Alignment.Horizontal = Alignment.Start,
    flingBehavior: FlingBehavior = ScrollableDefaults.flingBehavior(),
    userScrollEnabled: Boolean = true,
    content: PaginationListScope<T>.() -> Unit
)
```
