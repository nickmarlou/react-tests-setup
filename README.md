# Тестирование React приложений

Привет! Здесь собрана информация и примеры для удобной подготовки к тестированию React приложений.

- [Подготовка](#подготовка)
  - [Настройка](#настройка)
  - [Расположение и название тестовых файлов](#расположение-и-название-тестовых-файлов)
  - [Основной синтаксис тестов](#основной-синтаксис-тестов)
- [Вступление](#вступление)
  - [Что тестировать?](#что-тестировать)
  - [Что не тестировать?](#что-не-тестировать)
  - [Что использовать?](#что-использовать)
- [Частые вопросы](#частые-вопросы)
  - [Почему не стоит тестировать реализацию?](#почему-не-стоит-тестировать-реализацию)
  - [Как найти нужный DOM элемент?](#как-найти-нужный-dom-элемент)
  - [Как тестировать состояние/редьюсер компонента?](#как-тестировать-состояние%2Fредьюсер-компонента)
  - [Как тестировать `fetch` запросы?](#как-тестировать-fetch-запросы)
  - [Как тестировать асинхронные функции?](#как-тестировать-асинхронные-функции)
  - [Почему `getByRole` не находит нужный элемент?](#почему-getbyrole-не-находит-нужный-элемент)
  - [Что за ошибка `Should be wrapped into act`?](#что-за-ошибка-should-be-wrapped-into-act)
  - [Что за ошибка `Nock disallowed net connect`?](#что-за-ошибка-nock-disallowed-net-connect)
- [Полезные материалы](#полезные-материалы)

## Подготовка

### Установка

Добавляем необходимые `npm пакеты`:

- jest
- nock
- @testing-library/jest-dom
- @testing-library/react
- @testing-library/user-event
- babel-jest
- eslint-plugin-jest
- eslint-plugin-jest-dom
- eslint-plugin-testing-library

После этого создаем и настраиваем файлы конфигурации `jest.config.js`, `jest.setup.js`, `.eslintrc.js`, `.babelrc`. Они уже есть в этом репозитории и их можно использовать в проектах в текущем или доработанном виде

### Запуск тестов

Вызов `npm run test` запускает одноразовую проверку тестов.

Вызов `npm run test:watch` запускает слежение за изменениями в файлах тестов и их автоматическое выполнение.

### Расположение и название тестовых файлов

В проекте файлы с тестами располагаются либо в папке своего компонента, либо в папке `/tests` в корневой директории. Папка `/tests` содержит тесты страниц (`/pages`), функций-хелперов (`/helpers`) и другие.

Название тестового файла компонента:\
`ComponentName.test.js`

Название тестового файла функции:\
`functionName.test.js`

### Основной синтаксис тестов

Каждый тест оборачивается в функцию `test()` с его описанием:

```js
test('send request on form submit', () => {...})
```

Описание теста должно отражать его действие. Например, "отправляет запрос при подтверждении формы".

Так же тесты можно группировать в смысловые блоки с помощью `describe()`:

```js
describe('Main logic tests', () => {
    test('do something', () => {...})
})
```

Чтобы пропустить один или несколько тестов используется `skip`:

```js
test.skip('...', () => {...})
// ИЛИ
describe.skip('...', () => {...})
```

## Вступление

### Что тестировать?

В первую очередь, работоспособность компонентов и взаимодействие с приложением от лица юзера. Верно ли отредерены props, нажатие на кнопки, заполнение форм, открытие модалок, подзагрузка элементов и др. Тестируем результат нашего кода.

### Что не тестировать?

Как компоненты или функции написаны и сделаны. Названия переменных, структура функций, типы props, стили, сторонние библиотеки и др. Не тестируем то, как код реализован.

Сторонние библиотеки имеют свои тесты. В наших мы можем либо полностью заменить их компоненты (например, используя заглушку типа _\</ div>_), либо тестировать только самую необходимую логику (например, отправку форм из _Formik_).

### Что использовать?

Для рендера и получения элементов используется библиотека [@testing-library/react](https://testing-library.com/docs/react-testing-library/intro).

Для проверки элементов на содержимое и состояние используется [jest-dom](https://github.com/testing-library/jest-dom)

Для вызова клиентских событий используется [@testing-library/user-event](https://github.com/testing-library/user-event).

Для перехвата `fetch` запросов используется [nock](https://github.com/nock/nock).

Основной фреймворк для тестов с базовыми инструментами – [jest](https://jestjs.io/docs/getting-started)

## Частые вопросы

### Почему не стоит тестировать реализацию?

В большинстве случаев такой подход ведет к высокому шансу на ошибку и неверный результат. Допустим, мы что-то незначительно поменяли в компоненте. В итоге он всё ещё работает, но устаревший тест теперь будет говорить обратное (_False negatives_). Или наоборот, в результате изменений компонент может перестать работать, но на тесте это не отразится (_False positives_). И шансы на такие ошибки возрастают при малейшем рефакторе кода.

Поэтому мы тестируем _результат_ кода, а не его реализацию. Работаем именно с тем, что в конечно итоге дойдет до пользователя и будет непосредственно влиять на его опыт. По этой же причине мы не тестируем [снимками](https://jestjs.io/docs/snapshot-testing). Особенно приятно то, что такой подход не только надежнее, но и проще в реализации и поддержке.

Подробнее на эту тему можно почитать в [этом материале](https://kentcdodds.com/blog/testing-implementation-details).

### Как найти нужный DOM элемент?

Используем запросы `(queries)` от библиотеки [@testing-library/react](https://testing-library.com/docs/react-testing-library/cheatsheet#queries).

- Для элементов с определенной `aria` ролью используем `...ByRole` запросы.\
   Например:

  ```js
  screen.getByRole('button', { name: 'Some text' });
  ```

  В этом примере мы, во-первых, будем уверены в том, что найденный элемент является именно кнопкой, а во-вторых, проверяем ее же доступность. Список [ARIA Roles](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles).

- Для разных элементов есть более подходящие `queries`, чем другие. Например, для полей форм лучше использовать `getByLabelText`.
- Различия между `getBy...`, `queryBy...` и `findBy...`:
  - `getBy...` находит элемент или выбрасывает ошибку, если он не найден.
  - `queryBy...` находит элемент или возвращает `null`, если он не найден (удобно для проверки на отсутствие).
  - `findBy...` возвращает выполненный промис c элементов, если он был найден за определенное время (удобно в случае, когда элемент рендерится не сразу).
- Для получения сразу нескольких элементов в виде массива используются `getByAll...`, `queryByAll...` и `findByAll...`.

### Как тестировать состояние/редьюсер компонента?

Оригинальные `state` или `reducer` не нужно использовать или повторять, проверяем и оцениваем только результат их работы. Например то, что при клике на кнопку должна появиться модалка.

Примеры есть в\
[/examples/component-with-state](/examples/component-with-state)\
[/examples/component-with-reducer](/examples/component-with-reducer)

### Как тестировать `fetch` запросы?

С помощью HTTP перехватчика [nock](github.com/nock/nock). Эта библиотека помогает перехватить настоящие запросы и вернуть любой желаемый ответ. Она удобна тем, что нам не нужно задумываться, как мокать функцию для вызова запроса, ведь можно работать с ним напрямую.

Например:

```js
nock(BASE_API_URL).get(API_PATH).reply(200, { some_data: [] });
```

Примеры использования `nock` в тестах:\
[/examples/component-with-form](/examples/component-with-form)\
[/examples/component-with-uploading](/examples/component-with-uploading)

### Как тестировать асинхронные функции?

В тестах они являются частой причиной поломки, ведь тестирование происходит в синхронном режиме. Правильный подход в том, чтобы дождаться выполнения всех асинхронных колбеков перед завершением теста. Сделать это можно при помощи фунции-промиса `await waitFor()`.

Например, вызываем асинхронную функцию, которая меняет какое-то состояние, и затем проверяем результат ее выполнения в обертке `waitFor()`:

```js
userEvent.click(openDialogButton); // Клик по кнопке с асинхронным обработчиком

await waitFor(() => {
  expect(screen.getByRole('dialog')).toBeVisible();
});
```

Такой способ (когда мы ожидаем выполнения каких-то действий в `waitFor()` после асинхронного колбека) является самым предпочтительным решением. Однако есть и другие варианты:

- Дождаться появления каких-то элементов с помощью асинхронного `findBy...`.
- Обернуть асинхронное действие в `act()`. Но такой способ используется [в очень редких](https://kentcdodds.com/blog/fix-the-not-wrapped-in-act-warning) случаях.
- Поставить вызов `await waitFor(() => {})` с пустым колбеком в конец теста. Это заставит тест дождаться завершения всех асинхронных действий перед закрытием. Хоть это и своего рода хак, но иногда его можно использовать.

### Почему `getByRole` не находит нужный элемент?

Функция `getByRole` используется для поиска элемента с явной ARIA ролью и описанием. Но если у элемента нет описания или он не доступен для пользователя, то эта функция вернёт ошибку. Способы решения:

- Если элемент скрыт – передаем в `getByRole` параметр `hidden: true`.
- Если у элемента нет описания – добавляем его любым удобным способом. Например, у тега `<form />` по умолчанию нет ARIA роли `form`, пока у него не будет явного описания. Мы можем это исправить добавив его, например, с помощью `aria-label`.

### Что за ошибка `Should be wrapped into act`?

Такая ошибка часто возникает из-за оставшихся асинхронных действий после завершения теста. Её можно встретить при тестах форм под управлением `Formik` или при вызове асинхронных колбеков. Решение описано [выше](#как-тестировать-асинхронные-функции)

### Что за ошибка `Nock disallowed net connect`?

Произошел HTTP запрос, не обработанный `nock`. Все реальные запросы в тестах запрещены, поэтому если появляется данная ошибка, нужно добавить перехватчик запроса _перед_ его вызовом.

**Внимание!** URL запроса в `nock` должен совпадать с настоящим, чтобы перехватчик мог его обработать.

## Полезные материалы

В этих материалах могут встречаться другие библиотеки или методы тестирования. Поэтому этот список скорее нужен для ознакомления с правильными подходами к написанию тестов, а за примерами иногда лучше заглянуть в [/examples](/examples).

- [Введение в тестирование](https://www.freecodecamp.org/news/testing-react-hooks/) (есть устаревшее)
- [Современные методы тестирования](https://blog.sapegin.me/all/react-testing-3-jest-and-react-testing-library/)
- [Распространенные ошибки и их решения](https://kentcdodds.com/blog/common-mistakes-with-react-testing-library)
- [Почему нужно тестировать результат, а не реализацию](https://kentcdodds.com/blog/testing-implementation-details)
- [Список `expect` методов библиотеки Jest](https://jestjs.io/docs/expect)
- [Список сопоставителей `(matchers)` библиотеки jest-dom](https://github.com/testing-library/jest-dom)

Так же советую почитать статьи от одного из лучших мастеров тестирования – [Kent C. Dodds](https://kentcdodds.com/blog/?q=testing).
