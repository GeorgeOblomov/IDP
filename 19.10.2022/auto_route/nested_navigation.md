<p>
	<img src="./../rersources/nested_navigation.png" alt="nested_navigation" width="500">
</p>

# Вложенная навигация

В auto_route вложенная навигация достигается путем указания рутов в параметре children объекта AutoRoute:

```dart
@MaterialAutoRouter(
    replaceInRouteName: 'Page,Route',
    routes: <AutoRoute>[ 
        AutoRoute(path: "/", page: HomePage), 
        AutoRoute(
            path: "/books",
            name: "BooksRouter",
            page: EmptyRouterPage,
            children: [
                AutoRoute(path: '', page: BooksPage),
                AutoRoute(path: ':bookId', page: BookDetailsPage),
                RedirectRoute(path: '*', redirectTo: ''),
            ],
        ),
        AutoRoute(
            path: "/account",
            name: "AccountRouter",
            page: EmptyRouterPage,
            children: [
                AutoRoute(path: '', page: AccountPage),
                AutoRoute(path: 'details', page: AccountDetailsPage),
                RedirectRoute(path: '*', redirectTo: ''),
            ],
        ),
    ],
)
class $AppRouter {}
```

Также желательно указать name, например `"BooksRouter"`, таким образом проще релизовать навигацию между двумя страницами с вложенной навигацией:

```dart
context.pushRoute(AccountRouter(
  children: [
    // push any sequence of Account routes here   
    // the last route will be the one that is currently visible
    AccountRoute(),
    AccountSettingsRoute()
  ]
));
```

Со стороны UI нужно познакомится с  виджетом - `AutoTabsScaffold`, который значительно упрощает работу с вложенными рутами. Данный виджет помимо стандартных параметров Scaffold предоставляет нам параметр routes для хранения списка вложенных рутов.


```dart
@override
Widget build(context) {
  return AutoTabsScaffold(
    routes: const [
      BooksRouter(),
      AccountRouter(),
    ],
    bottomNavigationBuilder: (_, tabsRouter) {
      return BottomNavigationBar(
        currentIndex: tabsRouter.activeIndex,
        onTap: tabsRouter.setActiveIndex,
        items: [
          BottomNavigationBarItem(
            icon: Icon(Icons.book),
            label: 'Books',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.account_box),
            label: 'Account',
          ),
        ],
      );
    },
  );
}
```

Такого же поведения можно добится используя AutoTabsRouter, только придется написать несколько больше шаблонного кода:

```dart

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Group " + id),
      ),
      body: AutoTabsRouter(
        routes: const [
          GroupTab1Router(),
          GroupTab2Router(),
          GroupTab3Router(),
        ],
        duration: const Duration(milliseconds: 400),
        builder: (context, child, animation) {
          final tabsRouter = context.tabsRouter;
          return Scaffold(
            body: FadeTransition(
              opacity: animation,
              child: child,
            ),
            bottomNavigationBar: buildBottomNavigationBar(context, tabsRouter),
          );
        },
      ),
    );
  }
}

BottomNavigationBar buildBottomNavigationBar(
    BuildContext context, TabsRouter tabsRouter) {
  return BottomNavigationBar(
    onTap: tabsRouter.setActiveIndex,
    currentIndex: tabsRouter.activeIndex,
    items: [
      BottomNavigationBarItem(
          icon: Icon(Icons.airline_seat_flat), label: 'Tab 1'),
      BottomNavigationBarItem(icon: Icon(Icons.event), label: 'Tab 2'),
      BottomNavigationBarItem(icon: Icon(Icons.poll), label: 'Tab 3'),
    ],
  );
}
```

 | [Назад](./../auto_route/animations.md) | [На главную](./../../README.md) |
 | -------------------------------------- | ------------------------------- |