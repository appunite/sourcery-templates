## Description
This template is used to generate protocol with the interface for given structure or class - it will contain only those methods and properties getters/setters which have same access level as source type. It will generate one file per targeted type and place it in the same directory. Since targeted types might be located in different directories, you cannot place list of imports on top of the template as you would in other templates which place the generated code inline or in a single file - those imports need to match the imports of the file which defines the targeted type. Sourcery does not support extracting imports and therefore we need to use this bash script and run it everytime we generate a new interface or modify the existing one:

`git status | grep "filegenerated" | sed 's/modified://' | xargs -I % sh -c 'cat % | grep "bash:imports" | sed -e 's/bash:imports//' | xargs -L 1 cat | grep "import " >> %'`

It will simply look through modified files tracked by git, grab the ones generated by this template, look for path to the original file and copy imports from there. If you are not checking out Sourcery-generated code in your repository, you will need to use different script.

## Example
- Targeted type:

```swift
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
// sourcery: AutoMockable, AutoDependency
public protocol WebService {
    var authorizationStatus: AuthorizationStatus? { get }
    func getUsers() -> Result<[User], ApiError>
}
extension WebServiceImp: WebService {}

```

## Reference

1. **Supported types**: structs, classes
2. **Output type**: file per type
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
    // sourcery: AutoMockable, AutoDependency
    public protocol WebService {
        init(keychain: Keychain)
    }
    extension WebServiceImp: WebService {}
	
	```
	- NoAutoDependency - use this annotation on type if you wish to make generated interface not be annotated with AutoDependency. If you are not using AutoDependency at all, you will want to remove it from template completely.
	
	

## Source 
_