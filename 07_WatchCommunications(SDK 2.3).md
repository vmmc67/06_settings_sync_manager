# Watch Communication (SDK 2.3)

Watch Communication is a wrapper around the WatchConnectivity framework, the iOS/watchOS framework that allows watch-to-phone communications on the Apple platform.

The WatchCommunication instance is created by DexKitOrchestrator. The user should use that instance method.


## Notes On The Application Context

The Application Context is a WCSession mechanism provided by the OS to sync data between counterpart applications. It is a dictionary of type [String: Any], that when modified will have its contents synced with the counterpart automatically. The priority this sync is given by the system is not guaranteed, but in practice seems to be extremely high. There is a maximum context size of 262KB, which must be adhered to!

In the WatchCommunication class we are abstracting the session context interface to wrap our serialization and messaging features around it - we are not allowing direct access to the contents of the context. This also helps with enforcing types, as despite being Any type for values, only plist types are valid, and ultimately we only end up storing Data.

## Properties
