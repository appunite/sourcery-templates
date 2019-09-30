## Description
This template is used to generate static methods returning instances of structures and classes, this method  will have default argument for every single property of given class or struct, making it easy to instantiate it in your unit tests - `User.mock()`. When testing, in most cases, you don't need to set values for all of the properties in the model - you need to verify only one or two, but your inits will have no default arguments, forcing you to set all of the fields when creating the instance. Using these static methods solves the issue, allowing you to instantiate the model only with those arguments which are important in given test, while others are set to default values.

## Example
- Targeted type:

```swift
// sourcery: AutoModelMockable
public struct User {
    public let id: String
    public let username: String
    public let emailAddress: String?
    public let firstName: String?
    public let lastName: String?
}

```

- Generated output:

```swift
extension User {
    static func mock(id: String = .mock(), username: String = .mock(), emailAddress: String? = nil, firstName: String? = nil, lastName: String? = nil) -> User {
        return User(id: id, username: username, emailAddress: emailAddress, firstName: firstName, lastName: lastName)
    }
}

private extension Bool {
    static func mock() -> Bool {
        return false
    }
}

private extension StringProtocol {
    static func mock() -> Self {
        return "Default text"
    }
}

private extension ExpressibleByIntegerLiteral {
    static func mock() -> Self {
        return 7
    }
}

```

## Reference

1. **Supported types**: structs, classes
2. **Output type**: single file
3. **Available annotations**:
	- defaultMock - use this annotation if you wish to control the generated default init argument for given property:

	```swift
	// sourcery: AutoModelMockable
	public struct User: Codable {
		public let id: String
		// sourcery: defaultMock = ""MyUsername""
		public let username: String
	}
	```
	Generated method will then look like this:

	```swift
    static func mock(id: String = .mock(), username: String = "MyUsername") -> User {
        return User(id: id, username: username, emailAddress: emailAddress, firstName: firstName, lastName: lastName)
    }

	```
