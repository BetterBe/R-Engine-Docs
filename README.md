# **R-Engine Contents**

# **Base API**
This API is intended to be used for event-driven programming. Coroutines are not required, and should not be
used unless otherwise required. Please follow optimization guidelines and consult with the API designer(s) before utilizing coroutines.

The Core of R-Engines design is light-weight modules indended to be re-used across multiple projects, and as such please do NOT modify
contents of the internal engine without consultation and redocumentation. 

Most of the R-Engine source will be located under the **SHARED** folder- and anything that pertain specifically to the client or server should remain in their respective **CLIENT** and **SERVER** folders.

....




# **Shared.Audio**
## Sound2D (WIP)
## Sound3D (WIP)
...
# **Shared.ItemInfo**
## Info (WIP)
...
# **Shared.Math**
## Easing
The easing module is intended to be used in conjunction with RunService or to create smoothe transitions for UI, while returning a value from 0-1 for scalability.

OutCubic Example:
```typescript
function getOutCubicFromStep()
{
    let step = this.currentTime / this.timeToShow
    let scaled = outCubic(step, 0, 1, 1) //0, 1, 1 is required to maintain a 0-1 return.
    return scaled
}
```

**Any Arguments of 'a', 'p', or 's' mean 'Amplitude', 'Frequency', and 'Elipson' respectively.**

```ts
function linear(t: number, b: number, c: number, d: number): number 
function inQuad(t: number, b: number, c: number, d: number): number
function outQuad(t: number, b: number, c: number, d: number): number
function inOutQuad(t: number, b: number, c: number, d: number): number
function outInQuad(t: number, b: number, c: number, d: number): number
function inCubic(t: number, b: number, c: number, d: number): number
function outCubic(t: number, b: number, c: number, d: number): number
function inOutCubic(t: number, b: number, c: number, d: number): number
function outInCubic(t: number, b: number, c: number, d: number): number
function inQuart(t: number, b: number, c: number, d: number): number
function outQuart(t: number, b: number, c: number, d: number): number
function inOutQuart(t: number, b: number, c: number, d: number): number
function outInQuart(t: number, b: number, c: number, d: number): number
function inQuint(t: number, b: number, c: number, d: number): number
function outQuint(t: number, b: number, c: number, d: number): number
function inOutQuint(t: number, b: number, c: number, d: number): number
function outInQuint(t: number, b: number, c: number, d: number): number
function inSine(t: number, b: number, c: number, d: number): number
function outSine(t: number, b: number, c: number, d: number): number
function inOutSine(t: number, b: number, c: number, d: number): number
function outInSine(t: number, b: number, c: number, d: number): number
function inExpo(t: number, b: number, c: number, d: number): number
function outExpo(t: number, b: number, c: number, d: number): number
function inOutExpo(t: number, b: number, c: number, d: number): number
function outInExpo(t: number, b: number, c: number, d: number): number
function inCirc(t: number, b: number, c: number, d: number): number
function outCirc(t: number, b: number, c: number, d: number): number
function inOutCirc(t: number, b: number, c: number, d: number): number
function outInCirc(t: number, b: number, c: number, d: number): number
function inElastic(t: number, b: number, c: number, d: number, a: number, p: number): number
function outElastic(t: number, b: number, c: number, d: number, a: number, p: number): number
function inOutElastic(t: number, b: number, c: number, d: number, a: number, p: number): number
function outInElastic(t: number, b: number, c: number, d: number, a: number, p: number): number
function inBack(t: number, b: number, c: number, d: number, s: number): number
function outBack(t: number, b: number, c: number, d: number, s: number): number
function inOutBack(t: number, b: number, c: number, d: number, s: number): number
function outInBack(t: number, b: number, c: number, d: number, s: number): number
function outBounce(t: number, b: number, c: number, d: number): number
function inBounce(t: number, b: number, c: number, d: number): number
function inOutBounce(t: number, b: number, c: number, d: number): number
function outInBounce(t: number, b: number, c: number, d: number): number
```
...
## Intersections (WIP)
Intended as a module to aid 2D collision detection

...
## Noise (WIP)
Intended as a module to generate noise-maps for seed-based randomized worlds.

...
## Spline (WIP)
Intended as a means to smooth out pathfinder generated paths.

...
## Transform (WIP)
Intended and a Quaternion library for future projects.

...

# **Shared.Network**

## ClientNetworkChannel (WIP)
Intended for use as a delegate of UniverseNetwork

...
## LocalNetwork (WIP)
Intended for use as a communicator between the game, and the users local computer.

...

## Network

The Networking of R-Engine is intended to be event based, and have no reliances on the volitile "RemoteFunction" instance.

The Internals of each Network Class `Network.Server` and `Network.Client` are:
```typescript
public answerServer(r: RequestIDInternal, cb: Callback)
public answerClient(client: Player, r: RequestIDInternal, cb: Callback)
public answerAllClients(r: RequestIDInternal, cb: Callback)
public requestClient(client: Player, r: RequestIDInternal, timeout: number | void): NetworkResponse
public requestServer(r: number, timeout: number | void): NetworkResponse
protected pingServer(packageId: any, data?: any[])
protected pingClient(client: Player, packageId: any, data?: any[])
protected pingAllClients(packageId: any, data?: any[])
```

However the exposed methods and build-ons are what make each class special and more versatile while maintaining a simple "ping" method between both client and server for simplicity.

### ~**Class Network.Client**~

- **answerServer(requestID, callback)**: Called when the server sends a request for a value from the client. The return of `callback` will be returned the server.
- **requestServer(requestID [, number timeout]) -> NetworkResponse**: Called to request a array of values from the server. A NetworkResponse (*alias for Array\<unknown>*) is returned. The server can return whatever data it feels is being requested.
- **ping(packageID, any[]?)**: Called to simply signal to the server an event took place. This is like firing a normal RemoteEvent.

### ~**Class Network.Server**~

- **answerClient(Player, requestID, callback)**: Called when a specific client sends a request for a value from the server. The return of `callback` will be returned the client.
- **answerAllClients(requestID, callback)**: Called when **ANY** client sends a request for a value from the server. The return of `callback` will be returned the client.
- **requestClient(requestID [, number timeout]) -> NetworkResponse**: Called to request a array of values from the client. A NetworkResponse (*alias for Array\<unknown>*) is returned. The client can return whatever data it feels is being requested.
- **ping(Player, packageID, any[]?)**: Called to simply signal to the client an event took place. This is like firing a normal RemoteEvent.
- **pingAll(packageID, any[]?)**: Called to simply signal to **EVERY*** client an event took place. This is like firing a normal RemoteEvent.


...
## Packaging

Sending messages and requests that can be efficiently picked apart between game tasks require unique package identifiers for delegating events to the correct parts of the program.

A Network will accept a `MessageID`, and `RequestID` for pings and requests respectively. These are aliases for `number`.

To Generate a `MessageID` or `RequestID`, under packaging we call `createMessageID()` and `createRequestID()` respectively.

### Setting up a Network Package Category:
If we want to create a set of different Message or Request ID's and place them in categories, we can do so like such:
```typescript
export enum Stats
{
    SEND_PING = createMessageID()
    SEND_FPS = createMessageID()            //client_network.ping(Stats.SEND_FPS, currentFrames)
    GET_PLAYER_CASH = createRequestID()     //let cash = client_network.requestServer(Stats.GET_PLAYER_CASH)
    GET_PLAYER_CLOTHES = createRequestID()
}


```
...

## PlayerNetwork (VirtualPlayer vPlayer)

The file for this is currently small:
```typescript
import { VirtualPlayer } from "server/Virtual/VirtualPlayer";
import { Network, NetworkResponse } from "./Network";
import { MessageIDInternal, RequestIDInternal } from "./packaging";

export class PlayerNetwork extends Network.Server
{
    protected player: VirtualPlayer
    constructor(player: VirtualPlayer)
    {
        super()
        this.player = player
    }

    public message = (msgId: MessageIDInternal, ...data: any)=>{ this.pingClient(this.player.player, msgId, data) }
    public request = (r: RequestIDInternal, timeout: number | void): NetworkResponse=>{ return this.requestClient(this.player.player, r, timeout) }
    public answer = (r: RequestIDInternal, cb: Callback)=>{ this.answerClient(this.player.player, r, cb) }
}
```

This class is intended to be used from the Server to handle network operations with a specific player. 

...
## UniverseNetwork (WIP)
Intended as a Cross-Server Network class

## UniverseTopicChannel (WIP)
Intended to aid Cross-Server networking by creating topics to subscribe to for event handling.

## WebServer (WIP)
Intended as a HTTPService Wrapper. 

# Shared.Pathfinding
## AStarPathfinder ()

The only item here that matters is
```ts
public findPath(startingNode: Node3, endingNode: Node3): Array<Vector3>
```
If a viable path exists, the returned array will be populated, otherwise it will be empty.

## Node3 (Vector3 Position)
This is intended to be a pathway/connection marker.
```ts
let node = new Node3(vec3_position)
```
When you want to signal a connecting node, simply call `node.addNode(other_Node)`

...
# Shared.Physics
## CollisionGroup (String ID)
This handles Collision allowance between Instances or parts. When a Instance is provided or removed via `addInstance` or `removeInstance`, it will append ALL BasePart descendants into the collision group.
```ts
public addPart(p: BasePart)
public containsPart(p: BasePart): boolean
public canCollideWith(group: CollisionGroup): boolean
public toggleCollisions(group: CollisionGroup, enabled: boolean)
public addInstance(i: Instance)
public removeInstance(i: Instance)
```
...
# Shared.Utils (Miscellanious Utilities)
## AssetID
```ts
function assetID(s: string | number): string {...} //returns "rbxassetid:// ...."
```
## SelectDescendingClasses
```ts
function SelectDescendingClasses<T extends Instance>(instance: Instance, className: string): Array<T>
```
Returns an array of objects that extend 'T' within the scanned Instance's descendant list.

## StopTimer (number Seconds)
```ts
class StopTimer
{
    onFinish: Event<Callback> //fired after an internal stop-call.
    public start()
    public stop() //resets
    public pause() //suspends
    public resume()
}
```

## TimedEvent (number Interval)

```ts
class TimedEvent
{
    public onTick: Event<()=>void> //fired every interval
    public start()
    public resume()
    public stop()
    public pause()
}
```

## Welder
```ts
namespace Welder
{
    enum weldClass
    {
        Motor6D = 'Motor6D',
        Weld = 'Weld'
    }
    function weldRelativeTo (reference: BasePart, target: BasePart, weldClass: weldClass): Weld
    function weldByC0 (reference: BasePart, target: BasePart, c0: CFrame, weldClass: weldClass): Weld
}
```

## WaitForDescendants
```ts
function WaitForDescendants(instance: Instance, timeout: number|void)
```
