# Time Integration

## Purpose

Time Integration is the DexAppKit implementation of TimeProvider. Integrating TimeProvider is required in order to persist time offsets and provide a reference to the TimeProvider component required for initializing the rest of DexAppKit and general system usage.

## Configurability
* **BaseUrl:** Base URL for the server to contact, see Definitions for BaseDataUrl environment examples (Scopes and Definitions)
* **txTickTimePublisher:** Emits events periodically with the current transmitter time in milliseconds.

## Models

The TimeIntegration class instantiates the TimeProvider component and manages the dispersal of certainty information between the various sub components in the DexAppKit.


PLACEHOLDER 1


## Event Scheduling Workflows


PLACEHOLDER 2
