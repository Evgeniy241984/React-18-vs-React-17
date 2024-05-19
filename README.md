## React 18 vs 17 Основні відмінності

1. Automatic batching - автоматичний батчинг оновлення стану для асинхронних операцій.

Батчингом в React називають процес группування декількох кикликів оновлення стану в один етап рендерінгу.

Приклад_1:

```javascript
const [country, setCountry] = useState("");
const [language, setLanguage] = useState("");

const onClick = (newCountry, newLanguage) => {
  setCountry(newCountry);
  SetLanguage(newLanguage);
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
  setLoading(true);
  setSubmitButtonDisabled(true);

  const deal = await createDeal(formData);

  setDeal(deal);
  setSubmitButtonDisabled(false);
  setLoading(false);
};
```

Кількість рендерів:

    V.17 - 5 рендерів
    V.18 - 2 рендера
