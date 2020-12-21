## JSON

JSON是 JavaScript Object Notation 的简称，是一种轻量的数据表示方法。JSON格式采用key:value的方式记录数据，非常直观，比XML简洁，因而大受欢迎

介绍JSON格式前，先让我们看看XML格式。显然，XML 得到了相当多的关注（正面和负面的评价都有），已经在 Ajax 应用程序中广泛使用：

```xml
<request>
  <firstName>Brett</firstName>
  <lastName>McLaughlin</lastName>
  <email>brett@newInstance.com</email>
</request>
```

这只是另一种数据格式，使我们能够使用 XML 而不是纯文本和名称/值对。

本文讨论另一种数据格式，JSON。JSON 看起来既熟悉又陌生。它提供了另一种选择，选择范围更大总是好事情。

### 添加 JSON

在使用名称/值对或 XML 时，实际上是使用 JavaScript 从应用程序中取得数据并将数据转换成另一种数据格式。在这些情况下，JavaScript 在很大程度上作为一种数据操纵语言，用来移动和操纵来自 Web 表单的数据，并将数据转换为一种适合发送给服务器端程序的格式。

但是，有时候 JavaScript 不仅仅作为格式化语言使用。在这些情况下，实际上使用 JavaScript 语言中的对象来表示数据，而不仅是将来自 Web 表单的数据放进请求中。在这些情况下，从 JavaScript 对象中提取数据，然后再将数据放进名称/值对或 XML，就有点儿多此一举 了。这时就合适使用 JSON：JSON 允许轻松地将 JavaScript对象转换成可以随请求发送的数据（同步或异步都可以）。

JSON 并不是某种魔弹；但是，它对于某些非常特殊的情况是很好的选择。

### JSON 基础

简单地说，JSON 可以将 JavaScript 对象中表示的一组数据转换为字符串，然后就可以在函数之间轻松地传递这个字符串，或者在异步应用程序中将字符串从 Web 客户机传递给服务器端程序。这个字符串看起来有点儿古怪（稍后会看到几个示例），但是 JavaScript 很容易解释它，而且 JSON 可以表示比名称/值对更复杂的结构。例如，可以表示数组和复杂的对象，而不仅仅是键和值的简单列表。

### 简单 JSON 示例

按照最简单的形式，可以用下面这样的 JSON 表示名称/值对：

```json
{ "firstName": "Brett" } 
```

 这个示例非常基本，而且实际上比等效的纯文本名称/值对占用更多的空间：

```
firstName=Brett
```

但是，当将多个名称/值对串在一起时，JSON 就会体现出它的价值了。首先，可以创建包含多个名称/值对的记录，比如：

```JSON
{ "firstName": "Brett", "lastName":"McLaughlin", "email": "brett@newInstance.com" }
```

从语法方面来看，这与名称/值对相比并没有很大的优势，但是在这种情况下 JSON 更容易使用，而且可读性更好。例如，它明确地表示以上三个值都是同一记录的一部分；花括号使这些值有了某种联系。

### 值的数组

当需要表示一组值时，JSON 不但能够提高可读性，而且可以减少复杂性。例如，假设您希望表示一个人名列表。在 XML 中，需要许多开始标记和结束标记；如果使用典型的名称/值对（就像在本系列前面文章中看到的那种名称/值对），那么必须建立一种专有的数据格式，或者将键名称修改为 person1-firstName 这样的形式。

如果使用 JSON，就只需将多个带花括号的记录分组在一起：

```JSON
{ "people": [
  { "firstName": "Brett", "lastName":"McLaughlin", "email": "brett@newInstance.com" },
  { "firstName": "Jason", "lastName":"Hunter", "email": "jason@servlets.com" },
  { "firstName": "Elliotte", "lastName":"Harold", "email": "elharo@macfaq.com" }
]}
```

这不难理解。在这个示例中，只有一个名为 people 的变量，值是包含三个条目的数组，每个条目是一个人的记录，其中包含名、姓和电子邮件地址。上面的示例演示如何用括号将记录组合成一个值。当然，可以使用相同的语法表示多个值（每个值包含多个记录）：

```JSON
{ "programmers": [
  { "firstName": "Brett", "lastName":"McLaughlin", "email": "brett@newInstance.com" },
  { "firstName": "Jason", "lastName":"Hunter", "email": "jason@servlets.com" },
  { "firstName": "Elliotte", "lastName":"Harold", "email": "elharo@macfaq.com" }
 ],
"authors": [
  { "firstName": "Isaac", "lastName": "Asimov", "genre": "science fiction" },
  { "firstName": "Tad", "lastName": "Williams", "genre": "fantasy" },
  { "firstName": "Frank", "lastName": "Peretti", "genre": "christian fiction" }
 ],
"musicians": [
  { "firstName": "Eric", "lastName": "Clapton", "instrument": "guitar" },
  { "firstName": "Sergei", "lastName": "Rachmaninoff", "instrument": "piano" }
 ]
}
```

这里最值得注意的是，能够表示多个值，每个值进而包含多个值。但是还应该注意，在不同的主条目（programmers、authors 和 musicians）之间，记录中实际的名称/值对可以不一样。JSON 是完全动态的，允许在 JSON 结构的中间改变表示数据的方式。

在处理 JSON 格式的数据时，没有需要遵守的预定义的约束。所以，在同样的数据结构中，可以改变表示数据的方式，甚至可以以不同方式表示同一事物。

### 在 JavaScript 中使用 JSON

掌握了 JSON 格式之后，在 JavaScript 中使用它就很简单了。JSON 是 JavaScript 原生格式，这意味着在JavaScript 中处理 JSON 数据不需要任何特殊的 API 或工具包。

### 将 JSON 数据赋值给变量

例如，可以创建一个新的 JavaScript 变量，然后将 JSON 格式的数据字符串直接赋值给它：

```javascript
var people =
  { "programmers": [
    { "firstName": "Brett", "lastName":"McLaughlin", "email": "brett@newInstance.com" },
    { "firstName": "Jason", "lastName":"Hunter", "email": "jason@servlets.com" },
    { "firstName": "Elliotte", "lastName":"Harold", "email": "elharo@macfaq.com" }
   ],
  "authors": [
    { "firstName": "Isaac", "lastName": "Asimov", "genre": "science fiction" },
    { "firstName": "Tad", "lastName": "Williams", "genre": "fantasy" },
    { "firstName": "Frank", "lastName": "Peretti", "genre": "christian fiction" }
   ],
  "musicians": [
    { "firstName": "Eric", "lastName": "Clapton", "instrument": "guitar" },
    { "firstName": "Sergei", "lastName": "Rachmaninoff", "instrument": "piano" }
   ]
  }
```

这非常简单；现在 people 包含前面看到的 JSON 格式的数据。但是，这还不够，因为访问数据的方式似乎还不明显。

### 访问数据

尽管看起来不明显，但是上面的长字符串实际上只是一个数组；将这个数组放进 JavaScript 变量之后，就可以很轻松地访问它。实际上，只需用点号表示法来表示数组元素。所以，要想访问 programmers 列表的第一个条目的姓氏，只需在 JavaScript 中使用下面这样的代码：

```javascript
people.programmers[0].lastName;  
```

注意，数组索引是从零开始的。所以，这行代码首先访问 people 变量中的数据；然后移动到称为 programmers的条目，再移动到第一个记录（[0]）；最后，访问 lastName 键的值。结果是字符串值 “McLaughlin”。

下面是使用同一变量的几个示例。

```javascript
people.authors[1].genre             // Value is "fantasy"
people.musicians[3].lastName // Undefined. This refers to the fourth entry, and there isn't one
people.programmers.[2].firstName      // Value is "Elliotte"
```

利用这样的语法，可以处理任何 JSON 格式的数据，而不需要使用任何额外的 JavaScript 工具包或 API。

### 修改 JSON 数据

正如可以用点号和括号访问数据，也可以按照同样的方式轻松地修改数据：

```javascript
people.musicians[1].lastName = "Rachmaninov";
```

在将字符串转换为 JavaScript 对象之后，就可以像这样修改变量中的数据。

### 转换回字符串

当然，如果不能轻松地将对象转换回本文提到的文本格式，那么所有数据修改都没有太大的价值。在 JavaScript中这种转换也很简单：

```javascript
String newJSONtext = people.toJSONString();
```

这样就行了！现在就获得了一个可以在任何地方使用的文本字符串，例如，可以将它用作 Ajax 应用程序中的请求字符串。

更重要的是，可以将任何 JavaScript 对象转换为 JSON 文本。并非只能处理原来用 JSON 字符串赋值的变量。为了对名为 myObject 的对象进行转换，只需执行相同形式的命令：

```javascript
String myObjectInJSON = myObject.toJSONString();
```

这就是 JSON 与本系列讨论的其他数据格式之间最大的差异。如果使用 JSON，只需调用一个简单的函数，就可以获得经过格式化的数据，可以直接使用了。对于其他数据格式，需要在原始数据和格式化数据之间进行转换。即使使用 Document Object Model 这样的 API（提供了将自己的数据结构转换为文本的函数），也需要学习这个API 并使用 API 的对象，而不是使用原生的 JavaScript 对象和语法。

最终结论是，如果要处理大量 JavaScript 对象，那么 JSON 几乎肯定是一个好选择，这样就可以轻松地将数据转换为可以在请求中发送给服务器端程序的格式。