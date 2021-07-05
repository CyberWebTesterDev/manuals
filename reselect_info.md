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