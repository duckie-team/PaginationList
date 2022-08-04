# API Design

```kotlin
@Immutable
interface PaginationListConfig {
    val initPrefetchPageCount: Int
    val prefetchPageListDistance: Int
    val enablePlaceholders: Boolean
    val loadedPageListMaxCount: Int
}

@Immutable
interface PageItemState {
    val isFirstItem: Boolean
    val isLastItem: Boolean // only true when last loaded item is visible
    val isPlaceholder: Boolean
    val exception: Throwable?

    suspend fun retry(): Boolean
}

@Immutable
interface PaginationListScope<T> {
    // can be header, footer, separator, item, etc...
    // item is null when it's placeholder
    fun items(content: @Composable (item: T?, itemState: PageItemState) -> Unit)
}

@Immutable
interface PagingSource<T, Key> {
    suspend fun loadPage(pageKey: Key): Result<List<T>>

    fun calcNextPageKey(currentPageKey: Key, lastLoadedItem: T?): Key? // return: null if next page key is none
}

@Composable
fun <T, Key> PaginationColumn(
    modifier: Modifier = Modifier,
    pagingConfig: PaginationListConfig,
    pagingSource: PagingSource<T, Key>,
    contentPadding: PaddingValues = PaddingValues(0.dp),
    reverseLayout: Boolean = false,
    verticalArrangement: Arrangement.Vertical = when (reverseLayout) {
        true -> Arrangement.Bottom
        else -> Arrangement.Top
    },
    horizontalAlignment: Alignment.Horizontal = Alignment.Start,
    flingBehavior: FlingBehavior = ScrollableDefaults.flingBehavior(),
    content: PaginationListScope<T>.() -> Unit
)
```
