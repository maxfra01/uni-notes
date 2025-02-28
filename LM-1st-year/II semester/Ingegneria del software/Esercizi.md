# Lab 9

### Tabella es1

| Test Case           | Nodes Covered         | Node Coverage per Test Case | Node Coverage per Test Suite | Edges Covered                                              | Edge Coverage per Test Case    | Edge Coverage per Test Suite | Loop in Line 5  |
|---------------------|-----------------------|-----------------------------|------------------------------|------------------------------------------------------------|--------------------------------|-------------------------------|-----------------|
| `all_equals = [0, 0, 0, 0, 0]` | 1, 3, 4, 5, 7, 8, 9, 10 | 8/10 (80%) | 10/10 (100%) | 1->3, 3->4, 4->5, 5->7, 7->8, 8->9, 9->4, 4->10 | 7/11 (63.64%) | 11/11 (100%) | Yes |
| `all_positive = [1, 2, 3, 4, 5]` | 1, 3, 4, 5, 7, 8, 9, 10 | 8/10 (80%) | 10/10 (100%) | 1->3, 3->4, 4->5, 5->7, 7->8, 8->9, 9->4, 4->10 | 7/11 (63.64%) | 11/11 (100%) | Yes |
| `all_negative = [-1, -2, -3, -4, -5]` | 1, 3, 4, 5, 6, 8, 9, 10 | 8/10 (80%) | 10/10 (100%) | 1->3, 3->4, 4->5, 5->6, 6->8, 8->9, 9->4, 4->10 | 7/11 (63.64%) | 11/11 (100%) | Yes |
| `out_of_size = [1, 2, 3, 4, 5, 6]` | 1, 2 | 2/10 (20%) | 10/10 (100%) | 1->2 | 1/11 (9.09%) | 11/11 (100%) | No |
| `mixed = [-10, 10, 3, 5, -6]` | 1, 3, 4, 5, 6, 7, 8, 9, 10 | 9/10 (90%) | 10/10 (100%) | 1->3, 3->4, 4->5, 5->6, 5->7, 6->8, 7->8, 8->9, 9->4, 4->10 | 8/11 (72.73%) | 11/11 (100%) | Yes |
| `empty = []` | 1, 3, 4, 10 | 4/10 (40%) | 10/10 (100%) | 1->3, 3->4, 4->10 | 3/11 (27.27%) | 11/11 (100%) | No |
### Es4

```js
function acceptableToEat(carb: number, protein: number, fat: number): boolean
{
	if (carb < 0 || protein < 0 || fat < 0) {
		return false;
	}
	const totalCalories = 4 * carb + 4 * protein + 9 * fat;
	const ratio = (carb + protein) / fat;
	return totalCalories < 1000 && ratio > 0.5;
}
```

- Divisione per zero: (>0,>0, 0)
- Errore iniziale (<0, >0, >0)
- Piu di mille calori 200,100,2
- Meno di mille calorie (2,3,4)
- Ratio elevato (2,3, 1)
- Ratio basso (1,1,3)

