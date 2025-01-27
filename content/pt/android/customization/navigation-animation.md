---
title: Animações de navegação
weight: 109
description: >-
  Nesta seção, vamos aprender sobre o que é possível customizar na navegação de
  telas do Beagle.
---

---

## Introdução

No Beagle, a ferramenta de [**navegação entre telas**]({{< ref path="resources/screen-navigation.md" lang="pt" >}}) is called: 
permite que você configure ações como navegar para uma nova tela ou fechar uma tela.

A questão é que, além de realizar estas configurações, você pode também **customizar a navegação** da sua aplicação. 

De modo geral, existem duas opções de customização: 

1. De acordo com as animações padrões do Android 
2. A partir de transição de fragmentos fornecidas pelo próprio Beagle e que, neste caso, podem ser customizadas de acordo com a sua preferência e design system. São elas: 

* [**PushView**]({{< ref path="api/actions/navigate/pushview.md" lang="pt" >}})
* [**PopView**]({{< ref path="api/actions/navigate/popview.md" lang="pt" >}})
* [**PopToView**]({{< ref path="api/actions/navigate/poptoview.md" lang="pt" >}})

## Protocolo e customização

O **protocolo padrão** **do Beagle** para casos de customização é utilizar a ferramenta nativa do Android para fazer transição de fragmentos para customizar sua animação. 

{{% alert color="info" %}}
Para entender melhor como este processo funciona, leia mais na seção [**setCustomAnimations**](https://developer.android.com/reference/android/app/FragmentTransaction#setCustomAnimations%28int,%20int,%20int,%20int%29) do Android. 
{{% /alert %}}

A outra maneira de customizar uma animação de transição é por meio da [**implementação da classe BeagleActivity**]({{< ref path="android/getting-started.md" lang="pt" >}})**,** na qual o Beagle utiliza o método _**getFragmentTransitionAnimation\(\).**_ 

Caso este método não seja implementado e customizado, as transições seguirão a animação padrão Beagle. O código abaixo mostra o **método da BeagleActivity**, que pode ser sobrescrito dessa forma:

```kotlin
open fun getFragmentTransitionAnimation() = FragmentTransitionAnimation(
    enter =  R.anim.slide_from_right,
    exit = R.anim.none_animation,
    popEnter = R.anim.none_animation,
    popExit = R.anim.slide_to_right
)
```

 A animações padrão usada pelo Beagle são: 

* **Transição de entrada** _\(enter_\) à nova tela surge da direta do aplicativo.
* **Animação de saída da pilha** \(_popExit_\), em que a tela também vai para a direita.

Como visto no código acima, para sobrescrever o método de customização você só precisa enviar como resultado uma instância da data class _`FragmentTransitionAnimation`_, como descrito abaixo:

```kotlin
data class FragmentTransitionAnimation(
    @AnimatorRes
    @AnimRes
    val enter: Int,
    @AnimatorRes
    @AnimRes
    val exit: Int,
    @AnimatorRes
    @AnimRes
    val popEnter: Int,
    @AnimatorRes
    @AnimRes
    val popExit: Int
)
```

{{% alert color="info" %}}
Vale contextualizar que esta classe possui **quatro atributos,** sendo eles: 

1. **enter**
2. **exit**
3. **popEnter**
4. **popExit** 

E todas elas descritas na [**documentação nativa do setCustomAnimations**](https://developer.android.com/reference/android/app/FragmentTransaction#setCustomAnimations%28int,%20int,%20int,%20int%29)
{{% /alert %}}

Depois que você sobrescrever o método _`getFragmentTransitionAnimation()`_ na implementação da _BeagleActivity,_ as transições de suas telas e fragmentos Beagle irão agora seguir sua própria animação.

## Exemplo

O exemplo de uso a seguir é para caso de telas que entram e saem com uma animação vertical. Para fazer esta configuração, siga os passos abaixo: 

**Passo 1:** Crie uma pasta com o nome **anim** no arquivo **res** da sua aplicação Android. Este será o local onde as animações serão armazenadas.


```markup
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:duration="@android:integer/config_longAnimTime"
        android:interpolator="@android:anim/decelerate_interpolator"
        android:fromYDelta="100%"
        android:toYDelta="0%"/>
</set>
```


Se seguir o código acima, você indicará ao Beagle de que a animação deverá subir do ponto mínimo vertical para o ponto máximo, completando toda a tela do aplicativo. Esta animação será usada nas **transições de entrada.**

**Passo 2:**  É hora de configurar a animação de saída. Nela, você fará uma configuração para que ela se comporte como o oposto da animação de entrada, descendo toda a tela . Para fazer isso, basta copiar o código abaixo:  


```markup
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:duration="@android:integer/config_longAnimTime"
        android:interpolator="@android:anim/decelerate_interpolator"
        android:fromYDelta="0%"
        android:toYDelta="100%"/>
</set>
```


**Passo 3:** Depois de criar as animações, você pode utilizá-las sobrescritas ao método `getFragmentTransition()` na sua implementação da `BeagleActivity`, que ficará como no exemplo abaixo:

```kotlin
override fun getFragmentTransitionAnimation() = FragmentTransitionAnimation(
    enter =  R.anim.slide_from_bottom,
    exit = R.anim.slide_to_bottom,
    popEnter = R.anim.slide_from_bottom,
    popExit = R.anim.slide_to_bottom
)
```

{{% alert color="success" %}}
 Pronto, o Beagle agora já consegue usar as novas animações de transição!
{{% /alert %}}

Nesse exemplo apenas duas animações foram criadas, sendo uma delas usada para animar as transições de **enter** e **popEnter** e a outra para **exit** e **popExit.** Você pode criar uma para cada ou uma para todos.
