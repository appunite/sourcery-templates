## Description
This template is used to generate mocks for interfaces that you usually inject to objects that you wish to unit test. For each method and variable within interface, it will create set of variables with which you can control returned value, test how many times given method was called or verify if passed arguments were correct.

## Example
- Targeted type:

```swift
// sourcery: AutoMockable
protocol WebService {
    func getUser(withId id: Int) -> Result<User, WebServiceError>
}

```

- Generated output:

```swift
class WebServiceMock: WebService {

    //MARK: - getUser

    var getUserWithIdCallsCount = 0
    var getUserWithIdCalled: Bool {
        return getUserWithIdCallsCount > 0
    }
    var getUserWithIdReceivedId: Int?
    var getUserWithIdReturnValue: Result<User>!
    var getUserWithIdClosure: ((Int) -> Result<User>)?

    func getUser(withId id: Int) -> Result<User> {
        getUserWithIdCallsCount += 1
        getUserWithIdReceivedId = id
        return getUserWithIdClosure.map({ $0(id) }) ?? getUserWithIdReturnValue
    }
}
```

## Reference

1. **Supported types**: protocols
2. **Output type**: single file
3. **Available annotations**: -

## Source
[https://github.com/krzysztofzablocki/Sourcery/blob/master/Templates/Templates/AutoMockable.stencil](https://github.com/krzysztofzablocki/Sourcery/blob/master/Templates/Templates/AutoMockable.stencil)
