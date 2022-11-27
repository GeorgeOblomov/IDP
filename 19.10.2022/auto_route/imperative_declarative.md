<p>
	<img src="./../rersources/routes.png" alt="routes" width="250">
</p>

# Использование пакета в императивном и декларативном стилях

## Прежде чем обсуждать работу с пакетом, пару слов о нем самом:

Как пишет сам автор, этот пакет предназначен для управления навигацией в приложении. Он позволяет генерировать руты с кастомными параметрами, поддерживает простую работу с диплинками и многое другое.<br/>
При этом можно выбрать какой подход для работы с навигацией для вас более предпочтительный - императивный (Navigator 1.0) или декларативный (Navigator 2.0).

* ### **Императивный стиль**:

  В случае если вы выбрали императивный подход - все просто. Работа аналогична использованию Navigator 1.0. Вы также манипулируете страницами в стеке используя привычные методы - `push`, `pop` и `replace` и их различные реализации. Только вместо MaterialPageRoute вы используете наследников PageInfoRoute сгенерированных пакетом. И вместо объекта Navigator, нужно достучаться через конекст до AutoRouter и использовать его. Также можно работать с AutoRouter без использования контекста например внедряя его с помощью пакета [get_it](https://pub.dev/packages/get_it) 
  
   Выглядит это следующим образом:
   
   ```dart
   // First, get the scoped router by calling
  AutoRouter.of(context)
  // or using the extension
  context.router

  // PUSH methods
  // for pushing to the top of the stack
  .push(BooksRoute())
  .pushNamed('/books')  
  // for pushing multiple pages into the stack
  .pushAll([BooksRoute(), BookDetailsRoute()])
  .popAndPush(BooksRoute())
  .popAndPushAll([BooksRoute(), BookDetailsRoute()]) 
  // pushes a route and pops the underlying pages based on a predicate
  .pushAndPopUntil(BooksRoute(), (route) {return shouldPop(route);})

  // REPLACE methods
  // for replacing the last page in the stack (throws an error if the stack is empty)
  .replace(BooksRoute())
  .replaceNamed('/books')
  // for replacing multiple pages into the stack
  .replaceAll([BooksRoute(), BookDetailsRoute()]) 

  // POP methods
  // for popping last page of the current stack (unless the stack only has one entry)
  .pop()
  .popAndPush(BooksRoute())
  .popAndPushAll([BooksRoute(), BookDetailsRoute()])
  // pops until a specific page based on a predicate
  .popUntil((route) {return shouldPop(route);})
  .popUntilRoot()
  .popUntilRouteWithName('/books') 
   ```
   
   Также AppRouter предлагает методы типа `navigate`. Они позволяют делать pop до тех пор пока не встретят нужный рут. В случае если данного рута в стеке нет, создается новый и далее происходит навигация к нему.
   
   ```dart
  .navigate(BooksRoute())
  .navigateNamed('/books')  
  .navigateAll([BooksRoute(), BookDetailsRoute()])
   ```

* ### **Декларативный стиль**:
  
  Прежде стоит вспомнить, что при декларативном подходе во Flutter разделяют два способа работы с Navigator 2.0:
  1. Так называемый Pages API, когда в Navigator декларативно устанавливается список из объектов Page, который в последствии преобразуется в стек из объектов Route.
  ```dart
  MaterialApp(
      ...
      home: Navigator(
        pages: [
          MaterialPage(
            key: const ValueKey('ItemListScreen'),
            child: ItemsListScreen(
              onItemTapped: _handleItemTapped,
              onRouteTapped: _handleRouteTapped,
            ),
          ),
          if (show404)
          const MaterialPage(
            key: ValueKey('Error Page'),
            child: Center(
              child: Text('404'),
            ),
          )
          else if (_selectedItem != null)
            MaterialPage(
              key: ValueKey(_selectedItem!),
              child: ItemDetailsScreen(
                item: _selectedItem!,
              ),
            )
        ],
        onPopPage: (route, result) => route.didPop(result),
      ),
    );
  ```
  2. С помощью Router. Когда мы реализуем собственные версии RouterDelegate и RouteInformationParser и используем их в MaterialPage.router.
  ```dart
  class _MyAppState extends State<MyApp> {
    ShopListRouterDelegate routerDelegate = ShopListRouterDelegate();
    ShopListRouterInformationParser routerInformationParser = ShopListRouterInformationParser();

    @override
    Widget build(BuildContext context) {
      return MaterialApp.router(
        ...
        routerDelegate: routerDelegate,
        routeInformationParser: routerInformationParser,
      );
    }
  }
  ```
  
  Также и в случае auto_route, у нас есть возможность применить декларативный подход в двух направлениях:
  
  1. Это использование AutoRouter.declarative аналогично тому, когда мы использовали Navigator. Где также внутри мы можем указать стек рутов и определить коллбэк onPopRoute, аналогичный onPopPage в Navigator.
  ```dart
  AutoRouter.declarative(      
  routes: (handler) => [      
     BookListRoute(),      
     if(_selectedBook != null)      
     BookDetailsRoute(id: _selectedBook.id),      
  ],);    
  ```
  2. А также использование AutoRouterDelegate.declarative, когда у нас есть возможность самим определить RouterDelegate и RouteInformationParser:
  ```dart
  Widget build(BuildContext context) {
    return MaterialApp.router(
        routerDelegate: AutoRouterDelegate.declarative(
          _appRouter,
          routes: (_) => [
            if (appState.isSignedIn)
              AppStackRoute()
            else
              SignInRoute(
                onSignedIn: _handleSignedIn,
              ),
          ],
        ),
        routeInformationParser:
            _appRouter.defaultRouteParser(includePrefixMatches: true));
  }
  ```
  
| [Назад](./../../README.md) | [Далее](./../auto_route/under_the_hood.md) |
| -------------------------- | ------------------------------------------ |