# React справочник

## Основы

При разработке React-приложений обычным является способ разбиения различных компонент на отдельные блоки и последующей композиции их в более сложную структуру. Часто разбивают как компоненты интерфейса, так и вспомогательные компоненты, реализующие определенную логику приложения.

React использует препроцессор JSX, который удобен тем, что можно смешивать код JavaScript и язык разметки в одном файле.

Пример использования JSX:
```javascript
const el = <h1>Hello World</h1>;
```
В примере выше создается легковесный объект по сравнению со стандартным <h1>. Эта техника называется Virtual DOM.

В JSX если элементов несколько, код оборачивается в круглые скобки, также обязательно должен быть элемент-контейнер, который будет вмещать в себя список других элементов:

```javascript
const el = (
	<div>
		<h1>First</h1>
		<h2>Second</h2>
	</div>
	);
```


После установки необходимых библиотек, не забываем импортировать библиотеку React:
		`import React from 'react';`
Также в файле верхнего уровня, где идет рендеринг в реальный DOM-элемент импортируем:
		`import ReactDOM from 'react-dom';`

Часто такую конструкцию можно встретить в файле `index.js`:
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/app/app';

//рендерим React компонент App в DOM-элемент с id = 'root'
ReactDOM.render(<App />, document.getElementById('root'));
```

Пакет `react-dom` предоставляет специфические для DOM методы, которые могут быть использованы на верхнем уровне вашего приложения. Кроме этого, эти методы можно использовать в качестве лазейки, чтобы выйти из модели React, если вам будет это нужно. Поэтому большинство из ваших компонентов не должны использовать этот модуль.

`ReactDOM.render()` управляет содержимым передаваемого вами узла контейнера. Любые существующие элементы DOM внутри заменяются при первом вызове. Более поздние вызовы используют алгоритм отслеживания изменений React DOM для эффективного обновления.

`ReactDOM.render()` не изменяет узел контейнера (изменяет только дочерние элементы контейнера). Если нужно, можно вставить компонент в существующий узел DOM без перезаписи существующих дочерних элементов.


## React элементы и компоненты

React элементы это просто переменные, которые хранят какую-либо разметку. Например в примере из раздела выше:
```javascript
const el = <h1>Hello World</h1>;
```
`el` является *элементом*

В React есть также понятие *компонентов* - независимых блоков пользовательского интерфейса, у которых может быть собственное поведение. Реализация компонентов в React обычно производится через функции или классы.



**Важно!** Компоненты всегда должны иметь название с буквы верхнего регистра.

React-компоненты используются в JSX в виде тэгов, например: `<SomeReactComponent />`

Пример React-компонент, реализованных через стрелочные функции:


например:
```javascript
const FunctionalComponentName = () => {
		//...code
};
```
В случае компонента-класса записывается так:
```javascript
class ClassComponentName extends React.Component {

	//...code
	render () {
		//здесь будет рендеринг

		};

};
```
У компонента-класса всегда должна быть render-функция. Компоненты классы как правило используются для тех случаев, когда нужно манипулировать внутренним состоянием компоненты. С появлением хуков функции теперь тоже могут манипулировать своим состоянием.

### Передача параметров компонентам | props, children

Каждому React компоненту передается объект `props`, из которого можно получить значения переданных параметров. Даже если компоненту не передано никаких свойств, объект `props` все равно будет существовать, просто пустым.

пример передачи и получения значения из объекта props:
```javascript
<FunctionalComponentName param="Value" />

const FunctionalComponentName = (props) => {
		console.log(props.param);
};
```

в данном случае `props.param` - обращение к объекту `props` и извлечение его значения по имени свойства

пример с использованием деструктурирующего присваивания:

```javascript
<FunctionalComponentName param="Value" />


const FunctionalComponentName = ({ param }) => {
		console.log(param);
};
```


Компонент-класс также может получить доступ к пропсам:

```javascript
class ClassComponentName extends React.Component {

	let {param} = this.props;

	Boolean(param === this.props.param); //true

	//...code
};
```

**Передача компонентам свойств с помощью спред оператора**

В React компоненты можно передавать весь объект сразу разложенным. То есть ключи этого объекта станут названием переданных свойств (props) а значения ключей соответственно переданными значениями.

Например:

```javascript
import React from 'react';
import SomeComponent from '../component/some-component';


//компоненту SomeComponent предадутся свойства label и important с соответствующими значениями
const App = () => {

	const item = {
		label: "First label",
		important: false
	}

	return <SomeComponent {...item} />
}
```

В пропсы можно также передавть другие компоненты, например:

```javascript
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}      </div>
      </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
    	}
      right={
        <Chat />
    	} />
  );
}
```

## Передача компонентам параметров как children

Компонентам можно также передавать, например, другие компоненты как children:

```javascript
//этот компонент будем передавать
const SomeComp = () => {
	return <div>Some div</div>;
}

const CompContainer = () => {
		//получаем доступ к children
	return <div>{this.props.children}</div>;

//SomeComp передается в children компонента CompContainer
const CompRender = () => {
	return (
		<CompContainer>
			<SomeComp />
		</CompContainer>
		);
}

```
в `this.props.children` может быть любой тип данных: строка, массив, объект, функция, дерево элементов React и т.д.

Если передано несколько дочерних элементов, получить к ним доступ можно по индексу.

В React есть API для работы с `children`, очень похожая на классический `map` при работе со стандартными массивами.

```javascript
const SomeComp = () => {
	return <div>Some div</div>;
}

const CompContainer = () => {

		//получаем доступ к children

	return (<div>
		{
			React.Children.map(this.props.children, (child, i) => {
				//будет вызвано для каждого child
				return <div>{child} индекс {i}</div>
			})
		}
		</div>;)

//SomeComp передается в children компонента CompContainer
const CompRender = () => {
	return (
		<CompContainer>
			<SomeComp />
			<SomeComp />
			<SomeComp />
		</CompContainer>
		);
}

```

Если требуется изменить какое-либо свойство элемента, необходимо использовать метод React `cloneElement()` который полностью скопирует элемент, после чего можно изменять копии.


```javascript
const SomeComp = () => {
	return <div>Some div</div>;
}

const CompContainer = () => {

		//получаем доступ к children

	return (<div>
		{
			React.Children.map(this.props.children, (child, i) => {
				//будет вызвано для каждого child
				// добавляем дополнительное свойство копии итерируемой сущности
				return React.cloneElement(child, { item })
			})
		}
		</div>;)

//SomeComp передается в children компонента CompContainer
const CompRender = () => {
	return (
		<CompContainer>
			<SomeComp />
			<SomeComp />
			<SomeComp />
		</CompContainer>
		);
}

```



Пример с React элементом:

```javascript

const loginBox = <span>Log in please</span>;

const App = () => {
	return (
		<div>
			{ loginBox } //вставляем React элемент
			//нужно обратить внимание, что объекты нельзя использовать в качестве React child
			//допустимо, если значением элемента является null, undefined, true или false (будут проигнорированы)
			<span>span</span>
		</div>
		)
}
```

Пример использования логики отображения в зависимости от условий с помощью тернарного оператора:

```javascript
const isLoggedIn = true;
const loginBox = <span>Log in please</span>;

const App = () => {
	return (
		<div>
			{isLoggedIn ? null : loginBox }
			<span>span</span>
		</div>
		)
}
```

Пример использования логики отображения в зависимости от условий с помощью оператора &&:

```javascript
const isLoggedIn = true;

const App = () => {
	return (
		<div>
			{isLoggedIn && (<span>You are logged in !</span>) }
			<span>span</span>
		</div>
		)
}
```



Также возможен рендеринг массива JSX в фигурных скобках:

```javascript
import React from 'react';

const App = () => {
	const todos = ['first task', 'second task', 'third task'];
	return (
		<div>
			<SomeComponent todos={todos} />
		</div>
		)
}

const SomeComponent = ({todos}) => {
	const elements = todos.map(task => {
		return (
			<li>
				{task}
			</li>
		);
	});
	return (
		<ul>
			{elements} //рендеринг массива
		</ul>
	);

};
```

## Ref. Референции

Для непосредственного доступа к нативным элементам DOM в React есть специальные возможности.

В компоненте, который необходимо связать для манипуляций через React можно добавить специальный пропс ref, реф в React создается с помощью встроенной функции `React.createRef()`. Как правило они используются для узкого круга специализированных задач, например для задания фокуса на конкретном элементе.

Пример без прямого вызова встроенной функции:

```javascript
class ExampleRef extends React.Component {
	//инициализируем поле inputRef, в котором будем хранить ссылку на реф
	this.inputRef;

	focus = () => {
		//работаем с DOM элементом
		this.inputRef.focus();
	}

	  render() {
    // описываем, что мы хотим связать реф <input>
    // с `textInput` созданным в конструкторе
    return (
      <div>
        <input
          type="text"
          ref={(inputRef) => this.inputRef = inputRef}
          />
      </div>
    );
  }
}


```


В примере ниже используется `React.createRef()`. При таком подходе в пропс ref уже не передается callBack функция.


```javascript
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // создадим реф в поле `textInput` для хранения DOM-элемента
    this.textInput = React.createRef();
    this.focusTextInput = this.focusTextInput.bind(this);
  }

  focusTextInput() {
    // Установим фокус на текстовое поле с помощью чистого DOM API
    // Примечание: обращаемся к "current", чтобы получить DOM-узел
    this.textInput.current.focus();  }

  render() {
    // описываем, что мы хотим связать реф <input>
    // с `textInput` созданным в конструкторе
    return (
      <div>
        <input
          type="text"
          ref={this.textInput} />        <input
          type="button"
          value="Фокус на текстовом поле"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}

```




## Context API

В React есть возможность передавать различным в иерархии компонентам некий общий контекст, который может быть им нужен. Это удобно и исключает последовательную передачу пропсов от родителя к дочерним компонентам.


Например, есть общий сервис `DataProcessor`, который обращается к серверу за данными, выполняет какие-либо манипуляции, либо есть какой-либо признак `isAuthenticated`, который должен быть доступен всему дереву элементов для того, чтобы знать аутентифицирован ли пользователь.

Реализуется в React это следующим образом.



```javascript
import React from 'react';

// для создания контекста используем встроенную фунцию createContext
// может принимать опционально аргумент, который выступает в качестве значения по умолчанию, если Consumer не найдет Provider


//возвращает два компонента Provider и Consumer. Их можно сразу деструктурировать.

const { Provider, Consumer } = React.createContext();

//также распространена практика переименовывания этой пары так, чтобы название более точно передавало смысл или контекст, который предоставляют данные компоненты

const {
	Provider: DataProcessorProvider,
	Consumer: DataProcessorConsumer
} = React.createContext();

//не забываем про экспорт

export {
	DataProcessorProvider,
	DataProcessorConsumer
};

```

Теперь подключаем контекст к приложению


```javascript
//корневое приложение App
import React from 'react';
//импортируем провайдера
import { DataProcessorProvider } from '/context-file.js';
import { getData } from '../services/data-processor.js'



export default class App extends React.Component {

	state = {
		someKey: someValue
	};

	render() {

		return (
			//оборачиваем провайдером дерево компонентов и в качестве пропса
			//value в провайдер передаем необходимый контекст
			<DataProcessorProvider value={getData}>
				<div id="main">
				<Header />
				<SomeComponent/>
				<OtherComponent/>
				<SomeComponent2>
					<InnerComponent/>
					<InnerComponent2/>
				</SomeComponent2>
				</div>
			</DataProcessorProvider>

			)
	}


}
```

Получение контекста в компонентах


```javascript
//компонент InnerComponent
import React from 'react';
//импортируем консьюмера
import { DataProcessorConsumer } from '/context-file.js';




const InnerComponent = () => {


	return (
		//оборачиваем в консьюмер дочерние компоненты
		//консьюмер принимает в качестве тела функцию, аргументом которой является контекст
		<DataProcessorConsumer>
			{
				(getData) => {
					return (
								<div>
									<InnerLevel2Component func={getData} />
									<InnerLevel2Component func={getData} />
								</div>
							)
						}
			}

		</DataProcessorConsumer>
		);
}
```

Контекст часто используеся с HOC.

Пример оборачивания компонента с помощью HOC для присоединения ему контекста.


```javascript
//компонент with-data-processor-consumer
import React from 'react';
//импортируем консьюмера
import { DataProcessorConsumer } from '/context-file.js';

//функция withDataProcessorConsumer в качестве аргумента Wrapped принимает оборачиваемый компонент

export const withDataProcessorConsumer = (Wrapped) => {

	return (props) => {
		return (
				<DataProcessorConsumer>
				{
					(getData) => {
						return (
							<Wrapped {...props} getData={getData} />
							)
					}

				}
				</DataProcessorConsumer>
			);



	}

};



```

Оборачиваем необходимый компонент:


```javascript
//компонент InnerComponent
import React from 'react';
//импортируем HOC
import { withDataProcessorConsumer } from '/hoc'



//достаем из пропсов переданный из контекста с помощью HOC метод

const InnerComponent = ({ getdata }) => {


	return (
		//теперь консьюмер не нужен, так как он предоставляется из HOC
		<div>
			<InnerLevel2Component func={getData} />
			<InnerLevel2Component func={getData} />
		</div>
		);
}


//Оборачиваем компонент

export withDataProcessorConsumer(InnerComponent);

```


## Особенность рендеринга коллекции элементов | Reconciliation algorithm

Когда мы в JSX вставляем массив элементов React проверяет есть ли у итерируемой записи в html тэге свойство `key`.

Например:

elements = [
<li>
	{someItem}
</li>,
<li>
	{someItem2}
</li>
]


<ul>
	{elements}
</ul>

В данном случае у тэга `li` нет свойства key, где бы содержался уникальный идентификатор. React при каждом рендеринге проверяет каждый элемент коллекции на предмет изменений. Сначала отбираются элементы с одинаковым ключом, затем сравнивается их значение, если значение не совпадает, этот элемент перерисовывается, иначе остается как есть. Этот подход называется `reconciliation algorithm` и служит для оптимизации производительности.

Пример оптимизации:

elements = [
<li key="id1">
	{someItem}
</li>,
<li key="id2">
	{someItem2}
</li>
]

<ul>
	{elements}
</ul>

## JavaScript выражения в коде JSX

Для того, чтобы выполнить код, или получить значение какой-либо сущности (переменной, объекта, массива и т.п.) в коде JSX JavaScript код заключается в фигурные скобки:

```javascript
const label = 'текущее время';

const App = () => {
	return (
		<div>
			<span>{label}</span>
			<span>{ (new Date()).toString() }</span>
		</div>
		)
}
```

Аналогично работает для HTML атрибутов:

```javascript
const SearchPanel = () => {
	const searchText = 'Type here to search';
	return <input placeholder={searchText} />;
}
```
в JSX следующие 2 атрибута именуются отлично от HTML:

`className` (class в HTML) и `htmlFor` (for в HTML)

## Передача компонентам других компонент как props

В `props` можно передавать не только React элементы, но и любые объекты.

```javascript
//передача
const PeoplePage = ({history, match}) => {
    const {id} = match.params

        return (
            <Row
            left={<PersonList onItemSelected={ (id) => {
                history.push(id)
            }
            }/>}
            right={<PersonDetail itemId={id}/>} />
        )
}
```

```javascript
import React from 'react';
import PropTypes from 'prop-types';
//элемент контейнер для обертки других компонент
const Row = ({left, right}) => {
    return (
        <div className="list-data">
        <div className="items-list">
            {right}
        </div>
        <div className="items-list2">
            {left}
        </div>
    </div>
    )
}
//дополнительно проверяем, что тип переданных параметров в props это все, что может быть отрендерено
// числа, строки, элементы или массивы
// (или фрагменты) содержащие эти типы
Row.propTypes = {
    left: PropTypes.node,
    right: PropTypes.node
}
```



В JSX коде можно использовать JS-выражения, заключив их в фигурные скобки:

```javascript
const header = 'Header';
const elem = <h1>{header}</h1>;
```


## Состояние компонентов

**Состояние у компонента-класса**

Состояния компонентам нужны, чтобы отображать картину в приложении, соответствующую изменяющимся условиям.

У компонентов-классов состояние задается с помощью свойства state:

Например, с иcпользованием конструктора:
```javascript
class ExampleComponent extends React.Component {
	constructor(props) {
	super(props);
	this.state = {prop: 15};
			}
	render () {
			return <div>{this.state.prop}</div>;
		}
}
```
В классе `state` всегда является объектом.
Пропс передается базовому (родительского класса) конструктору. Можно использовать и без конструктора.
После того как первый раз `state` был проинициализирован, его больше нельзя изменять напрямую. Его можно читать напрямую. Для установки стейта используется функция `setState`, которая в классе вызывается так:

```javascript
class ExampleComponent extends React.Component {
	constructor(props) {
	super(props);
	this.state = {flag: true, text: 'div'};
			}
	onClickListener = () => {
		this.setState({
			flag: !flag
		})
	}
	render () {
			return <div isFlagged={this.state.flag} onClick=> >{this.state.text}</div>;
		}
}
```

В функцию `setState()` следует передавать на вход объект только теми ключами и значениями, которые нужно изменить.

**Важный момент**. Если обновление одного из свойств `state` *зависит* от предыдущего значения этого свойства, необходимо на вход передавать функцию:

```javascript
onMarkImportant = () => {
	this.setState( (state) => {
		return {
			important: !state.important
		};
	});
};
```
пример с деструктуризацией:
```javascript
onLabelClick = () => {
	this.setState( ({done}) => {
		return {
			done: !done //меняем на противоположное значение
		};
	});
};
```

Это необходимо выполнить, так как функция `setState` по своей природе асинхронна и в момент обновления текущее состояние может быть не конечным.

Еще вариант работы с `setState` с учетом ее асинхронности:

```javascript

this.setState({key: 'value'}, () => {
    // Данный блок исполнится после обновления стейта
});
```
## Хуки

**Состояние у компонента-функции**

Изначально в React не было возможности компонентам-функциям использовать состояние. Но это изменилось с появлением хуков (hooks).

Хуки это функции, которые добавляют функциональность манипулирования состоянием внутри компонентов-функций.

Инициализация хуков производится следующим образом:

для начала не забываем импортировать их:
```javascript
	import React, { useState } from 'react';

	const FunctionalComponentName = () => {
			const [a, setA] = useState('Initial value of a') //инициализируем состояние и функцию, изменяющую его
			setA('Next value of a') //изменяем значение a

			return <div>Now a: {a}</div>;
	}
```
В примере выше useState - это хук, инициализирующий состояние.

Также в компонентах-функциях хуки можно использователь более одного раза:
```javascript
const FunctionalComponentName = () => {
		const [a, setA] = useState('Initial value of a');
		const [b, setB] = useState('Initial value of b');
		setA('Next value of a');
		setB('Next value of b');
		return (
			<div>
				<span>Now a: {a}</span>
				<span>Now b: {b}</span>
			</div>
			);
}
```

### useEffect

Данный хук используется как аналог методов жизненного цикла в классовых компонентах. useEffect всегда вызывается при первом рендере, также через него можно задать логику рендеринга компонента при изменении состояния данного компонента. То есть данных хук используется для отслеживания рендера компонента.

Базовый синтаксис:

```javascript
function App() {
  const [type, setType] = useState('users')

  useEffect(() => {
    console.log('render')
  })

  return (
    <div>
      <h1>Ресурс: {type}</h1>
      <button onClick={() => setType('users')}>Пользователи</button>
      <button onClick={() => setType('todos')}>Todos</button>
      <button onClick={() => setType('posts')}>Посты</button>
    </div>
  )
}
```

Если необходимо, чтобы `useEffect` срабатывал только при изменении каких-либо данных из состояния компонента, то используется также второй аргумент `deps`, который принимает массив необходимых зависимостей.

Пример:

```javascript
function App() {
  const [type, setType] = useState('users')

   useEffect(() => {
    console.log('Type change: ', type);
  }, [type]) // в массив зависимостей передан type, а значит useEffect сработает только при изменении type

  return (
    <div>
      <h1>Ресурс: {type}</h1>
      <button onClick={() => setType('users')}>Пользователи</button>
      <button onClick={() => setType('todos')}>Todos</button>
      <button onClick={() => setType('posts')}>Посты</button>
    </div>
  )
}
```

#### Эмуляция ComponentDidMount

Если необходимо реализовать какую-либо логику при первом рендере компонента без необходимости в дальнейшем ее использовать, то следует в качестве аргумента `deps` передать пустой массив:

```javascript
function App() {
  const [type, setType] = useState('users')

   useEffect(() => {
    console.log('Type change: ', type);
  }, [type])

	useEffect(() => {
    console.log('ComponentDidMount');
  }, [])

  return (
    <div>
      <h1>Ресурс: {type}</h1>
      <button onClick={() => setType('users')}>Пользователи</button>
      <button onClick={() => setType('todos')}>Todos</button>
      <button onClick={() => setType('posts')}>Посты</button>
    </div>
  )
}
```

Такой подход нередко используется когда нужно назначить какого-либо слушателя на определенное событие.

#### Определение и уничтожение слушателя

Для начала зададим слушателя события на глобальный объект window и зададим новый стейт, в котором будет хранится позиция мыши:

```javascript
function App() {
  const [type, setType] = useState('users')
  const [pos, setPos] = useState({
    x: 0, y: 0
  })

  // Определим функцию-обработчик события движения мыши
  // такое определение необходимо для дальнейшего удаления слушателя

  const mouseMoveHandler = event => {
    setPos({
      x: event.clientX,
      y: event.clientY
    })
  }

   useEffect(() => {
    console.log('Type change: ', type);
  }, [type])

	useEffect(() => {
    console.log('ComponentDidMount')
	// при первом рендере компонента добавляем слушатель
    window.addEventListener('mousemove', mouseMoveHandler)

    return () => {
	// данная функция позволяет очистить слушателя
	// функция будет вызвана когда сам компонент будет уничтожен
      window.removeEventListener('mousemove', mouseMoveHandler)
    }
  }, [])

  return (
    <div>
      <h1>Ресурс: {type}</h1>
      <button onClick={() => setType('users')}>Пользователи</button>
      <button onClick={() => setType('todos')}>Todos</button>
      <button onClick={() => setType('posts')}>Посты</button>
    </div>
  )
}
```

### useRef

Данный хук возвращает объект со свойством `current`, значение в которое присвается по дефолту. Дефолтное значение передается в качестве аргумента в хук `useRef`.

Отличительная особенность `useRef` в том, что изменение значения указывающего на него объекта.

Данный хук используется в основном в следующих случаях:
1. Если необходимо использовать какое-либо значение, изменение которого не приводит к изменению стейта (а значит и к ререндеру), но которое необходимо сохранять для каких-либо дальнейших манипуляций.
2. Получение ссылок на какие-либо DOM-элементы. Такое поведение часто используется для задания фокуса на элементы.
3. Получение значения предыдущего стейта.

Иллюстрация использования useRef по всем вышеописанным кейсам:

```javascript
function App() {
  const [value, setValue] = useState('initial')
  // кейс #1
  const renderCount = useRef(1)
  // кейс #2
  const inputRef = useRef(null)
  // кейс #3
  const prevValue = useRef('')

  useEffect(() => {
    renderCount.current++
  })

  useEffect(() => {
    prevValue.current = value
  }, [value])

  // объект inputRef это ссылка на конкретный DOM-элемент со всеми его свойствами
  const focus = () => inputRef.current.focus()

  return (
    <div>
      <h1>Количество рендеров: {renderCount.current}</h1>
      <h2>Прошлое состояние: {prevValue.current}</h2>
	  {/* ref={inputRef} это связь объекта inputRef с DOM-элементом input */}
      <input ref={inputRef} type="text" onChange={e => setValue(e.target.value)} value={value} />
      <button className="btn btn-success" onClick={focus}>Фокус</button>
    </div>
  )
}
```

### useCallback

Возвращает мемоизированный колбэк. Передайте встроенный колбэк и массив зависимостей. Хук `useCallback` вернёт мемоизированную версию колбэка (ссылки на функцию), который изменяется только, если изменяются значения одной из зависимостей. Это полезно при передаче колбэков оптимизированным дочерним компонентам, которые полагаются на равенство ссылок для предотвращения ненужных рендеров (например, shouldComponentUpdate).



Пример использования хука:

```javascript
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

Хук `useCallback` похож на хук `useMemo` с тем отличием, что он возвращает мемоизированный колбэк (ссылку на функцию), который будет обновлен, только если одна из зависимостей будет изменена.

В примере выше можно увидеть, как происходит процесс мемоизации функции. Достаточно в первый параметр передать функцию, которую требуется мемоизировать, поместив ее в колбек и передав ей зависимые параметры.

Вторым параметром `useCallback` является массив зависимых параметров, благодаря которому будет происходить мемоизация данного колбэка. Если один из переданных параметров изменится, то `useCallback` вернет новую мемоизированную функцию, при этом, если зависимые параметры не будут изменяться в функциональном компоненте React, то будет использоваться копия данной функции, что позволит оптимизировать применение памяти реактом.

#### Применение на практике

Если посмотреть на пример:

```javascript
function App() {
  const [colored, setColored] = useState(false)
  const [count, setCount] = useState(1)

  const styles = {
    color: colored ? 'darkred' : 'black'
  }
// пример НЕмемоизированной функции
  const generateItemsFromAPI = () => {
	  return new Array(count).fill('').map((_, i) => `Элемент ${i + indexNumber}`
  }

  return (
    <>
      <h1 style={styles}>Количество элементов: {count}</h1>
      <button className={'btn btn-success'} onClick={() => setCount(prev => prev + 1)}>Добавить</button>
      <button className={'btn btn-warning'} onClick={() => setColored(prev => !prev)}>Изменить</button>

      <ItemsList getItems={generateItemsFromAPI} />
    </>
  )
}
```
То складывается ощущение, что все работает хорошо и правильно, но это не так.

Проблема в том, что при ререндере родительского компонента (например, если изменилось что-либо в его стейте) будет также заново вызываться и ререндериться дочерний компонент, даже несмотря на то, что не изменилась его зависимость (так как изменилась ссылка на описанную функцию), то есть по примеру выше: count остался прежним.
Если же в функции еще используется какое-либо API по взаимодействию с сервером, то это начнет представлять уже более серьезные проблемы, так как появятся ненужные вызовы и соответственно замедление производительности.

Для решения данной проблемы используется хук `useCallback`:

```javascript
function App() {
  const [colored, setColored] = useState(false)
  const [count, setCount] = useState(1)

  const styles = {
    color: colored ? 'darkred' : 'black'
  }
// теперь мы обернули функцию в колбэк и присвоили ее ссылку generateItemsFromAPI
  const generateItemsFromAPI = useCallback(
    (indexNumber) => {
    return new Array(count).fill('').map((_, i) => `Элемент ${i + indexNumber}`)
  }, [count])

  return (
    <>
      <h1 style={styles}>Количество элементов: {count}</h1>
      <button className={'btn btn-success'} onClick={() => setCount(prev => prev + 1)}>Добавить</button>
      <button className={'btn btn-warning'} onClick={() => setColored(prev => !prev)}>Изменить</button>

      <ItemsList getItems={generateItemsFromAPI} />
    </>
  )
}
```
Если проводить аналогию с `useMemo`, отличие в том что `useCallback` возвращает именно саму мемоизированную функцию-callback (ссылку на нее), а не вычисленное значение функции. `generateItemsFromAPI` хранит ссылку на переданную функцию и ссылка не будет изменена, пока не будет изменена хотя бы одна из
зависимостей, переданных в массиве в качестве второго аргумента для `useCallback`.

Также при необходимости можно использовать аргументы при использовании `useCallback` (как в примере выше), если подразумевается, что данный колбэк будет вызван с каким-либо аргументом.

### useMemo

Данный хук используется для кэширования какого-либо значения, возвращаемого функцией, то есть мемоизирует результат вызова переданной функции и при необходимости вызывает переданную функцию заново, если что-либо изменилось в массиве зависимостей. Более наглядно на примере:


```javascript
// имитируем высокозатратные вычисления
function complexCompute(num) {
  console.log('complexCompute')
  let i = 0
  while (i < 1000000000) i++
  return num * 2
}
// базовый функциональный компонент
function App() {
  const [number, setNumber] = useState(42)
  const [colored, setColored] = useState(false)

  const styles = useMemo(() => ({
    color: colored ? 'darkred' : 'black'
  }), [colored])


// если оставить так, то функция complexCompute будет вызываться каждый раз при рендере App, даже если аргумент number не изменился
  const computedNotOptimized = complexCompute(number);

// здесь было запомнено значение вычисленное функцией complexCompute, повторных вызовов complexCompute не будет
// пока не изменится значение аргумента number, переданного в массив зависимостей
  const computed = useMemo(() => {
    return complexCompute(number)
  }, [number])

  useEffect(() => {
    console.log('Styles changed')
  }, [styles])

  return (
    <>
      <h1 style={styles}>Вычисляемое свойство: {computed}</h1>
      <button className={'btn btn-success'} onClick={() => setNumber(prev => prev + 1)}>Добавить</button>
      <button className={'btn btn-danger'} onClick={() => setNumber(prev => prev - 1)}>Убрать</button>
      <button className={'btn btn-warning'} onClick={() => setColored(prev => !prev)}>Изменить</button>
    </>
  )
}
```
Здесь функция `complexCompute` имитирует высокозатратные вычисления некоего числа. Без использования useMemo она бы вызывалась каждый раз при изменении состояния App, даже если аргумент (number) вызываемой функции не изменился.
Для того, чтобы избежать ненужных повторных вычислений производится оптимизация по запоминанию вычисленных данных функции на основании переданного в массиве зависимостей аргументов.

#### Пример использования useMemo с объектами

Основываясь на примере выше можно заметить следующее поведение программы. В консоль браузера 'Styles changed' выводится как при рендере App, так и при изменении свойства состояния App, от которого не зависит объект styles. При этом данный объект каждый раз создается заново.
Дело в том, что в JS объекты хранятся по ссылочной системе. То есть два идентичных объекта (по свойствам и значениям) будут иметь разные ссылки и с точки зрения интерпретатора они не равны. Объекты в JS всегда сравниваются по ссылке а не по значению. Наглядно это можно проиллюстрировать следующим образом:
```javascript
const obj = {
	a: 1,
	b: 2,
};

const obj2 = {
	a: 1,
	b: 2,
};

console.log(obj === obj2) // выведет false
```
Чтобы этого избежать можно обратиться к хуку `useMemo`;

```javascript
// теперь объект styles мемоизирован и его пересоздание зависит только от параметра colored
  const styles = useMemo(() => ({
    color: colored ? 'darkred' : 'black'
  }), [colored])
```

### Хуки для использования в библиотеке react-redux

#### useSelector

Данный хук используется для подключения к глобальному хранилищу (store) для получения необходимых данных компоненту. Данная конструкция заменяет подход, используемый в классовых компонентах, а именно функцию mapStateToProps, которая используется для маппинга каких-либо данных из стейта в пропсы компонента.

Пример использования хука в функциональном компоненте:

```javascript
import { useSelector } from 'react-redux'; // импорт из библиотеки react-redux

export const VKSearchMatchedProfilesComponent = () => {
  const profileMatchesProps = {
    vkMatches: {
      matchedProfiles: useSelector(getVKMatchedProfiles),
      matchesLength: useSelector(getNotNullLengthMatchesSelector),
    },
    searchForm: useSelector(getVKSearchFormData),
    pageRenderDetails: useSelector(getVKRenderData),
  };

    return <VKSearchMatchedProfilesController {...profileMatchesProps} />;
};
```

#### useDispatch

Данный хук используется для инициализации функции dispatch в функциональном компоненте. Это замена подходу c использованием mapDispatchToProps в классовых компонентах, с помощью которого action creator'ы связывались с функцией dispatch.

Пример использования:

```javascript
import { useDispatch, useSelector } from 'react-redux'; // импорт из библиотеки react-redux

export const VKSearchMatchedProfilesComponent = () => {
  const profileMatchesProps = {
    vkMatches: {
      matchedProfiles: useSelector(getVKMatchedProfiles),
      matchesLength: useSelector(getNotNullLengthMatchesSelector),
    },
    searchForm: useSelector(getVKSearchFormData),
    pageRenderDetails: useSelector(getVKRenderData),
  };
    const profileDispatchProps = {
    dispatch: useDispatch(),
  };

    return <VKSearchMatchedProfilesController {...profileMatchesProps} {...profileDispatchProps} />;
};
```

## Методы жизненного цикла компонентов

Методы ЖЦ компонент используются для выполнения определенных действий в момент определенного события.
Основные (наиболее часто используемые) методы ЖЦ компонентов-классов перечислены ниже:

`componentDidMount` первоначальный рендеринг компонента в DOM ("монтирование")
`componentDidUpdate` вызывается сразу после обновления. Не вызывается при первом рендере. Важно не забыть сравнить значение предыдущих пропсов с нынешними (если используется `setState()`), чтобы избежать зацикливания:
```javascript
		componentDidUpdate(prevProps) {
		  // Популярный пример (не забудьте сравнить пропсы):
		  if (this.props.userID !== prevProps.userID) {
		    this.fetchData(this.props.userID);
		  };
		};
```
`componentWillUnmount()` вызывается непосредственно перед размонтированием и удалением компонента. В этом методе выполняется необходимый сброс: отмена таймеров, сетевых запросов и подписок, созданных в `componentDidMount()`.

Аналоги с хуками.

импорт `import React, { useState, useEffect } from 'react';`

аналог componentDidMount:
```javascript
		useEffect(() => {
		  //...code
		}, []);
```
аналог componentDidUpdate/componentDidMount:
```javascript
		useEffect(() => {
		  //...code
		});
```
аналог componentDidUpdate с проверкой предыдущего состояния:
```javascript
		useEffect(() => {
		  document.title = `Вы нажали ${count} раз`;
		}, [count]); // Перезапускать эффект только если count поменялся
```
Следующая запись означает, что useEffect будет выполняться после каждого рендера:
```javascript
		useEffect(() => {
		  //...code
		});
```
Эффект со сбросом
```javascript
		 useEffect(() => {
		    const handleStatusChange = (status) => {
		      setIsOnline(status.isOnline);
		    }
		    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
		    // Указываем, как сбросить этот эффект:
		    return () => {
		      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
		    };
		  });
```

## Обработка событий



## Принцип проектирования и масштабирования в React

Нормальной практикой при построении React приложений является разделение компонентов на:
* компоненты с состоянием (контейнерныйе компоненты)
* компоненты без состояния (презентационные компоненты)
* компоненты высшего порядка (HOC)

### Принцип разделения ответственности

Каждому типу компонентов назначается определенная задача, за которую будет ответственен компонент это принцип разделения ответственности, стандартный подход к проектированию React приложений.

Как правило логика и обработка данных осуществляется в *контейнерных компонентах*, рендеринг же осуществляется в *презентационных*, не имеющих состояния и служащих для отображения данных. HOC-компоненты используются в качестве обертки для других компонент, снабжая оборачиваемый компонент дополнительными возможностями, например, присоединяя контекст или добавляя источники данных.


## Поток данных в React

В React используется **однонаправленный** поток данных. Компонент может передать своё состояние вниз по дереву в виде пропсов дочерних компонентов. Состояние всегда принадлежит определённому компоненту, а любые производные этого состояния могут влиять только на компоненты, находящиеся «ниже» в дереве компонентов.

В React применяется принцип *обратного потока данных*. Если предполагается манипуляция данными в вышестоящем компоненте-контейнере с помощью событий, генерируемыми нижележащим компонентом, вышестоящий компонент может передать дочернему компоненту функцию через пропсы, которая позволяет манипулировать с данными состояния у контейнерного компонента, поскольку компонент отправляет данные обратно в качестве аргументов функции.

Для объяснения этого принципа рассмотрим один из примеров.

Допустим, у нас есть React-приложение, вот с такой структурой:

<App>  <- здесь хранится массив данных
	<AppHeader>
	<SearchPanel>
	<ItemStatusFilter>
	<TodoList>
		<TodoListItem> <- Button, событие click удаляет один из элементов массива по id
		<TodoListItem>

в компоненте App хранятся данные, которые передаются через пропсы дочерним компонентам и в итоге попадают в самый нижележаший компонент TodoListItem. В TodoListItem в свою очередь есть обработчик события click, который вызывается при нажатии кнопки. Задумано так, что при нажатии кнопки обработчик события должен удалить именно тот элемент, на котором кнопка была нажата, причем так, чтобы это было отрендерено. Другими словами нам нужно, чтобы компонент App каким-то образом узнавал, что кнопка на компоненте TodoListItem была вызвана именно для определенного элемента, который должен быть удален.

Реализовать это можно следующим образом:
1. На контейнерном компоненте определяем функцию обработчик.
2. По нисходящей траектории передаем дочернему компоненту функцию-обработчик, которая в итоге будет выполнять необходимую задачу.

В итоге, получив нужную функцию, можно будет через дочерний компонент манипулировать данными на вышестоящем компоненте.


## Рекомендация по работе с формами в React

В React приложениях рекомендован следующий подход, называемый *односторонним связыванием* для работы с формами, обеспечивающий синхронизацию состояния с представлением. Суть подхода описана ниже.

Значение полей в элементах формы связано с внутренним состоянием компонента.

Например:

в поле ввода <input type="text" name="title" value={this.state.title} /> организована привязку к состоянию контейнерного компонента. Для синхронизации добавлен слушатель события `onChange`:

<input type="text" name="title" value={this.state.title}
 onChange={this.handleChange}/>

Таким образом ввод данных в поле ввода будет связан с внутренним состоянием компонента. То есть состояние изменяет представление и все. В обратную сторону это не работает. При таком подходе компонент становится управляемым. В управляемом компоненте значение поля ввода всегда определяется состоянием React. Хотя это означает, что вы должны написать немного больше кода, теперь вы сможете передать значение и другим UI-элементам или сбросить его с других обработчиков событий.


## Маршрутизация в React

Нередко для маршрутизации в React используется библиотека `react-router-dom`. Структура, как правило следующая. В корневом компоненте сборщике (например, `App`):

```javascript
//подключаем библиотеку и подключаем нужне компоненты
import {Switch, Route} from 'react-router-dom';
import React from 'react';
import GetTestDataComp from '../data-detail/test-detail';
import ProfileEnrichmentContainer from '../forms/containers/profile-enrichment-container';
import {TestCodeContainer} from "../forms/test-style-forms/test-code-container";
import MainPage from '../pages/main-page';
import Header from '../header/header';
import VkMainPage from '../pages/vk-main-page';


const App = () => {

    return (
        <div className="main-container">
            <Header />
            //маршутизация начинается с обертки маршрутов в Switch, который пробегает по своим children и осуществляет дальнейший рендеринг

            <Switch>
            //если параметр опционален можно добавить ? после наименования
            		//компонент который должен быть отрендерен по определенному пути можно передать в пропсы Route через component

                <Route path="/vkdata/profile/:id?" component={ProfileEnrichmentContainer}/>
                //если путь точен, т.е. вплоть до указанного в path, не включая дальнейшие пути
                //то нужно указать пропс exact
                <Route path="/vkdata" exact component={VkMainPage}/>
                <Route path="/testdata" component={GetTestDataComp}/>
                <Route path="/vkdata/profile-enrichment/:id" component={ProfileEnrichmentContainer}/>
                //также нужный компонент для рендеринга по маршруту можно передать в children Route
                <Route path="/test-styles">
                    <TestCodeContainer />
                </Route>
                <Route path="/" component={MainPage}/>
            </Switch>
        </div>
    );
};

export default App;

```

 Получить доступ к параметру, передаваемому в строке URL можно несколькими способами:

```javascript

// Способ #1 при роутинге через пропс component
//аналогично можно использовать в компоненте классе с this

export const ProfileEnrichmentContainer = (props) => {

	//получаем доступ к параметру через обращение к объекту params

	return <h2>Данные из URL id {props.match.params.id}</h2>;

}

```

```javascript

// Способ #2 при роутинге как через component так и через children
// используем хук useParams библиотеки react-router-dom
//не работает в компоненте классе

import { useParams } from "react-router-dom";

export const ProfileEnrichmentContainer = (props) => {

	let { id } = useParams();
	//получаем доступ к параметру через обращение к объекту params

	return <h2>Данные из URL id {id}</h2>;

}

```

```javascript

// Способ #3 через пропс render передаем дочернему компоненту параметр в пропс
// используем хук useParams библиотеки react-router-dom
//не работает в компоненте классе


//app.js

                <Route path="/test-styles/:id" render={(props) => {
                    <TestCodeContainer id={props.match.params.id}/>
                }} />


//в переданном компоненте получаем доступ к параметру стандартно в пропсах


```

## Селекторы из библиотеки reselect

Хоть напрямую к React и не относится данный раздел, но напишу также о селекторах, так как нередко они применяются совместно с React-Redux приложениями.

Селекторы это функции, которые запоминают результаты своего вызова и возвращают запомненный (мемоизированный) результат, если входные аргументы не изменились.

Они часто используются для отбора и какой-либо обработке данных из стейта.

Ниже более подробно и наглядно описан принцип работы селекторов.

Селектор создается следующим образом

```javascript
//достаем из библиотеки reselect функцию
import { createSelector } from "reselect";

//эта функция просто отбирает из стейта данные по подходящим профилям
export const getMatchesSelector = (vkData) => vkData.vkMatches.matchedProfiles;


//создаем селектор на основании отобранных ранее профайлов
//вторая функция у селектора принимает в качестве аргумента результат работы
//переданный выше функции
//при первом запуске селектор вычисляет результат и запоминает его
//если результат работы переданной функции в аргументах селектора не изменился
//то не происходит повторного пересчета и возвращается сохраненный ранее результат
//иначе происходит пересчет
export const getNotNullLengthMatchesSelector = createSelector(
    getMatchesSelector,
    (matches) => counterNotNull(matches)
);

//аналог:

export const getNotNullLengthMatchesSelectorExample = createSelector(
    (vkData) => vkData.vkMatches.matchedProfiles,
    (matches) => counterNotNull(matches)
);


//пример файла компонента контроллера, где используются селекторы


import { connect } from 'react-redux';

const mapStateToProps = ({ vkData }) => {
//эти данные будут переданы смаплены из стейта и переданы ему в качестве пропсов
    return {
        vkMatches: {
            ...vkData.vkMatches,
            matchedProfiles: getMatchesSelector(vkData),
            matchesLength: getNotNullLengthMatchesSelector(vkData) //используем мемоизированный селектор
        },
        searchForm: vkData.searchForm,
        pageRenderDetails: vkData.pageRenderDetails
    }
}

//не забываем также подключить компонент к хранилищу
export default withMainProps( connect(mapStateToProps, mapDispatchToProps)(GetMatchesFormController) );

```

В селекторы также можно передавать как другие селекторы, так и функции:

```javascript
//достаем из библиотеки reselect функцию
import { createSelector } from "reselect";

//эта функция просто отбирает из стейта данные по подходящим профилям
export const getMatchesSelector = (state) => state.vkMatches.matchedProfiles;



export const getNotNullLengthMatchesSelector = createSelector(
    getMatchesSelector,
    (matches) => counterNotNull(matches)
);


//второй селектор использует результат работы предыдущего селектора

export const hasNotNullProfiles = createSelector(
	getNotNullLengthMatchesSelector,
	(counter) => counter > 0 ? true : false
	);





```

Также допустимо передавать N функций:

```javascript

const mySelector = createSelector(
  state => state.values.value1,
  state => state.values.value2,
  (value1, value2) => value1 + value2
)

//Допускается также передача функций в массиве:
const totalSelector = createSelector(
  [
    state => state.values.value1,
    state => state.values.value2
  ],
  (value1, value2) => value1 + value2
)
```

### createStructuredSelector

Также можно создавать объект, значения ключей который являются результатами работы N селекторов.

Пример:

```javascript

//основные селекторы

const mySelectorA = state => state.a
const mySelectorB = state => state.b

//также создают объект
const structuredSelector = createSelector(
   mySelectorA,
   mySelectorB,
   mySelectorC,
   (a, b, c) => ({
     a,
     b,
     c
   })
)

//вот пример с использованием createStructuredSelector

const mySelectorA = state => state.a
const mySelectorB = state => state.b

const structuredSelector = createStructuredSelector({
  x: mySelectorA,
  y: mySelectorB
})


```