# Definition

Consist of a subject (or publisher), which maintains a list of observers (or subscribers) and notifies them automatically  whenever it changes its state.

>[!IMPORTANT]
It stablishes a one-to-many dependency: when subject changes, observers get updated.

# Structure
## Subject (publisher)
Owns the state and sends notifications, allows observers to subscribe/unsubscribe.
## Observers (subscribers)
Depend on [[Observer#Subject (publisher)|subject]], and react to its notifications.

# Communication Styles

## Push
![[Excalidraw/Observer.excalidraw.md#^clippedframe=B1wZTswY3FW58W3MtBn-M|700]]
## Pull
![[Excalidraw/Observer.excalidraw.md#^frame=yJqjokgbQRyLSbHVxvC-C|700]]



