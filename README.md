The [Source engine](https://developer.valvesoftware.com/wiki/Source) is a 3D video game engine developed by [Valve](https://developer.valvesoftware.com/wiki/Valve). The Source engine is well-known for its advancements in physics, AI, and graphics which made the games realistic for its time, while being scalable on older, less powerful hardware. Source engine **servers** are hosted game services that allow players to connect and play together in the server's world. Servers in the Source engine can be customized with **mods** that results in a unique and often times fun gameplay experience for all players connected to the server.

Games that run on the Source engine include [Team Fortress 2](https://store.steampowered.com/app/440/Team_Fortress_2/), [Counter-Strike: Source](https://store.steampowered.com/app/240/CounterStrike_Source/), [Garry's Mod](https://store.steampowered.com/app/4000/Garrys_Mod/), and many others!

A common question asked is what are the hardware and network requirements for running a Source engine server, along with how it compares to servers in other games such as [Minecraft](https://www.minecraft.net/en-us) and [Rust](https://rust.facepunch.com/). The answer to this question is, it *really depends*.

While Source engine servers are **a lot** lighter on RAM usage compared to other games like Minecraft and Rust, they are *mostly* **single-threaded**. This means having a processor with a low core count, but high single-threaded performance is going to perform better than a processor with many cores, but low single-threaded performance for Source engine servers. This is especially true if you plan on hosting an intensive game mode or want to have many players connected to the server at once.

With that said, here is a list of additional factors to consider when deciding your hardware and network requirements.

* The peak amount of players the server will have connected and playing at once.
* The game mode, amount of mods, and what type of maps the server runs.
* The server's [tickrate](https://developer.valvesoftware.com/wiki/Source_Multiplayer_Networking#Basic_networking).
* The server's performance-related [ConVar](https://developer.valvesoftware.com/wiki/ConVar) values.

If you want the absolute *best* performance and have the money, I personally would suggest looking for a server that contains a CPU from a list such as the highest-rated CPU benchmarks for single-threaded performance from [Passmark](https://www.cpubenchmark.net/singleThread.html).

## Table Of Contents
* [Concurrent Players](#concurrent-players)
* [GameMode, Amount Of Mods, & Types Of Maps](#game-mode-amount-of-mods--types-of-maps)
* [The Server's Tickrate](#the-servers-tickrate)
* [The Server's Performance Rate ConVar Values](#the-servers-performance-rate-convar-values)
* [Monitoring Server Performance](#monitoring-server-performance)
* [Dedicated Hosting VS LAN Server](#dedicated-hosting-vs-lan-server)
* [See Also](#see-also)
* [Conclusion](#conclusion)

## Concurrent Players
The more players that are connected to the server at once, the more network data that is exchanged between each client and typically the more that goes on in the game and server itself resulting in higher CPU usage.

While there isn't a general guideline or rule of thumb on single-threaded speeds and player counts because there are so many other variables, if you plan on having many players connected to the server at once, you should try acquiring a more modern CPU with higher single-threaded performance. 

## Game Mode, Amount Of Mods, & Types Of Maps
These are very important factors to consider when deciding what server requirements you need. For example, a server running a game mode like Deathmatch in Counter-Strike: Source where players are constantly shooting at each other, respawning, and running around is most likely going to consume more resources than a server running a game mode where when users die, they don't respawn until the next round.

With that said, close-quarter maps or maps that use a lot of [physical](https://developer.valvesoftware.com/wiki/Physics) [props](https://developer.valvesoftware.com/wiki/Prop_physics) are most likely going to consume more CPU power than a larger and more open map.

## The Server's Tickrate
The [tickrate](https://developer.valvesoftware.com/wiki/Source_Multiplayer_Networking#Basic_networking) is how often the server simulates the game. By default, a server's tickrate is set to **66.66...** (every *15ms*). This value can be changed using the `-tickrate` command-line argument in the server's startup command if the game or a mod allows server administrators to change it.

The higher the server's tickrate is, the more CPU power and bandwidth required. This also results in the server sending and receiving more updates to and from clients which improves client latency, hit registration, and more!

There are many servers in Garry's Mod that host up to **128 players connected at once**. They're able to achieve this because they run a low tickrate value such as *33* or *22* which is low enough for the CPU to handle the general server load in most cases (depending on the server, of course). 

## The Server's Performance Rate ConVar Values
There are a few [ConVars](https://developer.valvesoftware.com/wiki/ConVar) related to server performance you can modify on the server that impact how much CPU and bandwidth your server consumes. With that said, these settings can impact a client's latency, packet loss count, choke, and more.

Here is a list of some performance-related ConVars that I've found to be the most beneficial.

* `sv_maxrate` - The client's maximum bandwidth rate allowed (both incoming and outgoing) per second on the server in "bytes". Setting this to `0` indicates *unlimited* and recommended if you have the network and hardware capacity.
* `sv_minrate` - The client's minimum bandwidth rate allowed (both incoming and outgoing) per second on the server in "bytes".
* `sv_maxupdaterate` - The maximum amount of *update* packets per second the server can send to a client. This is typically equal to your server's tickrate value. However, in some cases can be lowered to take less load on the CPU or lower bandwidth usage.
* `sv_minupdaterate` - The minimum amount of *update* packets per second the server can send to the client. This is typically anywhere from `0` to half of your server's tickrate value (e.g. `33`).
* `sv_maxcmdrate` - The maximum amount of *command* packets per second the server can send to the client. This is typically equal to your server's tickrate value. However, in some cases can be lowered to take less load on the CPU or lower bandwidth usage.
* `sv_mincmdrate` - The minimum amount of *command* packets per second the server can send to the client. This is typically anywhere from `0` to half of your server's tickrate value (e.g. `33`).
* `net_splitpacket_maxrate` - The maximum amount of bytes per second the server can send split packets to the client. Setting this to a higher value such as `128000` can help in situations where choke occurs with a lot of network data being transmitted at once.
* `net_maxcleartime` - The maximum amount of time in seconds the server will spend processing and sending network data in a single *tick*. Setting this to a low value like `0.001` can reduce choke in certain situations, but will require more CPU power.
* `sv_parallel_sendsnapshot` - When `1`, will allow the server to use a **separate thread** to prepare game state snapshots for each client. This can highly improve server performance in high player environments.

I recommend reading about [Source Multiplayer Networking](https://developer.valvesoftware.com/wiki/Source_Multiplayer_Networking) to better understand what these values should be set to. I found [this](https://forums.nfoservers.com/viewtopic.php?f=25&t=3930) topic from [NFOservers.com](https://nfoservers.com) extremely informative and helpful as well!

If you are using a dedicated hosting provider, you most likely will have the network capacity to set these ConVar values to high values/unlimited. In this case, the following values should work in most cases.

```
sv_maxrate 0
sv_minrate 75000
sv_maxupdaterate 66
sv_minupdaterate 20
sv_maxcmdrate 66
sv_mincmdrate 20
net_splitpacket_maxrate 128000
net_maxcleartime 0.001
sv_parallel_sendsnapshot 1
```

If you are still unsure what values you should set these performance-related ConVars to, I recommend using a tool such as [this](https://ambaca.github.io/rate-calculator-2015/) to calculate the values. With that said, it is recommended you run a network speed test using tools such as [SpeedTest.net](https://speedtest.net) or [CloudFlare](https://cloudflare.com/) to retrieve your server's upload and download network speeds. These network speeds play a big role in how many players you can have connected to the server concurrently before running into network-related issues such as packet loss and so on (especially your **upload speed**, since this indicates the amount of bandwidth you can send to all players at once across the Internet).

## Monitoring Server Performance
The best in-game tool to use to monitor server performance in Source engine games is the built-in [Net Graph](https://developer.valvesoftware.com/wiki/Source_Multiplayer_Networking#Net_graph). You can display the net graph by adjusting your `net_graph` value in the Developer console from `0` to `4`.

This tool displays the amount of bandwidth you're sending and receiving from the server you're connected to, the server's FPS, the amount of updates you and the server are sending/receiving, and more!

A more in-depth explanation of Net Graph can be found [here](https://developer.valvesoftware.com/wiki/TF2_Network_Graph).

Here's some information from the previously linked Wiki page:

![Net Graph Image](https://developer.valvesoftware.com/w/images/thumb/e/e8/Net_graph_annotated2.jpg/640px-Net_graph_annotated2.jpg)

### Area 1
> This area is the legend for the colors used in the payload section of the graph. If a part of the payload arrives but doesn't fit into one of the predetermined buckets, then it is represented in the clear area between the last color and the little white dot the represents the full packet size (see indicator **a** in image)

### Area 2
> For packets **greater** than 300 bytes which are in the 95th percentile, the size of the packet is rendered in text at the top of the payload area (see marker 2). Note that the Orange Box tech uses compression on the packets, but the sizes reported in the `net_graph` payload are based on the decompressed payload size.

### Area 3
> The local connection's frames per second and round trip ping to the server are shown in area 3.

### Area 4
> This area shows the current bandwidth usage. The *in/out* show the size in bytes of the last incoming and outgoing packet. The *k/s* shows the kilobytes per second (rolling average) recently seen in each direction.

### Area 5
> This area shows the performance of the server the client is connected to. The `sv` tag shows the FPS of the server as of the latest networking update delivered to the client. The `var` shows the standard deviation of the server's frametime (where `server fps = 1.0 / frametime`) over the last 50 frames recorded by the server. If the server's framerate is below 20 fps, then this line will draw in yellow. If the server's framerate is below 10 fps, then this line will draw in red.

### Area 6
> The `lerp` indicator shows the number of msecs of interpolation being used by the client. Some notes on the value of lerp follow below.

### Area 7
> This area shows the user's current `cl_updaterate` setting, the actual number of updates per second actually received from the server, the actual number of packets per second sent to the server and the user's `cl_cmdrate` setting (which is the user's desired packets per second to send to the server).

### Area 8
> When `net_graphshowlatency` is 1, this area shows a historical view of the latency of the connection. The height (indicated by marker **d**) corresponds to `net_graphmsecs` time (actually there is a bit of headroom after `net_graphmsecs` at the top for the text fields to fit into). Red vertical lines indicate dropped packets from the server down to the client. If the graph shows a yellow marker (such as at marker **c**), this indicates that the server had to choke back one or more packets before sending the client an update.

### Area 9
> When `net_graphshowinterp` is 1, this area shows for each client frame how much interpolation was needed. If there is a large gap between packets (packet loss, server framerate too low, etc.), then the client will have insufficient data for interpolation and will start to extrapolate. The extrapolation is shown as orange bars rising up above the while line (a run of extrapolation can be seen just to the left of the 9 marker). In addition, the very bottom pixel indicates whether a `CUserCmd` (`usercmd`) packet was sent on that rendering frame, or held back by the client and aggregated due to the user's `cl_cmdrate` setting.

## Dedicated Hosting VS LAN Server
While you *can* host a public server on your home network (LAN), it is **not** recommended. Instead, you should purchase a server from a dedicated hosting provider.

While the subscription-based fee that often comes with dedicated hosting is often considered a big *con*, reasons to use dedicated hosting include:

* Better protection from [(D)DoS attacks](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/) due to higher network capacity and mitigation systems specifically designed to handle attacks.
* Better client routing and latency.
* More stable (e.g. if a power outage occurs at home, the server won't be taken offline).

Additionally, here are a couple reasons not to host your server at home.
* Some ISPs are against hosting a public game server on your home network.
* Higher home electric and network bandwidth costs depending on how many servers are ran, the server's load, the server's processor, etc.

## See Also
* [NFOservers](https://nfoservers.com) - [Valve games lag troubleshooting guide](https://forums.nfoservers.com/viewtopic.php?f=25&t=3930) 
* [Valve](https://www.valvesoftware.com/en/) - [Source Multiplayer Networking](https://developer.valvesoftware.com/wiki/Source_Multiplayer_Networking#Basic_networking)
* [Source Dedicated Server Rate Calculator 2015](https://ambaca.github.io/rate-calculator-2015/)
* [TF2 Net Graph](https://developer.valvesoftware.com/wiki/TF2_Network_Graph)

https://forum.moddingcommunity.com/t/how-to-download-run-steamcmd/190

## Conclusion
That concludes this guide/knowledgebase topic. By now, you should have a better understanding on what hardware and network you require when running a Source engine server.

If you have any questions or have feedback, please reply to this topic!