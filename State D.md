```mermaid
stateDiagram-v2
    [*] --> Idle
    
    Idle --> RoomSelected: Выбор номера
    RoomSelected --> Idle: Отмена выбора
    RoomSelected --> BookingConfirmed: Подтверждение бронирования
    RoomSelected --> BookingCancelled: Отмена бронирования
    
    BookingConfirmed --> Paid: Оплата
    BookingConfirmed --> BookingCancelled: Отмена бронирования
    BookingConfirmed --> BookingConfirmed: Изменение дат
    
    Paid --> Completed: Завершение бронирования
    Paid --> BookingCancelled: Отмена с возвратом средств
    
    BookingCancelled --> Idle: Новое бронирование
    
    Completed --> [*]
