## Description
This template is used to generate set of protocols which have single variable declaring containment of given dependency. Then we can create single depednency container, which conforms to all of those protocols, and inject whole container to our logic units. Those logic units will declare their dependency as single typelias which can be extended whenever another dependency is needed:

```swift
class LogicUnit {
    typealias Dependencies = HasWebService & HasKeychain

    init(dependencies: Dependencies) {
        self.dependencies = dependencies
    }
}
```

## Example

- Targeted type:

```swift
// sourcery: AutoDependency
protocol WebService {
    func getUser(withId id: Int) -> Result<User, WebServiceError>
}

// sourcery: AutoDependency
public protocol Keychain {
    func store(newAuthorization authorization: Authorization)
}
```

- Generated output:

```swift
typealias HasAllDependencies = HasKeychain & HasWebService & NoDependencies

protocol HasKeychain {
    var keychain: Keychain { get }
}

protocol HasWebService {
    var webService: WebService { get }
}
```

- Dependencies container could be defined like this:

```swift
struct DependenciesContainer: HasAllDependencies {
    let webService: WebService = WebServiceImp()
    let keychain: Keychain = KechainImp()
}
```

## Reference

1. **Supported types**: protocols
2. **Output type**: single file
3. **Available annotations**: -
