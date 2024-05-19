## React 18 vs 17 Основні відмінності

1. Automatic batching - автоматичний батчинг оновлення стану для асинхронних операцій.

Батчингом в React називають процес группування декількох кикликів оновлення стану в один етап рендерінгу.

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
