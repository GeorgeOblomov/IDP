<p>
	<img src="./../rersources/page_transition.png" alt="page_transition" width="250">
</p>

# Анимация переходов между экранами

 auto_route  предлагает несколько способов для работы с анимациями сопровождающих навигацию между страницами. Во первых в файле конфигурации мы можем указать одну и следующих аннотаций - @AdaptiveAutoRouter, @MaterialAutoRouter, @CupertinoAutoRouter и в зависимости от указанной подбирается опредленный соотвествующий тип анимации между экранами.
 
 ```dart
    @AdaptiveAutoRouter(
        replaceInRouteName: 'Page,Route',
        routes: [...],
    )
```
```dart
    @MaterialAutoRouter(
        replaceInRouteName: 'Page,Route',
        routes: [...],
    )
```
```dart
    @CupertinoAutoRouter(
        replaceInRouteName: 'Page,Route',
        routes: [...],
    )
 ```
 
 Также можно напрямую указать какого типа рут ожидается - MaterialRoute, CupertinoRoute, AdaptiveRoute. Для соотствующей обработки анимации.
 
 ```dart
 @MaterialAutoRouter(
  replaceInRouteName: 'Page,Route',
  routes: <AutoRoute>[
    CupertinRoute(page: InitPage, initial: true),
    MaterialRoute(page: MainPage),
    AutoRoute(page: CategoryPage),
    AutoRoute(page: FavoritePage),
    AdaptiveRoute(page: LanguagePage),
  ],
 ),  
 ```
 
 Бывает ситуации, когда стандартной платформенной анимации не хватает и приходится оборачивать виджет страницы в PageRouteBuilder или реализовать кастомный PageTransitionBuilder. У auto_route есть решение и для таких кейсов. В конфигурационном файле в списке рутов нужно использовать CustomRoute
 
 ```dart
 @MaterialAutoRouter(
        replaceInRouteName: 'Page,Route',
        routes: [
            ...,
            CustomRoute(),
        ],
    )
 ```
 
 В котором можно либо реализовать свою анимацию указав ее в параметре `transitionBuilder`:
 
 ```dart
 CustomRoute(
    path: '', 
    page: BooksPage,
    transitionsBuilder: SlideTransition(
        position: Tween<Offset>(
          begin: const Offset(0.0, -1.0),
          end: Offset.zero,
        ).animate(animation),
        child: child, 
    );
 )
 ```
 
 Либо использовать готовую из списка:
 1. **TransitionsBuilders.slideTop**
 2. **TransitionsBuilders.slideRight** 
 3. **TransitionsBuilders.slideLeft**
 4. **TransitionsBuilders.slideRightWithFade**
 5. **TransitionsBuilders.slideRightWithFade**
 6. **TransitionsBuilders.slideLeftWithFade**
 7. **TransitionsBuilders.slideBottom**
 8. **TransitionsBuilders.fadeIn**
 9. **TransitionsBuilders.zoomIn**
 10. **TransitionsBuilders.noTransition** 
 
 ```dart
 CustomRoute(
    path: '', 
    page: BooksPage,
    transitionsBuilder: TransitionsBuilders.slideTop
)
 ```
 
 | [Назад](./../auto_route/generate_routes.md) | [Далее](./../auto_route/nested_navigation.md) |
 | ------------------------------------------- | --------------------------------------------- |