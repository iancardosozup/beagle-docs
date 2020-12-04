---
title: Loading and Error Treatment
weight: 141
---

---

#### Added in Beagle 1.0.0

To customize the loading and error treatment behaviors on Beagle iOS, you have to create your own navigation controller. 

It can be done by: 

* Extending `BeagleNavigationController`
* Changing the dependencies to use them. 

{{% alert color="info" %}}
The `serverDrivenStateDidChange` method must be overwritten to make this customization.
{{% /alert %}}

See how to create a **navigation controller**:

```swift
import Beagle

class MyAppNavigationController: BeagleNavigationController {
    override func serverDrivenStateDidChange(
        to state: ServerDrivenState,
        at screenController: BeagleController
    ) {
        // TODO: Exibir carregamento e erros quando necessário.
    }
}
```

And, right after, how to configure a dependency:

```bash
let dependencies = BeagleDependencies()
dependencies.navigation.registerNavigationController(
    builder: MyAppNavigationController.init, 
    forId: "MyAppNavigationController")
Beagle.dependencies = dependencies
```

It's possible register more than one custom `BeagleNavigationController`.  To choose which one to use, the BFF needs to inform the `forId`. 

{{% alert color="info" %}}
Make the`navigationControllerType` configuration with other Beagle's settings, so you don't overwrite them. 
{{% /alert %}}

## Configuring the Loading

As standard, Beagle's implementation always return an interface with partially transparent black background. You can see this through `UIActivityIndicatorView`. 

If you want to display a specific loading behavior on your application, check the screen state \(`state`\) and react to it properly. See how on the example below:

```swift
extension UIView {
    func showLoading() {
        // ...
    }
    func hideLoading() {
        // ...
    }
}

class MyAppNavigationController: BeagleNavigationController {
    override func serverDrivenStateDidChange(
        to state: ServerDrivenState,
        at screenController: BeagleController
    ) {
        switch state {
        case .loading(let loading):
            loading ? view.showLoading() : view.hideLoading()
        case .error:
            view.hideLoading()
        }
    }
}
```

If you want to keep the original loading style, just run the standard implementation and move forward to[ **error treatment**](loading-and-error-treatment.md)**.**

```swift
override func serverDrivenStateDidChange(
    to state: ServerDrivenState,
    at screenController: BeagleController
) {
    super.serverDrivenStateDidChange(to: state, at: screenController)
    // TODO: Tratar casos de erro
}
```

## Checking the Error Treatment

When occurs an error on the application, Beagle changes the screen state \(`state`\) to `ServerDrivenState.Error`. In this case, you must check out what kind of error and make the necessary treatment. 

The **possibles errors** are:

* `remoteScreen(Request.Error)`: When the request to load a remote screen fails. Check out on the example below when it happens.
* `lazyLoad(Request.Error)`: When the request to load a [**Lazy component**](../../../api/components/lazy.md) fails. Check out on the example below when it happens.
* `action(Swift.Error)`: When an [**`Action`**](../../../api/actions/) 's execution fails.

**Errors on a request** \(`Request.Error`\):

* `urlBuilderError`: When the resource URL or a [**`baseURL`**](beagledependencies.md#baseurl) is invalid.
* `networkError`: When it's not possible to establish a connection or an error was returned from the [**BFF**](../../../principais-conceitos.md#backend-for-frontend).
* `decoding`: When the answer sent from BFF is different than the object expected.

{{% alert color="info" %}}
The error has a `retry` block that allows you to run again the action that has failed.
{{% /alert %}}

See the example below how to display an error screen when the screen loading fails:

```swift
class ErrorView: UIVIew {
    var retry: (() -> Void)?
    func present(in view: UIView) {
        // ...
    }
}

class MyAppNavigationController: BeagleNavigationController {
    override func serverDrivenStateDidChange(
        to state: ServerDrivenState,
        at screenController: BeagleController
    ) {
        super.serverDrivenStateDidChange(to: state, at: screenController)
        if case let .error(serverDrivenError, retry) = state,
           case .remoteScreen = serverDrivenError {
               let errorView = ErrorView()
               errorView.retry = retry
               errorView.present(in: view)
        }
    }
}
```