# Usage

## How to
This template is used to generate set of protocols which have single variable declaring containment of given dependency. Then we can create single depednency container, which conforms to all of those protocols, and inject whole container to our logic units. Those logic units will declare their dependency as single typelias:

```swift
class LogicUnit {
	typealias Dependencies = HasWebService
	
	init(dependencies: Dependencies) {
		self.dependencies = dependencies
	}
}
```
So whenever we need another dependency in this unit, we just extend this typealias by another `Has...`.

Example of targeted type:

```swift
// sourcery: AutoDependency
protocol WebService {
    func getUser(withId id: Int) -> Result<User>
}

// sourcery: AutoDependency
public protocol Keychain {
    func store(newAuthorization authorization: Authorization)
}
```

Running sourcery will generate this code:

```swift
typealias HasAllDependencies = HasKeychain & HasWebService & NoDependencies

protocol HasKeychain {
	var keychain: Keychain { get }
}

protocol HasWebService {
	var webService: WebService { get } 
}
```

Example of dependencies container:

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

## Source 
_