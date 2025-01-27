
---
title: Ação Simples
weight: 104
description: Criando e executando uma ação customizada
---

**Tópicos abordados:**

- O que é uma ação customizada
- Como criar uma ação customizada
- Como Registrar uma ação customizada
- Como usar uma ação customizada

**Requisitos:**

- Ter um projeto com o Beagle Configurado (FRONT e BACKEND)

## **O que é uma ação customizada?**

Uma ação customizada é uma ação com um comportamento especifico criada para seu caso de uso. O Beagle possui uma série de ações padrão, no entanto, pode haver algum uso que necessite de uma funcionalidade que não é padrão, como por exemplo, abrir uma interface de câmera em um celular.

Para mais informações sobre ações padronizadas do Beagle, veja a seção [**Tipos de Ações.**]({{< ref path="/api/actions/overview#tipos-de-ações" lang="pt">}}).

## **Como criar uma ação customizada?**

Para criar uma ação personalizada é necessário:

**1.** Criar uma classe anotada com **`@RegisterAction`**, e implementar a interface `Action`, no **Frontend** e no **Backend** da sua aplicação;
**2.** Colocar o nome da ação por parâmetro da annotation para evitar possíveis problemas com o Proguard. Certifique que a ação tem o mesmo nome no **Frontend** e no **Backend**;
**3.** Implemente o método **`execute`** (somente no **Frontend**).

O atributo  **`value`**  é um exemplo de parâmetro que pode ser declarado no construtor dessa classe, você pode usar quantos precisar. 
O exemplo a seguir mostra uma ação customizada para executar um **Toast** recebendo um texto no parâmetro **`value`**:

### **Classe que representa uma ação no frontend (Android)**

```kotlin
@RegisterAction("customActionAndroid")
data class CustomActionAndroid(
    val value: String
) : Action {
    override fun execute(rootView: RootView) {
        Toast.makeText(
            rootView.getContext(), 
            value, 
            Toast.LENGTH_SHORT
        ).show()
    }
}
```

### **Classe que representa a ação no Backend** ```

```kotlin
@RegisterAction("customActionAndroid")
data class CustomActionAndroid(
    val value: String
) : Action
```

{{% alert color="warning" %}}
   A classe precisa registrar o parâmetro que deseja declarar no **Backend** e utilizar no **Frontend**. Nessa ação declare apenas um **`value`** para guardar a mensagem que o **Toast** irá utilizar.
{{% /alert %}}

Abaixo temos o JSON que representa essa ação sendo chamada a partir do click de um botão:

```json
{
  "_beagleComponent_": "beagle:button",
  "text": "Beagle Button",
  "onPress": [
    {
      "_beagleAction_": "custom:myCustomAction",
      "value": "ParameterForAction."
    }
  ]
}
```

## **Como registrar sua Ação**

- Para isso, anote a classe da sua ação com o `@RegisterAction("className")` onde `className` é o nome da sua classe

## **Como usar uma Ação customizada**

Para utilizar uma ação customizada, declare-a em um componente que execute ações, como por exemplo, um `Botão`.
No exemplo abaixo, a ação é declarada no atributo **`onPress`** do botão:

```kotlin
Button(
   text = text,
   styleId = styleId,
   onPress = listOf(
      CustomActionAndroid("Sou uma ação customizada")
   )
)
```
