## React 18 vs 17 Основні відмінності

1. Automatic batching - автоматичний батчинг оновлення стану для асинхронних операцій.

Батчингом в React називають процес группування декількох кикликів оновлення стану в один етап рендерінгу.

Наприклад такий код:

Приклад_1:

```javascript
const onClick = async (formData) => {
  setLoading(true);

  await createDeal(formData);

  setLoading(false);
};
```

Кількість рендерів: 2

Приклад_2:

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
