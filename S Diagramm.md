```mermaid

sequenceDiagram
    actor User
    participant BookingSystem
    participant PaymentSystem
    participant Database
    participant NotificationSystem

    User->>BookingSystem: selectRoom(roomId, dates)
    BookingSystem->>BookingSystem: checkAvailability(roomId, dates)
    BookingSystem-->>User: availabilityOk / availableFalse

    alt available
        User->>BookingSystem: confirmBooking(userInfo)
        BookingSystem->>PaymentSystem: processPayment(paymentInfo, amount)
        PaymentSystem-->>BookingSystem: paymentResult(success/failed)

        alt payment success
            BookingSystem->>Database: saveBooking(booking)
            Database-->>BookingSystem: bookingId
            BookingSystem->>NotificationSystem: notifyUser(success, bookingId)
            NotificationSystem-->>User: notifyConfirmed(bookingId)
        else payment failed
            BookingSystem->>NotificationSystem: notifyUser(failed)
            NotificationSystem-->>User: notifyFailed
        end

    else not available
        BookingSystem-->>User: roomNotAvailable
    end
