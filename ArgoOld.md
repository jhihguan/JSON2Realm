Here's a backup for old Argo initialization without Curry

```Swift
class ArgoClass: BasicClass {
    convenience required init(name: String, birthday: String, age: Int) {
        self.init()
        self.name = name
        self.birthday = birthday
        self.age = age
    }
}

extension ArgoClass: Decodable {

    static func create(name: String) -> String -> Int -> ArgoClass {
        return
            { birthday in
                { age in
                    self.init(name: name, birthday: birthday, age: age)
            }
        }
    }

    // can't use curry(self.init)
    // will error with "Expression was too complext...
    static func decode(json: JSON) -> Decoded<ArgoClass> {
        return ArgoClass.create
            <^> json <| "name"
            <*> json <| "birthday"
            <*> json <| "age"

    }
}

class ArgoOptionalClass: BasicOptionalClass {
    convenience required init(distance: Int?, note: String?, value: Int) {
        self.init()
        self.distance.value = distance
        self.note = note
        self.value = value
    }
}

extension ArgoOptionalClass: Decodable {
    static func create(distance: Int?) -> String? -> Int -> ArgoOptionalClass {
        return
            { note in
                { value in
                    return self.init(distance: distance, note: note, value: value)
                }
            }
    }
    static func decode(json: JSON) -> Decoded<ArgoOptionalClass> {
        return ArgoOptionalClass.create
            <^> json <|? "distance"
            <*> json <|? "note"
            <*> json <| "value"
    }
}
```
