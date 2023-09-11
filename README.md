# Book Dealers

## Inicializálás

Készíts a Sprint Initializr segítségével egy új projektet `book-dealers` néven!
A szokásos beállításokat használd (Type: Maven, Group: hu.progmatic), 
majd a Next gomra kattintás után add hozzá az alábbi 3 dependenciát: 
- Developer Tools: Spring Boot DevTools
- Web: Spring Web
- Template Enginges: Thymeleaf

## Home page és About page készítése

Miután leellenőrizted, hogy fordul a projekt, nekiláthatsz a kódolásnak.
A `@SpringBootApplication` annotációval ellátott osztály mellé készíts egy `controller`
package-et és vegyél fel bele egy `PageController` osztályt!

Az osztályt lásd el `@Controller` annotációval és vegyél fel bele egy-egy metódust,
amelyek a `home` és az `about` oldalak elérését teszik lehetővé!
Ehhez mindkét metódust el kell látnod a megfelelő `@GetMapping`-gel és elkészíteni
egy-egy HTML fájlt az oldalakhoz. A `.html` fájlokat konvenció szerint tedd a
`resources/templates` mappába!

Az `application.properties`-be vedd fel az alábbi két sort:
```
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
```

Továbbá oldd meg, hogy a `home` és az `about` oldalakról át lehessen navigálni egymásra!

Formázd meg az oldalaid egy (vagy több) `.css` fájllal, amit konvenció szerint a `resources/static/css` mappába készíts!

Akkor vagy kész, ha a `localhost:8080/`-t megnyitva előjön a (szépen formázott) főoldal, amiről át tudsz navigálni az About
oldalra, majd vissza.

## Könyv adatstuktúra és service

Készíts egy `model` package-et, amiben vegyél fel egy `Book` classt, ami a könyvadatbázisunk könyveit fogja reprezentálni.
Minden könyvnek legyen egy szerzője és egy címe, továbbá készíts default és kétparaméteres konstruktorokat, gettereket,
settereket!

Hozz létre egy `service` package-et, azon belül pedig egy `BookService` osztályt!
Az osztályt lásd el `@Service` annotációval! Vegyél fel benne egy könyveket tároló listát, amit inicializálj
egy üres listával!
Készíts az osztálynak egy paraméter nélküli konstruktort, ahol töltsd fel példaadatokkal a könylistát!
Például: `books.add(new Book("The Lord of The Rings", "J. R. R. Tolkien"));`
Lehessen továbbá lekérdezni az összes könyvet, illetve új könyvet felvenni!

Szerenénk, ha a `PageController` osztályunk ebből a `BookService`-ből vegye az adatokat és megjelenítse egy külön
oldalon a könyveket. Ehhez injektáljuk be ezt az új osztályt az alábbi módon:

```
@Controller
public class PageController {

    private final BookService bookService;

    @Autowired
    public PageController(BookService bookService) {
        this.bookService = bookService;
    }
    
    // ...
   
```
Értelmezd a kódot, nézz utána az `@Autowired` annotációnak!


## Books page készítése
A kontroller osztályba vegyél fel egy `/books` endpointot:

```
@GetMapping("/books")
public String listBooks(Model model) {
    model.addAttribute("books", bookService.getAllBooks());
    return "bookList";
}
```
Hozd létre az ehhez tartozó HTML fájlt, ahol használd a Thymeleaf adta előnyöket:
a `th:each`-csel szépen végig tudunk iterálni az attribútumként átadott `books` kollekción!

## Új könyvek hozzáadásának támogatása
Az új könyv felvételének megvalósításához két metódusra lesz szükségünk a kontrollerben:
```
@GetMapping("/add-book")
public String showAddBookForm(Model model) {
    model.addAttribute("newBook", new Book());
    return "addBook";
}
```
```
@PostMapping("/add-book")
public String addBook(@ModelAttribute("newBook") Book newBook) {
    bookService.addBook(newBook);
    return "redirect:/books";
}
```
Figyeld meg, hogy nem `@GetMapping`, hanem `@PostMapping` annotáció került az egyik metódusra!

Készítsd el az `addBook.html`-t és ellenőrizd le, működik-e a könyv hozzáadása funkció!

## Bootstrapes formázás
Talán az egyik legnépszerűbb frontend toolkit a Bootstrap https://getbootstrap.com/, ami előre formált HTML, CSS, JS
elemeket tartalmaz. A linken a `Read the docs`-ra kattintva megnézhetsz pár konkrét példát.

_Ha szeretnél egy konkrét megoldást lelopni akkor itt megteheted: `https://github.com/LilianaBohus/book-dealers`,
a `resources/templates` mappa tartalmát kell figyelned._