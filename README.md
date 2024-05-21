## React 18 vs 17 Основні відмінності

### 1. Automatic batching - автоматичний батчинг оновлення стану для асинхронних операцій.

Батчингом в React називають процес группування декількох кикликів оновлення стану в один етап рендерінгу.

#### Приклад_1:

```javascript
const [country, setCountry] = useState("");
const [language, setLanguage] = useState("");

const onClick = (newCountry, newLanguage) => {
  setCountry(newCountry);
  setLanguage(newLanguage);
};
```

Кількість рендерів: 1

#### Приклад_2:

```javascript
const onClick = async (formData) => {
  setLoading(true);

  await createDeal(formData);

  setLoading(false);
};
```

Кількість рендерів: 2

Причина зміни кількості рендерів в асинхронних методах: \
setTimeout \
setInterval \
Promise \
XMLHttpreaquest \

#### Приклад_3:

```javascript
const onClick = async (formData) => {
  //в 17 та 18 версії ці два виклики групуються  в 1 рендер
  setLoading(true);
  setSubmitButtonDisabled(true);

  const deal = await createDeal(formData);

  //в 17 версії ці 3 виклики роблять 3 рендер
  //в 18 версії ці 3 виклики групуються  в 1 рендер
  setDeal(deal);
  setSubmitButtonDisabled(false);
  setLoading(false);
};
```

Кількість рендерів:

    V.17 - 4 рендерів
    V.18 - 2 рендера

---

В версіях до 18 можна вирішити за допомогою unstable_batchedUpdates()

```javascript
import { unstable_batchedUpdates } from "react-dom";

const onClick = async (formData) => {
  setLoading(true);
  setSubmitButtonDisabled(true);

  const deal = await createDeal(formData);

  unstable_batchedUpdates(() => {
    setDeal(deal);
    setSubmitButtonDisabled(false);
    setLoading(false);
  });
};
```

### 2. Transitions - нова концепція в React для розрізнення термінових і нетермінових оновлень.

2.1. Термінові оновлення (Urgent updates): відображають пряму взаємодію, як-от введення тексту, клік мишки, натискання кнопки. \
2.2. Переходи (Transotion updates): переміщення інтерфейсу користувача з одного перегляду в інший.

Наприклад, коли ви вибираєте фільтр у дропдаун меню, ви очікуєте, що сама кнопка фільтра негайно відреагує на натискання. Однак список з результатами може зявитись пізніше. Невелика затримка буде непомітною і часто очікуваною. І якщо ви знову зміните фільтр до завершення рендерингу результатів, ви бажаєте побачити лише останні результати.

Ця проблема може бути вирішена за допомогою хука useTransition()

Приклад реалізації тут https://codesandbox.io/p/sandbox/react-18-usetransition-hook-nxgq2d?file=%2Fsrc%2FTabButton.tsx%3A15%2C43-15%2C58

### 3. Suspence - дозволяє декларативно вказати стан завантаження для частини дерева компонентів, якщо воно ще не готове до відображення.

Наприклад

```javascript
<Suspense fallback={<Spinner />}>
  <Comments />
</Suspense>
```

Паттерни DataFetching в реакті

1. Fetch on Render - https://codesandbox.io/p/sandbox/fetch-on-render-x6y5pp?file=%2Fsrc%2FCountries.js%3A13%2C26 \
   ![Fetch on Render](/images/Fetch_On_Render.png)

- завантаження стану в useEffect
- підтримувати стан завантаження вручну (додержуватись в компоненті порядку зміни стану)

2. Fetch then Render - https://codesandbox.io/p/sandbox/fetch-then-render-ngjfzf?file=%2Fsrc%2FCountries.js%3A11%2C20 \
   ![Fetch then Render](/images/Fetch_Then_Render.png)

- підтримувати стан завантаження вручну (додержуватись в компоненті порядку зміни стану)

3. Render while Fetch - https://codesandbox.io/p/sandbox/render-while-fetch-rw8kv2?file=%2Fsrc%2FCountries.js%3A8%2C11 \
   ![Render while Fetch](/images/Render_While_Fetch.png)

### 4. Нові Client and Server Rendering APIs

#### 4.1. Client

    createRoot - використовується у 18 версії замість ReactDOM.render.

    в 17 версії

```javascript
const root = document.getElementById("root");

ReactDOM.render(
  <StrictMode>
    <App />
  </StrictMode>,
  root
);
```

    в 18 версії

```javascript
const rootElement = document.getElementById("root");
const root = createRoot(rootElement);

root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```
