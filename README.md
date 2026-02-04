# Language Syntax Specification

## 1. Structure Definitions

### 1.1 Basic Class Definition
```language
StructureName = class {
    field1: Type
    field2: Type = defaultValue
    // ... additional fields
}
```

### 1.2  Compile-time Class Creation Syntax
```language
StructureName = class[ClassName](parameter1: Type, parameter2: Type) {
    // field assignments using parameters
    field1 = parameter1
    field2 = parameter2
}
```

### 1.3 Generic Class Syntax
```language
StructureName = class<GenericType>[parameter1: Type, parameter2: Type] {
    // field assignments using parameters
    field1 = parameter1
    field2 = parameter2
}
```

## 2. Function Definitions

### 2.1 Basic Function Syntax
```language
functionName = fn(parameter1: Type, parameter2: Type): ReturnType {
    // function body
    // statements
    return value
}
```

### 2.2  Function Syntax with Compile-time Class Parameter
```language
functionName = fn[ClassName](parameter: Type)=>returnObj: ReturnType {
    // function body with class-specific usage
    // statements
   
}
```

### 2.3 Generic Function Syntax
```language
functionName = fn<GenericType>[parameter: Type]: ReturnType {
    // function body with generic type usage
    // statements
    return value
}
```

## 3. Type System

### 3.1 Basic Types
- `class` - Class definition
- `fn` - Function type

### 3.2 Type Parameters in Angle Brackets `<>`
**Meaning**: Type parameter configuration at compile time
- Unlike C++ templates which are for type constraints, `<>` parameters mean you can get and update all fields by the Type
- Used for generic type definitions that can access and modify all fields of the specified type
- `fn<T>{}` means fn is a "Type method" of T, can use and modify all fields just for the Type
- Example: `processItem = fn<T>[item: T]` - function can access all fields of type T

### 3.3 Object Parameters in Square Brackets `[]`
**Meaning**: Object of some type at compile time
- Used to specify an object instance of a type that exists at compile time
- The object can be accessed and used within the function/type definition
- Example: `handleRequest = fn[service: ServiceClass]` - function uses service object

### 3.4 Methods and Object Access
- `fn[type: Type]` automatically means it is a "method" of the type
- Can use other fields in the type and can be used by `Type.fn`
- Similarly, `varname = var[type: Type]{...}` creates an object with type-specific access
### class  trait(interface)
- `ClassName:Interface`just like object：Class

## 4. Statement Syntax

### 4.1 Assignment Statements
```language
fieldName = expression
```

### 4.2 Method Invocation
```language
object.methodName(parameters)
```

### 4.3 Variable Declaration and Assignment
```language
variableName = expression
```

## 5. Complete Examples

### 5.1 Class Definition
```language
ServiceClass = class {
    firstFilter: Filter
    secondFilter: Filter
    serviceProxy: Proxy = Proxy()
}
```

### 5.2 Function Definition with Compile-time Class
```language
processRequest = fn[service: ServiceClass](request: Request): Response {
    service.firstFilter.edit(request)
    response = service.serviceProxy.send(request)
    service.secondFilter.edit(request, response)
    return response
}
```

### 5.3 Compile-time Class Type(like inheritance in OOP) 
```language
ExtendedService = class[baseService: ServiceClass](customFilter1: Filter, customFilter2: Filter) {
    baseService.firstFilter = customFilter1
    baseService.secondFilter = customFilter2
}
```

**Note**: Inheritance in OOP is equivalent to `class[sc: SuperClass] { sc*; ... }`

## 6. Comments
```language
// Single line comment
```

## 7. Naming Conventions
- **Classes**: PascalCase (e.g., `ServiceClass`)
- **Functions**: camelCase (e.g., `processRequest`)
- **Parameters**: camelCase (e.g., `request`, `response`)
- **Fields**: camelCase (e.g., `firstFilter`, `secondFilter`)
