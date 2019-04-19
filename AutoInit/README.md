## Description
This template is used to generate init with the same access level as the targeted type. The generated code will be placed within the targeted type declaration, which means that this template can also be used for classes.

## Example
- Targeted type:

```swift
public struct SuccessMessage: AutoInit {
    public let title: String
    public let subtitle: String
}
```

- Generated output:

```swift
public struct SuccessMessage: AutoInit {
    public let title: String
    public let subtitle: String

// sourcery:inline:auto:SuccessMessage.AutoInit
    public init(title: String, subtitle: String) { // swiftlint:disable:this line_length
        self.title = title
        self.subtitle = subtitle
    }
// sourcery:end
}
```

## Reference

1. **Supported types**: structs, classes
2. **Output type**: auto inline
3. **Available annotations**: -

## Source 
[https://gist.github.com/Liquidsoul/efa4f65af055acfde64afed6cda0007d](https://gist.github.com/Liquidsoul/efa4f65af055acfde64afed6cda0007d)