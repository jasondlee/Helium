# Helium UI

Collection of AppBlocks powering common Android UI patterns:

- List
- Cards
- ViewPager

## Lists

Being the most common UI pattern for mobile apps, Helium provides a powerful and easy to use List Block that is ready to be assembled with your own Blocks.

<img src="/docs/images/list2.png" width=300> <img src="/docs/images/list.png" width=300>

### Features

##### ListLogic

- Only requires a `ListRepository` that you implement
- Handles fetching data asynchronously on resume
- Handles pagination logic
- Pushes loading, empty, error, loaded states, retained across configuration changes
- Optional: refresh policy

##### ListUi

- Only requires a `ListItem` that you implement
- Inflates a default layout
- Sets up a default layout manager
- Sets up an adapter that propagates user events and handles data changes with `DiffUtil`
- Renders loading, empty, error, and loaded states
- Pushes scroll events, swipe to refresh events, item UI events, empty and error UI events
- Optional: Enable Swipe to refresh
- Optional: provide your own Empty view & Error view
- Allows for full customization of the layout, recycler view, and layout manager

### Usage

For a simple, vertical list that shows items of type `MyListItem`, all you need is this:

```kotlin
val listUi = ListUi(layoutInflater, { inflater, container ->
  MyListItem(inflater, container)
})
```

A `ListItem` is a recycler view holder that can bind a model and push `BlockEvent`. In this example, we're binding a model of type `MyData` to show some text, and pushing events of type `ClickEvent`:

```kotlin
class MyListItem(inflater: LayoutInflater, container: ViewGroup) :
    ListItem<MyData, ClickEvent<MyData>>(R.layout.list_item_layout, inflater, container) {

    private val title : TextView = findView(R.id.title)

    override fun bind(data: MyData) {
        title.text = data.title
        view.setOnClickListener { pushEvent(ClickEvent(view, data)) }
    }
}
```

## Cards

To complement the ListUI block, Helium comes with a simple Card style list item to quickly build card lists with minimal effort.

IMPORTANT: In order to use these, your theme must extend one of the `Theme.MaterialComponents`.

<img src="/docs/images/cards.png" width=300>


#### CardListItem

- Only requires a body layout resource id or view that you implement
- Wraps your layout with a `MaterialCardView`
- Polished styling and elevation out of the box
- Allows for full customization of the card properties or the card layout
- Integrates seamlessly with a `ListUi` block

#### ContentCardListItem

- Convenience list item to display a simple card with thumbnail, title, subtitle
- Only requires extending it and implementing 3 methods to bind image, title, subtitle
- See screenshot above for a preview, and [CardListActivity](/samples/demoapp/src/main/java/com/joaquimverges/demoapp/CardListActivity.kt) for a code example.

## View Pagers

Another common Android UI pattern is swipeable screens, using a ViewPager of Fragments. Helium provides a handy block to simplify this process.

#### PagerUi

- Only requires passing a `FragmentPageProvider` interface that you implement
- Sets up and maintains a pager adapter
- Handles choosing between `FragmentPagerAdapter` and `FragmentStatePagerAdapter` depending on the number of fragments to displays
- Handles propagating the right lifecycle events to each fragment when on screen
- Allows for full customization of the ViewPager or its layout
- See [ViewPagerActivity](/samples/demoapp/src/main/java/com/joaquimverges/demoapp/ViewPagerActivity) for a code example.
