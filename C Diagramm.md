```mermaid

sequenceDiagram
    participant User
    participant ShoppingCart
    participant OrderProcessor
    participant PaymentSystem
    participant Warehouse
    participant Database
    participant NotificationSystem

    User ->> ShoppingCart: addItem(item)
    User ->> OrderProcessor: placeOrder(cart, userInfo)
    OrderProcessor ->> PaymentSystem: processPayment(paymentInfo, amount)
    PaymentSystem -->> OrderProcessor: paymentResult

    alt payment success
        OrderProcessor ->> Warehouse: reserveItems(cart)
        Warehouse -->> OrderProcessor: reserved / insufficient

        alt reserved
            OrderProcessor ->> Database: saveOrder(order)
            Database -->> OrderProcessor: orderId
            OrderProcessor ->> NotificationSystem: notifyUser(success)
            NotificationSystem -->> User: notifyConfirmed
        else insufficient
            OrderProcessor ->> NotificationSystem: notifyUser(failed)
            NotificationSystem -->> User: notifyFailed
        end

    else payment failed
        OrderProcessor ->> NotificationSystem: notifyUser(failed)
        NotificationSystem -->> User: notifyFailed
    end
