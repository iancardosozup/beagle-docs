---
title: Pushview
weight: 242
description: Here you'll find PushView description and its attribute.
---

---

### What it is?

Presents a new screen and puts it on the pile.

Your structure is represented by the attribute below: 

| **Attribute** | **Type** | Required | **Definition** |
| :--- | :--- | :--- | :--- |
| route | [**Route**](route/) |          ✓ | Navigation route. |

##  How to use it?

On the example below, there is a screen coming from BFF with a button when clicked, open a new  server-drive fragment with a specific BFF screen. 

To test is, you need a endpoint to return with the code below from your BFF and call it in the frontend. You can pass a local route \(that it will pass a screen in the route\) or remote route that will pass the endpoint of the screen which it will navigate. 

{{< tabs name="T110" >}}
{{% tab name="JSON" %}}
```javascript
{
  "_beagleComponent_" : "beagle:screenComponent",
  "child" : {
    "_beagleComponent_" : "beagle:button",
    "text" : "Click me!",
    "onPress" : [ {
      "_beagleAction_" : "beagle:pushView",
      "route" : {
        "screen" : {
          "_beagleComponent_" : "beagle:screenComponent",
          "child" : {
            "_beagleComponent_" : "beagle:text",
            "text" : "Hello second Screen"
          }
        }
      }
    } ]
  }
}
```
{{% /tab %}}

{{% tab name="Kotlin DSL" %}}
```kotlin
Screen(
    child = Button(
        text = "Click me!",
        onPress = listOf(
            Navigate.PushView(
                Route.Local(
                    Screen(
                        child = Text("Hello second Screen")
                    )
                )
            )
        )
    )
)
```
{{% /tab %}}
{{< /tabs >}}

### 👉 [Test this example on Web Playground](https://beagle-playground.netlify.app/#/demo/default-components/button.json)