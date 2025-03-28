# nasledovanie
#include <iostream>
#include <string>

// Базовый класс для электрических транспортных средств
class ElectricVehicle {
protected:
    int batteryLevel; // Уровень заряда батареи

public:
    ElectricVehicle() : batteryLevel(100) {} // Изначально батарея заряжена на 100%

    void charge(int amount) {
        batteryLevel += amount;
        if (batteryLevel > 100) batteryLevel = 100; // Максимум 100%
    }

    void showBattery() const {
        std::cout << "Уровень заряда батареи: " << batteryLevel << "%" << std::endl;
    }
};

// Базовый класс для наземного транспорта
class LandTransport {
protected:
    int speed; // Скорость

public:
    LandTransport() : speed(0) {}

    virtual void accelerate(int value) {
        speed += value;
        std::cout << "Увеличена скорость до: " << speed << " км/ч" << std::endl;
    }

    virtual void brake(int value) {
        speed -= value;
        if (speed < 0) speed = 0;
        std::cout << "Снижена скорость до: " << speed << " км/ч" << std::endl;
    }

    virtual void move() { // Виртуальный метод для движения
        std::cout << "Наземный транспорт движется." << std::endl;
    }
};

// Базовый класс для водного транспорта
class WaterTransport {
protected:
    int depth; // Глубина

public:
    WaterTransport() : depth(0) {}

    virtual void dive(int value) {
        depth += value;
        std::cout << "Достигнута глубина: " << depth << " метров" << std::endl;
    }

    virtual void surface(int value) {
        depth -= value;
        if (depth < 0) depth = 0;
        std::cout << "Поднялись на глубину: " << depth << " метров" << std::endl;
    }

    virtual void move() { // Виртуальный метод для движения
        std::cout << "Водный транспорт движется." << std::endl;
    }
};

// Класс для электрического автомобиля
class ElectricCar : public ElectricVehicle, public LandTransport {
public:
    void drive() {
        std::cout << "Электромобиль движется." << std::endl; 
    }

    void move() override { // Переопределение метода move
        std::cout << "Электромобиль движется по земле." << std::endl;
    }
};

// Класс для электрической лодки
class ElectricBoat : public ElectricVehicle, public WaterTransport {
public:
    void sail() {
        std::cout << "Электроход движется." << std::endl; 
    }

    void move() override { // Переопределение метода move
        std::cout << "Электроход движется по воде." << std::endl;
    }
};

// Класс для амфибийного транспортного средства
class AmphibiousVehicle : public ElectricCar, public ElectricBoat {
public:
    void switchToLandMode() {
        std::cout << "Переключение в режим наземного транспорта." << std::endl;
    }

    void switchToWaterMode() {
        std::cout << "Переключение в режим водного транспорта." << std::endl;
    }

    void drive() {
        ElectricCar::drive(); // Вызов метода drive() из ElectricCar
    }

    void sail() {
        ElectricBoat::sail(); // Вызов метода sail() из ElectricBoat
    }

    void move() {
        ElectricCar::move(); // Вызов метода move() из ElectricCar
        ElectricBoat::move(); // Вызов метода move() из ElectricBoat
    }
};

int main() {
    AmphibiousVehicle amphibiousVehicle;

    // Пример работы
    amphibiousVehicle.showBattery();       // Показываем уровень зарядки
    amphibiousVehicle.charge(20);          // Заряжаем батарею
    amphibiousVehicle.showBattery();       // Показываем уровень зарядки

    amphibiousVehicle.accelerate(50);      // Увеличиваем скорость
    amphibiousVehicle.drive();              // Заводим электрический автомобиль

    amphibiousVehicle.dive(10);             // Достигли глубины
    amphibiousVehicle.sail();               // Заводим электрическую лодку

    amphibiousVehicle.switchToWaterMode();  // Переключаемся в режим водного транспорта
    amphibiousVehicle.switchToLandMode();   // Переключаемся в режим наземного транспорта

    // Показать движение
    amphibiousVehicle.move();               // Показываем движение в обоих режимах

    return 0;
}
