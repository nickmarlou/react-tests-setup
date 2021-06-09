# Тестирование React приложений

Привет! Здесь собрана информация о подготовке к тестированию приложений на React и правильной работе с тестами.

## Подготовка

### Настройка

Необходимые `npm пакеты`:

- jest
- nock
- @testing-library/jest-dom
- @testing-library/react
- @testing-library/user-event
- babel-jest
- eslint-plugin-jest

Файлы `jest.config.js`, `jest.setup.js`, `.eslintrc.js`, `.babelrc` содержат конфигурацию, необходимую для тестирования. Они готовы к использованию в проектах в текущем или доработанном виде.

В папке `examples` лежат примеры различных компонентов и кейсов для тестирования.

### Расположение и название тестовых файлов

В проектах файлы с тестами располагаются либо в папке своего компонента, либо в папке `/tests` в корневой директории. Папка `/tests` содержит тесты страниц (`/pages`), функций-хелперов (`/helpers`) и др.

Название тестового файла компонента:\
`ComponentName.test.js`

Название тестового файла функции:\
`functionName.test.js`

## Изучение

### Что тестировать?

В первую очередь, работоспособность компонентов и взаимодействие с приложением от лица юзера. Нажатие на кнопки, заполнение форм, успешный рендер компонентов с пропсами, открытие модалок, подзагрузка элементов и др. То, что в первую очередь связано с UX.

### Что не тестировать?

То, что не влияет на взаимодействие с приложением. Мелочи вроде названия функций и переменных, типы props, стили, сторонние библиотеки и др.

Функции мокаются (_mock_) в самом базовом виде, они не импортируются или копируются до оригинала. Оставляем только базовую логику.

Сторонние библиотеки (например, _Яндекс Карты_) имеют свои тесты. В наших мы можем либо полностью их заменить (используя заглушку типа _\</ div>_), либо протестировать только самую необходимую логику (например, отправку форм из _Formik_).

### Что использовать?

Для рендера компонентов и их поиска используется библиотека [@testing-library/react](https://testing-library.com/docs/react-testing-library/intro).

Для вызова событий и взаимодействия с компонентами используется [@testing-library/user-event](https://testing-library.com/docs/ecosystem-user-event/).

Для подделывания запросов к API используется [Nock](github.com/nock/nock).

Синтаксис тестов и методы работы с ними обеспечивает [Jest](https://jestjs.io/docs/getting-started)

## Полезные материалы

В материалах могут встречаться другие библиотеки или методы тестирования. Поэтому этот список нужен преимущественно для ознакомления с логикой написания тестов, а за примерами иногда лучше заглянуть в `/examples`.

Ценность материалов обозначена кол-вом &starf; (где 5 - самый актуальный или полезный материал).

- [Введение в тестирование](https://www.freecodecamp.org/news/testing-react-hooks/) (&starf;&starf;&starf;&star;&star;)
- [Современные методы тестирования](https://blog.sapegin.me/all/react-testing-3-jest-and-react-testing-library/) (&starf;&starf;&starf;&starf;&starf;)
- [Распространенные ошибки](https://kentcdodds.com/blog/common-mistakes-with-react-testing-library) (&starf;&starf;&starf;&starf;&starf;)
- [Шпаргалка функций Jest](https://devhints.io/jest) (&starf;&starf;&starf;&starf;&star;)
