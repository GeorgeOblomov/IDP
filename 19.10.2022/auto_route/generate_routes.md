<p>
	<img src="./../rersources/routes.png" alt="routes" width="250">
</p>

# Генерация рутов с кастомными входными параметрами

Для каждого объявленного пользователем рута, пакетом генерируется соответствующий PageRouteInfo объект. Эти объекты содержат информацию о пути к руту в виде строки в формате: `"/books"`, а также аргументы которые были указаны в странице с которой связан рут.

```dart
class BookDetailsPage extends StatelessWidget {
    BookDetailsPage({@PathParam('bookId') this.bookId, this.onRateBook}); 

    final int bookId;  
    final void Function(int) onRateBook
```

```dart
class BooksRoute extends PageRouteInfo {
    const BooksRoute() : super(name, path: '/books');

    static const String name = 'BooksRoute'; 
}
```

Согласно документации, пакет может передавать в руты аргументы любого типа. Пакет автоматически просматривает аргументы переданные в страницу и генерирует руты с соответствующими аргументами. Поэтому нет необходимости в дополнительной работе с передаваемыми аргументами, что является большим плюсом.

| [Назад](./../auto_route/under_the_hood.md) | [Далее](./../auto_route/animations.md) |
| ------------------------------------------ | -------------------------------------- |
