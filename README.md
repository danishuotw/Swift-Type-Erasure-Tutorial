# Swift-Type-Erasure-Tutorial


```swift
protocol CarType {
    associatedtype Spec
    func printSpec() -> Spec
}

class BMW740: CarType {
    typealias Spec = Specifications

    func printSpec() -> BMW740.Spec {
        let spec = Specifications(fuelType: "petrol", maxPower: "600.7bhp@5400-6500rpm", seatingCapacity: "4")
        print("BMW740 Specifications -> \(spec)")
        return spec
    }
}

class BenzS: CarType {
    typealias Spec = Specifications

    func printSpec() -> BMW740.Spec {
        let spec = Specifications(fuelType: "petrol", maxPower: "630bhp@5000rpm", seatingCapacity: "4")
        print("BenzS Specifications -> \(spec)")
        return spec
    }
}

struct Specifications {
    var fuelType: String
    var maxPower: String
    var seatingCapacity: String
}

struct AnyCar<Specifications>: CarType {
    private let _printSpec: () -> Specifications

    init<Car: CarType>(_ car: Car) where Car.Spec == Specifications {
        _printSpec = car.printSpec
    }

    func printSpec() -> Specifications {
        return _printSpec()
    }
}


let cars = [AnyCar(BMW740()), AnyCar(BenzS())]
_ = cars.forEach { $0.printSpec() }
```
