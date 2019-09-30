## Description
This template is used to generate protocol with the interface for given structure or class - it will contain only those methods and properties getters/setters which have the same access level as the source type. It will place the generated code inline, within previously made markup, which has to match the one from template:

```swift
// sourcery:inline:{{ type.name }}.AutoInterface
// sourcery:end
```

## Example
- Targeted type:

```swift
// sourcery:inline:WebServiceImp.AutoInterface
// sourcery:end

// sourcery: AutoInterface
public struct WebServiceImp {
    public private(set) var authorizationStatus: AuthorizationStatus?

    public func getUsers() -> Result<[User], ApiError> {
        // implementation...
    }
}

```

- Generated output:

```swift

// sourcery:inline:WebServiceImp.AutoInterface
public protocol WebService: AutoMockable, AutoDependency {
    var authorizationStatus: AuthorizationStatus? { get }
    func getUsers() -> Result<[User], ApiError>
}
// sourcery:end

// sourcery: AutoInterface
public struct WebServiceImp {
    public private(set) var authorizationStatus: AuthorizationStatus?

    public func getUsers() -> Result<[User], ApiError> {
        // implementation...
    }
}

```

## Reference

1. **Supported types**: structs, classes
2. **Output type**: inline
3. **Available annotations**:
	- interfaceinit - use this annotation if you wish to include init of type in the interface, by default, inits are excluded.

	```swift
    // sourcery: AutoInterface
    public struct WebServiceImp {
        // sourcery: interfaceinit
        public init(keychain: Keychain) {
            // implementation...
        }
    }
	```

	Generated interface will then look like this:

	```swift
    // sourcery: AutoMockable
    public protocol WebService {
        init(keychain: Keychain)
    }
	```
