# YAML Format

File format has key:value pair separated by colon and space `:_`

lets see same data set with xml, json and yaml:

- xml

```xml
<Servers>
    <Server>
        <name>Server1</name>
        <owner>Matthew</owner>
        <created>23122020</created>
        <status>active</status>
    </Server>
</Servers>
```

- json

```json
{
  "Servers": [
    {
      "name": "Server1",
      "owner": "Matthew",
      "created": 23122020,
      "status": "active"
    }
  ]
}
```

- yaml

```yaml
Servers:
  - name: Server1
    owner: Matthew
    created: 23122020
    status: active
```

Key Value Pair

```yaml
Fruit: Apple
Vegetable: Carrot
Liquid: Water
Meat: Fish
```

Array / List

```yaml
Fruits:
  - Apple
  - Orange
  - Grape
```

Dictionary / Map

```yaml
Apple:
  Calories: 80
  Fat: 0.2 g
  Carbs: 20 g

Grapes:
  Calories: 110
  Fat: 0.3 g
  Carbs: 18 g
```

## Notes

- Dictionary / maps are unordered, order is not important in equality.

```yaml
Banana1:
  Calories: 105
  Fat: 0.4 g
  Carbs: 28 g

Banana2:
  Calories: 105
  Carbs: 28 g
  Fat: 0.4 g

Banana1 == Banana2
```

- Lists are ordered, order is important in equality

```yaml
Fruits1:
- Orange
- Apple
- Banana

Fruits2:
- Orange
- Banana
- Apple

Fruits1 != Fruits2
```

`#` is used for commenting

> :exclamation: tabs are not allowed

## Yaml Exercises

- try adding the address information. Note the address is a dictionary

  | Key/Property | Value |
  | :----------- | :---- |
  | name         | john  |
  | gender       | male  |
  | age          | 24    |

​ address:

| Key/Property | Value         |
| -----------: | :------------ |
|         city | edison        |
|        state | new jersey    |
|      country | united states |

```yaml
employee:
  name: john
  gender: male
  age: 24
  address:
    city: edison
    state: new jersey
    country: united states
```

- We would like to add additional details for each item, such as color, weight etc. We have updated the first one for you. Similarly modify the remaining items to match the below data.

  | Fruit | Color  | Weight |
  | :---- | :----- | :----- |
  | apple | red    | 100g   |
  | apple | red    | 90g    |
  | mango | yellow | 150g   |

```yaml
- name: apple
  color: red
  weight: 100g
- name: apple
  color: red
  weight: 90g
- name: mango
  color: yellow
  weight: 150g
```

- We would like to record information about multiple employees. Convert the dictionary `employee` to an array `employees`.

```yaml
employee:
  name: john
  gender: male
  age: 24
# converted to list below
employees:
  - name: john
    gender: male
    age: 24
```

- Add an additional employee to the list using the below information.

  | Key/Property | Value  |
  | :----------- | :----- |
  | name         | sarah  |
  | gender       | female |
  | age          | 28     |

```yaml
employees:
    -
        name: john
        gender: male
        age: 24
	-
		name: sarah
		gender: female
		age: 28
```

- Now try adding the pay information. Remember while address is a dictionary, payslips is an array of month and amount
  | Key/Property | Value |
  | :----------- | :---- |
  | name | john |
  | gender | male |
  | age | 24 |

​ address:

| Key/Property | Value         |
| -----------: | :------------ |
|         city | edison        |
|        state | new jersey    |
|      country | united states |

payslips

# month amount

1 june 1400
2 july 2400
3 august 3400

```yaml
employee:
  name: john
  gender: male
  age: 24
  address:
    city: edison
    state: new jersey
    country: united states
    payslips:
      - month: june
        amount: 1400
      - month: july
        amount: 2400
      - month: august
        amount: 3400
```

- Try to fix the errors below

```yaml
---
car:
  color: blue
  price: '$20,000'
  wheels:
    - model: KDJ39848T
      location: front-right
    - model: MDJ39485DK
      location: front-left
    - model: KCMDD3435K
      location: rear-right
    - model: JJDH34234KK
      location: rear-left
bus:
  color: white
  price: '$120,000'
  wheels: # This bus has 4 wheels
    - model: BBBB39848T
      location: front-right
    - model: BBBB9485DK
      location: front-left
    - model: BBBB3435K
      location: rear-right
    - model: BBBB4234KK
      location: rear-left
```
