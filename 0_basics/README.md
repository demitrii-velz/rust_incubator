Step 0: Building Up an Vocabulary
========================================

__Estimated time__: 4 days

Read through [Rust Book], [Rust FAQ], and become familiar with basic [Rust] concepts, syntax, memory model, type and module systems.

Polish your familiarity by completing [Rust By Example] and [rustlings].

Read through [Cargo Book] and become familiar with [Cargo] and its workspaces.

After completing these steps, you should be able to answer (and understand why) the following questions:
- What memory model [Rust] has? Is it single-threaded or multiple-threaded? Is it synchronous or asynchronous? What is the memory layout of the box and vector? What are heap and stack? Where, but on heap and stack data could live in RAM?
	- Работа с данным в Rust использует следующие механизмы:
		- Ownership (Владение) - Данные имеют только одного владельца. Данные существуют в памяти если Владелец данных находится в области видимости.
		- Borrow (Заимствование) - Возможность заимствовать данные, если Владелец данных существует, на время - до тех пор, пока Владелец данных существует. Одновременно заимствовать можно только двумя способами:
			1. Неизменяемое заимствование, сколь угодно копий одновременно.
			2. Изменяемое заимствование, только одно одновременно.
		- LIfetime (Время жизни) - Каждый Владелец и каждое Заимствование имеет lifetime, который даёт возможность компилятору отлавливать нарушения заимствования.
	- Механизмы работы с данными, работают и дают гарантии при однопоточном и многопоточном, а также синхронном и асинхронном использовании.
	- Memory layout для `Box<T>` и `Vec<T>` - участок для данных в динамической памяти процесса.
	- Участки памяти:
		- Heap - вся динамическая память процесса. Ограничена физическим объемом памяти.
		- Stack - фиксированные участок памяти, предзназначенный для хранения локальных переменных и аргументов вызова функции.
	+ Данные также могут храниться в статической памяти, что выделяется в начале запуска программы и освобождается после ее завершения.
- What runtime [Rust] has? Does it use a GC (garbage collector)?
	- Rust использует libc как часть своего runtime, также в бинарном файле могут присутствовать функции необходимые LLVM.
	- Rust не использует GC.
- What statically typing means? What is a benefit of using it? Weak typing vs strong typing? Implicit / explicit?
	- Статическая типизация подразумевает что все данные используемые в программе описаны типами языка на этапе компиляции.
	- Позволяет отслеживать корректность взаимодействия данных и проводить оптимизации на основании типов.
	- Слабая типизации даёт больше гибкости во время исполнения программы, также требует дополнительных механизмов во время исполнения.
	- Неявная типизация - механизм компилятора позволяющий выразить тип исходя из контекста использования данных.
- What are generics and parametric polymorphism? Which problems do they solve?
	- Механизмы позволяющие описывать алгоритмы для работы с любыми типами данных.
	- Позволяет строить алгоритмы не зависящие от конкретных типов данных.
- What is nominative typing and structural typing? What is difference?
	- Данные принадлежат одному типу если описаны названием одного и того же типа - Номинативная типизация.
	- Данные принадлежат одному типу если их описание расположения данных в памяти совпадает - Структурная типизация. 
- What are traits? How are they used? How do they compare to interfaces? What are an auto trait and a blanket impl? Uncovered type? What is a marker trait?
	- Трейт - механизм позволяющий абстрагировать нужное поведение от конкретного типа данных и при необходимости объявить для нужного типа это поведение.
	- Используется в паре с генериками, выступая в роли ограничителя, требуя чтобы для любого типа был реализован нужный трейт.
	- трейты могут использовать ассоциативные типы и константы.
	- auto trait позволяет на этапе объявления трейта задать всем типам реализацию этого трейта, структурно, и через negative impl вычесть типы для которых это реализация не нужна.
	- blanket impl в основном используется для extension traits, которые имеют только реализованные по умолчанию методы.
	- marker trait - трейт не имеющий методов, выступающий в роли метки, которую можно отследить в compile time.
- What are static and dynamic dispatches? Which should I use, and when? What is monomorphisation?
	- Статическая диспечеризация - компилятор подставляет вызов конкретной реализации параметрической функции для конкретного типа.
	- Динамическая диспечеризация - компилятор подставляет обращение к виртуальной таблице, для взятие указателя на нужную реализацию функции.
	- Динамическая диспечерезация используется когда нет точной информации на момент компиляции о том какой указатель нужно подставить, накладывая дополнительные расходы на вызов функции.
	- Мономорфизация - генерация компилятором варианта функции для каждого конкретного типа
- What is a crate, module and package in Rust? How do they differ? How are the used? What is workspace?
	- Модуль инкапсулирует в себе объявление всех конструкций языка.
	- Крейт инкапсулирует модули.
	- Пакет инкапсулирует крейты.
	- Воркспейс - общее пространство для крейтов, в package.
- What is cloning? What is copying? How do they compare? What is for trait drop? What is special about the trait?
	- Клонирование объекта, посредством вызова функции .clone().
	- Копирование объекта через механизм move-семантики.
	- Копирование в отличии от клонирования реализуется всегда через memory copy.
	- Drop и Сopy не могут быть реализованы для одного и того же типа.
- What is immutability? What is the benefit of using it? What is the difference between immutability and const?
	- Иммутабельность - концепция которая означает что данные не могут быть изменены.
	- Даёт гарантии что считываемое значение всегда актуальное.
	- const даёт гарантии что считываемое значение всегда актуально и неизменяймо со временем.
- What are move semantics? What are borrowing rules? What is the benefit of using them?
	- move semantics - передача владения в другой scope программы.
	- borrowing rules - правила по которым можно заимстовать объект.
	- Даёт гибкость и гарантии при работе с данными.
- What is RAII? How is it implemented in [Rust]? What is the benefit of using it?
	- RAII - это техника, которая гарантирует, что ресурс будет захвачен при создании и автоматически освобожден при уничтожении объекта.
	- Инициализация обычно осуществляется через метод ::new(), освобождение ресурса осуществляется через имплементацию трейта `Drop`. Примеры RAII - Box, Rc, Arc.
	- Корректное автоматическое управление ресурсом.
- What are lifetimes? Which problems do they solve? Which benefits do they give?
	- lifetime - аналитичская отметка того как долго в программе живут данные.
	- На этапе компиляции проверяется нет ли попытки после уничтожения обращения к объекту.
	- Успешная сходимость всех lifetime, даёт гарантию корректности работы с данными.
- What is an iterator? What is a collection? How do they differ? How are they used?
	- Итератор - интерфейс, трейт, позволяющий перебирать элементы коллекций, единым образом.
	- Коллекция - структура данных, реализованная конкретным образом и имеющая собственный набор методов для взаимодействия.
	- Коллекция ставит перед собой задачу эффективной работы с данным, а итератор как перебрать эти данные.
	- Коллекция подбирается в зависимости от типы работы с данными, по критериям эффективности. Итератор требуется там где нужно перебрать значения.
- What are macros? Which problems do they solve? What is the difference between declarative and procedural macro?
	- Макросы - инструмент для генерации кода.
	- Позволяют уменьшить кол-во шаблонного кода, создав из этого шаблона макрос.
	- Декларативные макросы подставляют код на основе переданных аргументов, процедурные макросы в качестве параметра принимают AST исходного кода - `TokenStream`, позволяя тем самым модифицировать, дополнять, и заменять на другой исходный код. 
- How code is tested in [Rust]? Where should you put tests and why?
	- Unit/Сomponent-тесты - релизуется в том же файле где и определен функционал.
	- Integration-тесты - реализуется в папке `tests` рядом с `src`
	- Doc-тесты - описываются как документация с указанием статуса после исполнения.
- What is special about slice? What is layout of Rust standard data types? Difference between fat and thin pointers?
	- Срез - толстый указатель, указывает на непрерывную область памяти содержащую тип T и содержит информацию о количестве элементов.
	- Лейаут содержит информацию об количестве памяты занимаемых типом и о том на какое число этот тип выравнен.
	- Тонкий указатель это непосредственно адресс ячейки в памяти, толстый указатель содержит указатель и дополнительную о нем информацию.
- Why [Rust] has `&str` and `String` types? How do they differ? When should you use them? Why str slice coexist with slice? What is differnece between `String` and `Vec`? 
	- &str - Толстый указатель на символы строки, String - аллоцированый буффер состоящий из символов.
	- Используются соответствующе со своим типом данных.
	- `&str` в отличии от слайса также как и  `String` в отличии от `Vec`, непозволяет использовать прямую индексацию.
- Is [Rust] OOP language? Is it possible to use SOLID/GRASP? Does it have an inheritance? Is Rust functional language? What variance rules does Rust have?
	- Rust не является OOP language, так как нет возможности наследовать классы.
	- Да это возможно.
	- В Rust нет наследования структур, но есть вомзожность наследование в `trait`.
	- В Rust есть многие возможности из функционального прогаммирования.
	- Раст поддерживает вариативность типов на уровне `lifetime`'ов. 

After you're done notify your lead in an appropriate PR (pull request), and he will exam what you have learned.

_Additional_ articles, which may help to understand the above topic better:
- [Chris Morgan: Rust ownership, the hard way][1]
- [Adolfo Ochagavía: You are holding it wrong][23]
- [Vikram Fugro: Beyond Pointers: How Rust outshines C++ with its Borrow Checker][30]
- [HashRust: A guide to closures in Rust][24]
- [Ludwig Stecher: Rusts Module System Explained][2]
- [Tristan Hume: Models of Generics and Metaprogramming: Go, Rust, Swift, D and More][3]
- [Jeff Anderson: Generics Demystified Part 1][4]
- [Jeff Anderson: Generics Demystified Part 2][5]
- [Bradford Hovinen: Demystifying trait generics in Rust][25]
- [Brandon Smith: Three Kinds of Polymorphism in Rust][6]
- [Jeremy Steward: C++ & Rust: Generics and Specialization][7]
- [cooscoos: &stress about &Strings][8]
- [Jimmy Hartzell: RAII: Compile-Time Memory Management in C++ and Rust][9]
- [Trait Drop][10]
- [Common Lifetime Misconception][11]
- [Visualizing Memory Layout][12]
- [Package vs. Crate terminology (r/rust)][13]
- [Packages and crates (Rust wiki)][14]
- [Full list of available crates on Rust Playground][16]
- [Blanket impl definition][17]
- [Auto-trait definition][18]
- [Georgios Antonopoulos: Rust vs Common C++ Bugs][21]
- [Yurii Shymon: True Observer Pattern with Unsubscribe mechanism using Rust][22]
- [Asynchronous vs Multithreading][29]

Additional:
- [Rust API guidline checklist][19]
- [Interview Questions on Rust Programming][20]
- [Step-by-step instruction to start development in Rust][26]
- [Awesome collection of crates for productive development in Rust][27]
- [Awesome Collection of Materials to Learn Rust][28]

[Cargo]: https://github.com/rust-lang/cargo
[Cargo Book]: https://doc.rust-lang.org/cargo
[Rust]: https://www.rust-lang.org
[Rust Book]: https://doc.rust-lang.org/book
[Rust By Example]: https://doc.rust-lang.org/rust-by-example
[Rust FAQ]: https://prev.rust-lang.org/faq.html
[rustlings]: https://rustlings.cool

[1]: https://chrismorgan.info/blog/rust-ownership-the-hard-way
[2]: https://aloso.github.io/2021/03/28/module-system.html
[3]: https://thume.ca/2019/07/14/a-tour-of-metaprogramming-models-for-generics
[4]: https://web.archive.org/web/20220525213911/http://jeffa.io/rust_guide_generics_demystified_part_1
[5]: https://web.archive.org/web/20220328114028/https://jeffa.io/rust_guide_generics_demystified_part_2
[6]: https://www.brandons.me/blog/polymorphism-in-rust
[7]: https://www.tangramvision.com/blog/c-rust-generics-and-specialization#substitution-ordering--failures
[8]: https://cooscoos.github.io/blog/stress-about-strings
[9]: https://www.thecodedmessage.com/posts/raii
[10]: https://vojtechkral.github.io/blag/rust-drop-order/
[11]: https://github.com/pretzelhammer/rust-blog/blob/master/posts/common-rust-lifetime-misconceptions.md
[12]: https://www.youtube.com/watch?v=rDoqT-a6UFg
[13]: https://www.reddit.com/r/rust/comments/lvtzri/confused_about_package_vs_crate_terminology/
[14]: https://rustwiki.org/en/book/ch07-01-packages-and-crates.html
[16]: https://github.com/integer32llc/rust-playground/blob/master/compiler/base/Cargo.toml
[17]: https://doc.rust-lang.org/reference/glossary.html#blanket-implementation
[18]: https://doc.rust-lang.org/reference/special-types-and-traits.html#auto-traits
[19]: https://rust-lang.github.io/api-guidelines/checklist.html
[20]: https://iq.opengenus.org/questions-on-rust/
[21]: https://geo-ant.github.io/blog/2022/common-cpp-errors-vs-rust
[22]: https://web.archive.org/web/20230319015854/https://ybnesm.github.io/blah/articles/true-observer-pattern-rust
[23]: https://ochagavia.nl/blog/you-are-holding-it-wrong
[24]: https://hashrust.com/blog/a-guide-to-closures-in-rust
[25]: https://gruebelinchen.wordpress.com/2023/06/06/demystifying-trait-generics-in-rust
[26]: https://github.com/rust-lang-ua/learn_rust_together/blob/master/introduction.md
[27]: https://github.com/rust-lang-ua/learn_rust_together/blob/master/toolbox_general.md
[28]: https://github.com/rust-lang-ua/learn_rust_together/blob/master/learn.md
[29]: https://github.com/Learn-Together-Pro/ComputerScience/blob/master/cheatsheets.md#asynchronous-vs-multithreading
[30]: https://dev.to/vikram2784/beyond-pointers-how-rust-outshines-c-with-its-borrow-checker-1mad