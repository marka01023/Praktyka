```mermaid
stateDiagram-v2
    [*] --> Idle
    
    Idle --> CarSelected: Выбрать автомобиль
    CarSelected --> Idle: Отмена выбора
    CarSelected --> OrderConfirmed: Подтвердить заказ
    CarSelected --> TripCancelled: Отменить заказ
    
    OrderConfirmed --> CarArrived: Автомобиль прибыл
    OrderConfirmed --> TripCancelled: Отменить заказ
    OrderConfirmed --> OrderConfirmed: Автомобиль задерживается
    
    CarArrived --> InTrip: Начать поездку
    CarArrived --> TripCancelled: Отменить заказ
    
    InTrip --> TripCompleted: Завершить поездку
    
    TripCompleted --> Idle: Оплата получена
    TripCompleted --> TripCompleted: Проблема с оплатой
    
    TripCancelled --> Idle: Вернуться в ожидание
