# react-hooks-new

<h2>Что такео useRef()</h2>

- [x] С помощью `useRef()` можно по ссылке получить доступ к DOM-элементам, чтобы использовать их в `useEffect()` или `обработчиках событий`, ожидая, что при работе с этими элементами мы не хотим ререндера компонентов.

  + Иногда в компонентах нам нужен доступ к `DOM-элементам` — например, чтобы вызывать методы этого элемента. 
  +	`Элементы DOM не являются частью React`, поэтому напрямую в компоненте к ним нет доступа. 
  +	Но для этого есть хук `useRef()`, который позволяет получить доступ к таким элементам.
     
```javascript
  function App() {
  // Создаем переменную, в которую будет помещена ссылка на DOM-элемент
    let squareRef = useRef();

// При помощи св-ва ref={squareRef} - создаем ссылку на div-элемент
    return (
      <div className="square">
        <div className="square-black" ref={squareRef}></div>
      </div>
    );
  }

export default App;

```
     
<hr>
<br>
<br>

<h2>Какие проблемы решает useRef()?</h2>

- [ ] Нам нужно изменить внешний вид элемента внутри нашего компонента. Мы можем изменить его внешний вид при помощи работы с состоянием компонент = проблема - мы получаем ререндер всего компонента. Нам это не нужно.

- [ ] Нам нужно хранить какие-то значения внутри жизненного цикла компонента. Хранение мутируемого значения.

<hr>
<br>
<br>

<h2>Как работает хук useRef.</h2>

+ Хук `useRef()` создает `ссылку` она `объект в DOM-дереве` в качестве JS объекта, который имеет св-во `current`, внутри которого содержатся методы этого элемента и св-ва.

+ При помощи это ссылки мы можем обращаться / работать "напрямую" с `DOM-элементом`. ПРИМ. React не позволяет работать напрямую с такими элементами.

+ Хук `useRef()` позволяет работать с элементами без дополнительных рендеров компонента.

<hr>
<br>
<br>

<h2>Примеры</h2>

[ПРИМЕР С КВАДРАТАМИ](https://codesandbox.io/p/sandbox/ls-1-task-answer-forked-tndkg2?file=%2Fsrc%2FApp.js%3A14%2C1)

[ПРИМЕР С ФОКУСАМИ НА ДВУХ ИНПУТАХ](https://codesandbox.io/p/sandbox/infallible-visvesvaraya-7nxysv?file=%2Fsrc%2FApp.js)

<hr>
<br>
<br>

<h2>Нюансы работы</h2>

- [ ] Ссылка на элемент создается в момент первого рендера компонента.

  + первый console.log(ref) выдаст `null`(если это инициализированное зн-е) или `undefned` если инициализации небыло.
  + можем вызвать console.log(ref) при первом рендере с помощью `useEffect()` тк он срабатывает как раз при первом рендере и выдаст ссылку на элемент.
     
```javascript
    let squareRef = useRef(null);

    // Выводим зн-е current элемента при первом рендере в консоль
    // Ссылка создается в момент первого рендера и изначально = null
    // Поэтому в консоль ее получим с помощью useEffect при первом рендере
    useEffect(() => {
      console.log(squareRef.current);
    }, []);

    // А тут в консоль выведется null, тк этот вывод будет срабатывать до рендера
    console.log(squareRef)
```  

<br>

- [ ] Ссылка на элемент создается при первом рендере и хранится до тех пор пока компонент не будет удален. (жизненный цикл компонентов помним)

	+ Когда ссылка на элемент будет получена - мы можем видеть все ее методы / св-ва в свойстве `current` в период всего жизненного цикла компонента.
	+ В моменты повторных рендеров - этот элемент так же не будет пересоздаваться, а будет хранить свои зн-я внутри `current`.
	+ Если удалить компонент или обновить страницу - компонент удалится или будет пересоздан и ссылка будет создана заново.
		+ В этом можно убедиться выводя переменную с ссылкой на элемент в консоль в момент каждой перерисовки компонента => элемент не изменяется.
     
<br>

- [ ] Изменение ссылки не приводит к рендеру компонента!

<br>

- [x] Используя ссылку, вы гарантируете, что:

	+ Вы можете хранить информацию между повторными рендерингами (в отличие от обычных переменных, которые сбрасываются при каждом рендеринге).
	+ Его изменение не приводит к повторному рендерингу (в отличие от переменных состояния, которые запускают повторный рендеринг).
	+ Информация является локальной для каждой копии вашего компонента (в отличие от внешних переменных, которые являются общими).

- [x] Изменение ссылки не приводит к повторному рендерингу, поэтому ссылки не подходят для хранения информации, которую вы хотите отобразить на экране. Вместо этого используйте состояние
