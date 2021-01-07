---
title: Listview
weight: 315
description: 'Description of the ListView component, its attributes and constructors'
---

---

## What is it? 

The ListView component is responsible for defining a list of recyclable items natively. These items can be any server driven components. The use of ListView is recommended for situations where there is repetition of components, but with different data.

{{% alert color="danger" %}}
From version 1.5.0, we started to support the use of context and cell recycling in ListView, with that we provide two ways to build the component. The depreciated version was maintained only to keep backward compatibility, upgrade to the new version of the component if possible for better performance.
{{% /alert %}}

See how the structure is represented:

### ListView

| Atributo | Tipo | Obrigatório | Definição |
| :--- | :--- | :---: | :--- |
| direction | [ListDirection](#listdirection) |   | Sets the direction in which list items are displayed. |
| context | [ContextData](/api/context) |  | Defines the context of the component. |
| onInit | List&lt;[Action](/api/actions)&gt; |  | List of actions to be performed as soon as the component is displayed.  |
| dataSource | [Bind](/api/context#bindings)&lt;List&lt;Any&gt;&gt; | ✓ | Expression that points to a list of values used to populate the component. |
| template | [ServerDrivenComponent](/api/components) | ✓ | It represents each cell in the list through a `ServerDrivenComponent`. |
| onScrollEnd | List&lt;[Action](/api/actions)&gt; |  | List of actions taken when the list ends. |
| scrollEndThreshold | Int |  | Defines the percentage scrolled from the list to trigger `onScrollEnd`. |
| iteratorName | String |  | It is the context identifier for each cell. |
| key | String |  | Points to a unique value present in each item of the `dataSource` to be used as a suffix in the ids of the template components. |

### ListDirection

It is an `ENUM`, the values are:

| **Values** | **Definition** |
| :--- | :--- |
| VERTICAL | When itens are displayed in **`LINES`**. |
| HORIZONTAL | When itens are displayed in **`COLUMNS`**. |

{{% alert color="info" %}}
Default value is ListDirection.VERTICAL
{{% /alert %}}

### Deprecated ListView

<table>
  <thead>
    <tr>
      <th style="text-align:left">Attribute</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Required</th>
      <th style="text-align:left">Descriptioon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">children</td>
      <td style="text-align:left">List&lt;<a href="../"><b>ServerDrivenComponent</b></a>&gt;</td>
      <td style="text-align:left">&#x2713;</td>
      <td style="text-align:left">
        <p></p>
        <p>Defines the item list view. They can be configured like a <code>ServerDrivenComponents </code>or
          like <code>views.</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">direction</td>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="https://docs.usebeagle.io/api/components/layout/listview"><b>ListDirection</b></a>&lt;b&gt;&lt;/b&gt;</td>
      <td
      style="text-align:left"></td>
        <td style="text-align:left">Defines the preview list direction.</td>
    </tr>
  </tbody>
</table>

## How to use it? 

### ListView

{{< tabs id="T128" >}}
{{% tab name="JSON" %}}
<!-- json-playground:listView.json
{
  "_beagleComponent_": "beagle:listView",
  "direction": "VERTICAL",
  "dataSource": [
    {
      "name": "Kelsier",
      "race": "Half-skaa",
      "planet": "Scadrial",
      "isMistborn": true,
      "age": 38,
      "sex": "male"
    },
    {
      "name": "Vin",
      "race": "Half-skaa",
      "planet": "Scadrial",
      "isMistborn": true,
      "age": 20,
      "sex": "female"
    },
    {
      "name": "TenSoon",
      "race": "Kandra",
      "planet": "Scadrial",
      "isMistborn": false,
      "age": 40,
      "sex": "male"
    }
  ],
  "template": {
    "_beagleComponent_": "beagle:container",
    "style": {
      "margin": {
        "bottom": {
          "value": 20,
          "type": "REAL"
        }
      }
    },
    "children": [
      {
        "_beagleComponent_": "beagle:text",
        "text": "Name: @{item.name}"
      },
      {
        "_beagleComponent_": "beagle:text",
        "text": "Race: @{item.race}"
      },
      {
        "_beagleComponent_": "beagle:text",
        "text": "Mistborn: @{item.isMistborn}"
      },
      {
        "_beagleComponent_": "beagle:text",
        "text": "Planet: @{item.planet}"
      },
      {
        "_beagleComponent_": "beagle:text",
        "text": "sex: @{item.sex}"
      },
      {
        "_beagleComponent_": "beagle:text",
        "text": "age: @{item.age}"
      }
    ]
  }
}
-->
{{% playground file="listView.json" language="en" %}}
{{% /tab %}}

{{% tab name="Kotlin DSL" %}}
```kotlin
ListView(
    dataSource = listOf(
        Person(
            name = "Kelsier",
            race = "Half-skaa",
            planet = "Scadrial",
            isMistborn = true,
            age = 38,
            sex = Sex.MALE
        ),
        Person(
            name = "Vin",
            race = "Half-skaa",
            planet = "Scadrial",
            isMistborn = true,
            age = 20,
            sex = Sex.FEMALE
        ),
        Person(
            name = "TenSoon",
            race = "Kandra",
            planet = "Scadrial",
            isMistborn = false,
            age = 40,
            sex = Sex.MALE
        ),
    ),
    template = Container(
        children = listOf(
            Text("Name: @{item.name}"),
            Text("Race: @{item.race}"),
            Text("Mistborn: @{item.isMistborn}"),
            Text("Planet: @{item.planet}"),
            Text("sex: @{item.sex}"),
            Text("age: @{item.age}"),
        )
    ).applyStyle(
        Style(
            margin = EdgeValue(bottom = 20.unitReal())
        )
    )
)
```
{{% /tab %}}
{{< /tabs >}}

### Deprecated ListView

{{< tabs id="T129" >}}
{{% tab name="JSON" %}}
<!-- json-playground:listViewDepreciado.json
{
  "_beagleComponent_": "beagle:listView",
  "children": [
    {
      "_beagleComponent_": "beagle:text",
      "text": "Beagle Text list",
      "textColor": "#FF0000",
      "alignment": "CENTER"
    },
    {
      "_beagleComponent_": "beagle:text",
      "text": "Beagle Text list",
      "textColor": "#00FF00",
      "alignment": "CENTER"
    },
    {
      "_beagleComponent_": "beagle:text",
      "text": "Beagle Text list",
      "textColor": "#0000FF",
      "alignment": "CENTER"
    }
  ],
  "direction": "HORIZONTAL"
}
-->
{{% playground file="listViewDepreciado.json" language="en" %}}
{{% /tab %}}

{{% tab name="Kotlin DSL" %}}
```kotlin
ListView(
    direction = ListDirection.HORIZONTAL,
    children = listOf(
        Text(
            text = "Beagle Text list",
            textColor = "#FF0000",
            alignment = TextAlignment.CENTER
        ),
        Text(
            text = "Beagle Text list",
            textColor = "#00FF00",
            alignment = TextAlignment.CENTER
        ),
        Text(
            text = "Beagle Text list",
            textColor = "#0000FF",
            alignment = TextAlignment.CENTER
        )
    )
)
```
{{% /tab %}}
{{< /tabs >}}