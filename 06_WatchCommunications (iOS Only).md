# Watch Communication (iOS Only)


Watch Communication is a wrapper around the WatchConnectivity framework, the iOS/watchOS framework that allows watch-to-phone communications on the Apple platform.

The WatchCommunication instance is created by DexKitOrchestrator, and the user should use that instance method.


## Properties

Property                                 | Type  | Description
---------------------------------------- | ------| ------------------
| isCommunicationOpen                    | Bool  | Can we currently send messages across. You should check this at opportune times to see if you can send a message. You should plan on communication being unavailable most of the time.
| isComplicationEnabled                  | Bool  | isComplicationEnabled (iOS Only)
| isPaired                               | Bool  | Are we currently paired to a watch? (iOS Only)
| isSessionActivated                     | Bool  | Is the WatchConnectivity session activated
| isWatchAppInstalled                    | Bool  | Is the watch extension currently installed? (iOS Only)
| remainingComplicationUserInfoTransfers | Int   | Number of remaining complication transfers we have left (using sendComplicationData) (iOS Only)
| watchDirectoryURL                      | URL?  | A directory for storing information specific to the currently paired and active Apple Watch
| watchStateChangePublisher              | PassthroughSubject<Bool, Never> | A publisher which emits upon WCSessionDelegate.sessionWatchStateDidChange firing (iOS Only)


## Methods

Method                       | Description                
-----------------------------| ---------------------
sendMessage<T: WatchMessagePayload>(_ type: WatchMessageType, _ payload: T) -> Future<Void, WCError>        | Sends a message to the other device using Watch Connectivity's `sendMessage` command
sendComplicationData<T: WatchMessagePayload>(_ type: WatchMessageType, _ payload: T) -> Future<Void, WCError>  | Send Complication Data using the rate-limited API (iOS Only)
func transferUserInfo<T: WatchMessagePayload>(_ type: WatchMessageType, _ payload: T) -> Result<WCSessionUserInfoTransfer, WCError>       | Transfers payload data to the other device using Watch Connectivity's `transferUserInfo` command.
listenFor(filesWithTypes messageTypes: [WatchMessageType] = [WatchMessageType]()) -> AnyPublisher<WatchFileResult, Never>         | Listen for file transfers with the given message types, which should be unique, or an empty array to listen for all file transfer events
itransferFile(url: URL, metadata: [String: Any]?, type: WatchMessageType, deletingAfterward: Bool = false) -> WCError?  | Begin the file transfer process for the file at the given URL, with optional additional metadata and a message type identifier (which should be unique to prevent collisions of the same file name). Whether or not to delete the file upon completion (success AND failure), defaulting to false, is optionally available.


## Enums

Enums             | Description    | Case      | Description
------------------|----------------| ----------|--------------
| WatchFileResult | An enum with associated objects which describes either a successful file transfer, or a failed transfer       | success(fileURL: URL, metadata: [String: Any]?, type: WatchMessageType   | A file transferred successfully, with the fileURL to retrieve it, the optional metadata, and the message type data attached
|                 |                | failure(type: WatchMessageType)   | A file transfer failed, with the message type of the request attached

#### Startup

![](../../../images/DexAppKit/settings_sync_manager/WatchComiOS_Starup.png)


#### Send Message Example (Inside Dak)

![](../../../images/DexAppKit/settings_sync_manager/WatchComiOS_SendMessageEx.png)


#### Transfer User Info Example (Inside Dak)


![](../../../images/DexAppKit/settings_sync_manager/WatchComiOS_TransUserInfoEx.png)


#### Successful Send File Example (Phone requests watch DB, please note that DebugDBImporterExporter is an app layer class and not within the SDK)


![](../../../images/DexAppKit/settings_sync_manager/WatchComiOS_SuccSendFileEx.png)


#### Unsuccessful Send File Example (Phone requests watch DB, please note that DebugDBImporterExporter is an app layer class and not within the SDK)


![](../../../images/DexAppKit/settings_sync_manager/WatchComiOS_UnsuccSendFileEx.png)
