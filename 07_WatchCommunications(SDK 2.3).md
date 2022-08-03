# Watch Communication (SDK 2.3)

Watch Communication is a wrapper around the WatchConnectivity framework, the iOS/watchOS framework that allows watch-to-phone communications on the Apple platform.

The WatchCommunication instance is created by DexKitOrchestrator. The user should use that instance method.


## Notes On The Application Context

The Application Context is a WCSession mechanism provided by the OS to sync data between counterpart applications. It is a dictionary of type [String: Any], that when modified will have its contents synced with the counterpart automatically. The priority this sync is given by the system is not guaranteed, but in practice seems to be extremely high. There is a maximum context size of 262KB, which must be adhered to!

In the WatchCommunication class we are abstracting the session context interface to wrap our serialization and messaging features around it - we are not allowing direct access to the contents of the context. This also helps with enforcing types, as despite being Any type for values, only plist types are valid, and ultimately we only end up storing Data.

## Properties

Property                                 | Type  | Description
---------------------------------------- | ------| ------------------
| isCommunicationOpen                    | Bool  | Can we currently send messages across. You should check this at opportune times to see if you can send a message. You should plan on communication being unavailable most of the time.
| isSessionActivated                | Bool  | is the WatchConnectivity session activated?
| isPaired                               | Bool  | Are we currently paired to a watch? (iOS Only)
| isWatchAppInstalled                    | Bool  | Is the watch extension currently installed? (iOS Only)
| isComplicationEnabled                    | Bool  | isComplicationEnabled? (iOS Only)
| remainingComplicationUserInfoTransfers | Int   | Number of remaining complication transfers we have left (using sendComplicationData) (iOS Only)
| watchDirectoryURL                      | URL?  | A directory for storing information specific to the currently paired and active Apple Watch
| watchStateChangePublisher              | PassthroughSubject<Bool, Never> | A publisher which emits upon WCSessionDelegate.sessionWatchStateDidChange firing (iOS Only)



## Methods

Method                       | Description                
-----------------------------| ---------------------
 | listenFor<T: Codable>(_ messageType: WatchMessageType, type: T.Type) -> AnyPublisher<T, Never>                          | Listen for an incoming message of a given type
 | sendMessage<T: WatchMessagePayload>(_ type: WatchMessageType, _ payload: T) -> Future<Void, WCError>                          | Sends a message to the other device using Watch Connectivity's "sendMessage" command
 | sendComplicationData<T: WatchMessagePayload>(_ type: WatchMessageType, _ payload: T) -> Future<Void, WCError>                          | Send Complication Data using the rate-limited API (iOS Only)
 | func transferUserInfo<T: WatchMessagePayload>(_ type: WatchMessageType, _ payload: T) -> Result<WCSessionUserInfoTransfer, WCError>                          | Transfers payload data to the other device using Watch Connectivity's "transferUserInfo" command
 | listenFor(filesWithTypes messageTypes: [WatchMessageType] = [WatchMessageType]()) -> AnyPublisher<WatchFileResult, Never>                          | Listen for file transfers with the given message types, which should be unique, or an empty array to listen for all file transfer events
 | transferFile(url: URL, metadata: [String: Any]?, type: WatchMessageType, deletingAfterward: Bool = false) -> WCError?                          | Begin the file transfer process for the file at the given URL, with optional additional metadata and a message type identifier (which should be unique to prevent collisions of the same file name). Whether or not to delete the file upon completion (success AND failure), defaulting to false, is optionally available.
 | setApplicationContext<T: WatchMessagePayload>(_ type: WatchMessageType, _ payload: T?) -> Result<Void, Error>                          | Sets the session context dictionary with a value conforming to WatchMessagePayload, and a key of WatchMessageType. When the counterpart receives this context update, it will send a message to any listeners registered against that message type, and additionally the data will be available via the receivedApplicationContext function. On the sending side, this updated data will be available via the applicationContext function. Sending nil as a payload will remove the value for the given key.
 | applicationContext<T: WatchMessagePayload>(_ messageType: WatchMessageType, type: T.Type) -> T?                          | Queries the local copy of applicationContext, which is exactly what has been sent or will be sent to the counterpart app. We are wrapping the session functionality here, so a message type key must be passed, and the type of the object expected must also be passed, so that decoding can be performed. If an object of that type for that key is available in the dictionary, it will be decoded and returned, else nil.
 | receivedApplicationContext<T: WatchMessagePayload>(_ messageType: WatchMessageType, type: T.Type) -> T?                          | Queries the local copy of receivedApplicationContext, which is exactly what has been received from the counterpart app. We are wrapping the session functionality here, so a message type key must be passed, and the type of the object expected must also be passed, so that decoding can be performed. If an object of that type, for that key is available in the dictionary, it will be decoded and returned, else nil.


## Enums

 Enums             | Description    | Case      | Description
 ------------------|----------------| ----------|--------------
 | WatchFileResult | An enum with associated objects which describes either a successful file transfer, or a failed transfer.       | success(fileURL: URL, metadata: [String: Any]?, type: WatchMessageType   | A file transferred successfully, with the fileURL to retrieve it, the optional metadata, and the message type data attached
 |                 |                | failure(type: WatchMessageType)   | A file transfer failed, with the message type of the request attached



 PLACEHOLDER 1



 PLACEHOLDER 2



 PLACEHOLDER 3



 PLACEHOLDER 4



 PLACEHOLDER 5



 PLACEHOLDER 6
