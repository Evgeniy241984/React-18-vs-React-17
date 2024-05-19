## React 18 vs 17 Основні відмінності

1. Automatic batching - автоматичний батчинг оновлення стану для асинхронних операцій.

Батчингом в React називають процес группування декількох кикликів оновлення стану в один етап рендерінгу.

Приклад_1:

```javascript
const [country, setCountry] = useState("");
const [language, setLanguage] = useState("");

const onClick = (newCountry, newLanguage) => {
  setCountry(newCountry);
  setLanguage(newLanguage);
};
```

Кількість рендерів: 1

Приклад_2:

```javascript
const onClick = async (formData) => {
  setLoading(true);

  await createDeal(formData);

  setLoading(false);
};
```

Кількість рендерів: 2

Причина зміни кількості рендерів в асинхронних методах:
setTimeout
setInterval
Promise
XMLHttpreaquest

Приклад_3:

```javascript
const onClick = async (formData) => {
  //в 17 та 18 версії ці два виклики груперуються в 1 рендер
  setLoading(true);
  setSubmitButtonDisabled(true);

  const deal = await createDeal(formData);

  //в 17 версії ці 3 виклики роблять 3 рендер
  //в 18 версії ці 3 виклики груперуються  в 1 рендер
  setDeal(deal);
  setSubmitButtonDisabled(false);
  setLoading(false);
};
```

Кількість рендерів:

    V.17 - 4 рендерів
    V.18 - 2 рендера

---

В версіях до 18 (з якої????) можна вирішити за допомогою unstable_batchedUpdates()

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
