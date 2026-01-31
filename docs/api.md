# API

## Methods
### changePrefix
```luau
Flux:changePrefix(player: Player, prefix: string, color: Color3?): ()
```
Changes the Chat prefix of the specified player. This prefix is automatically filtered.  
`color` can be left empty. `color` will default to white in that case.
!!! info
    This method can only be implemented on the server.
#### Code Sample
This example checks the users `UserId` and applies a prefix dependent on it.  
```luau
local ownerId = 1                       -- UserId of the user that is supposed to receive that prefix
local prefix = "[OWNER]"                -- Desired Prefix to be applied to Player
local color = Color3.fromRGB(255, 0, 0) -- Color of the Prefix Text
game:GetService("Players").PlayerAdded:Connect(function(player)
    if player.UserId == ownerId then
        Flux:changePrefix(player, prefix, color)
    end
end)
```
---
### broadcastMessage
Replicates a message to `recipient` in form of a system message. Supports Rich Text formatting.
```luau
Flux:broadcastMessage(recipient: any?, message: string): ()
```
`recipient` can either be a `Team` instance, a `Player` instance or a table. If `recipient` is a table, that table will be iterated and every `Player` instance found will receive the broadcast.  
!!! info
    This method can only be implemented on the server.
#### Code Sample  
This example broadcasts a serverwide message to all Players in the server instance.
```luau
local players = game:GetService("Players"):GetPlayers()
local message = "This is a serverwide announcement!"

Flux:broadcastMessage(players, message)
```
!!! warning
    The content of the message is not automatically filtered. This Method should never contain any user generated content!

---
### broadcastGlobalMessage
Replicates a message to every Player across all active game servers of the same experience. Supports Rich Text formatting.
```luau
Flux:broadcastGlobalMessage(message: string): ()
```
!!! info
    This method can only be called form the server.
!!! warning
    The content of the message is not automatically filtered. This Method should never contain any user generated content!

---
### chatBanPlayer
Bans a player from chatting over the Flux Chat System. This includes QuickChatting.
```luau
Flux:chatBanPlayer(violator: any, reason: string?): ()
```
`violator` can either be an instance of `Player` or a `UserId` that belongs to the violator. If a reason isn't specified, `reason` will default to "`No reason specified`".
!!! info
    This method can only be called from the Server.  
    This method will kick the player to ensure the chat ban will be enforced immediately. Banned Players can still see other players' messages.
#### Code Sample
This example bans a random player from the game server.
```luau
local Players = game:GetService("Players"):GetPlayers()
local target = math.random(1,#Players)
Flux:chatBanPlayer(Players[target], "Unlucky guy")
```

---
### unChatBanPlayer
Unbans a player, enabling them to actively chat over the Flux Chat System again.
```luau
Flux:unChatBanPlayer(userId: number): ()
```
!!! info
    This method can only be called from the Server.

---
## Events
### OnIncomingMessage
Called when Flux is receiving a chat message.
```luau
Flux.OnIncomingMessage(): message: string, player: Player
```
Returns `message`, the messages content that was sent in form of a string and `player` will pass the Player Instance of the messages author.
!!! info
    This event can only be implemented on the client.
#### Code Sample
This example will check if the LocalPlayers' DisplayName has been mentioned in a chat message and will print to the output if so.
```luau
local LocalPlayer = game.Players.LocalPlayer

Flux.OnIncomingMessage:Connect(function(message, player)
    -- Prevents the output if the mention comes from the Player themselves
	if player ~= LocalPlayer then 
		message = string.lower(message)
		local LocalPlayerName = string.lower(LocalPlayer.DisplayName)
        -- checks if DisplayName is contained in sent message
		if string.find(LocalPlayerName, message) ~= nil then
			print(player.Name.." has mentioned your name.")
		end
	end
end)
```
---

## Configuration Properties
Read the [Get Started Section](installation.md#customizing-features) to figure out where to find these configuration properties.  
These properties can technically be changed at runtime but it is recommended to either set them beforehand or at the start of the game server.  
A dynamic change of some of these properties could lead to unexpected behaviour.

### Flux.Enabled
```luau
Flux.Enabled: boolean
```
Defines whether the Chat System will be enabled or not.  
---
### Flux.Debug
```luau
Flux.Debug: boolean
```
Enables debug messages for Flux.

---
### Flux.DisableRobloxChat
```luau
Flux.DisableRobloxChat: boolean
```
Disables the default Roblox Chat when set to true.

---
### Flux.EnableFiltering
```luau
Flux.EnableFiltering: boolean
```
Defines whether chat messages will be filtered or not.
!!! danger
    Disabling this could potentially get your game in trouble.

---
### Flux.EnableProjection
```luau
Flux.EnableProjection: boolean
```
Defines whether projection system will be used.  
The Projection system enables players to communicate with eachother, even though they wouldn't normally be able to due to account settings.  

For the Projection system to work, a projector in form of another player in the same game server is required. This projector needs the ability to chat and will automatically be selected when this feature is enabled.
``` mermaid
graph LR
  A[Player1] -->|Can't chat| B{Player2};
  A -->|Communicates inability to chat| C[Server];
  C -->|Finds Projector| D[Player3];
  D -->|Projects/Masks Message| B;
```
!!! danger
    Enabling this could potentially get your game in trouble.

---
### Flux.EnableUserChecking
```luau
Flux.EnableUserChecking: boolean
```
This ensures that chatting between users is permitted.
!!! danger
    Disabling this could potentially get your game in trouble.

---
### Flux.ChatBehaviour_ChatCache
```luau
Flux.ChatBehaviour_ChatCache: boolean
```
Ever wanted to read previous messages in the chat before you joined the game server? The Chat Cache system loads previous messages into the chat of newly joined players.
!!! danger
    Enabling this could potentially get your game in trouble.

---
### Flux.ChatBehaviour_MaximumMessageLength
```luau
Flux.ChatBehaviour_MaximumMessageLength: number
```
Defines how many characters a chat message can consist of.

---
### Flux.ChatBehaviour_MaximumMessages
```luau
Flux.ChatBehaviour_MaximumMessages: number
```
Defines how many messages will be kept in the Chat Window before Flux automatically starts deleting the oldest ones.

---
### Flux.ChatBehaviour_JoinMessage
```luau
Flux.ChatBehaviour_JoinMessage: boolean
```
Defines whether it is to be announced when a player joins the game.

---
### Flux.ChatBehaviour_LeaveMessage
```luau
Flux.ChatBehaviour_LeaveMessage: boolean
```
Defines whether it is to be announced when a player leaves the game.

---
### Flux.EnableUtilities
```luau
Flux.EnableUtilities: boolean
```
Defines whether Utilities shall be enabled or disabled.

---
### Flux.Utilities_QuickChat
```luau
Flux.Utilities_QuickChat: boolean
```
Defines if the QuickChat Utilitiy shall be enabled. This lets players communicate vaguely with eachother without violating ToS.

---
### Flux.Utilities_QuickChatMessages
```luau
Flux.Utilities_QuickChatMessages: table
```
A table that consists of strings that will be offered to the user to use as QuickChat option. This table can be expanded on at will and Flux will automatically add these options to the QuickChat Window.
!!! Warning
    The strings in these tables will not be filtered! If you as a developer break ToS by adding inappropiate messages, that's 100% on you!

---
### Flux.EnableAntiSpamming
```luau
Flux.EnableAntiSpamming: boolean
```
Defines whether to enable the anti spamming system. Disabling this fully is not recommended.

---
### Flux.AntiSpamming_MaxMessages
```luau
Flux.AntiSpamming_MaxMessages: number
```
This will define how many messages a player is able to send in a certain amount of time before he gets throttled.

---
### Flux.AntiSpamming_Cooldown
```luau
Flux.AntiSpamming_Cooldown: number
```
This will define how long (in seconds) a user has to wait before he can send a message again after being throttled by the Anti Spamming System.

---
### Flux.AntiSpamming_DetectSpammers
```luau
Flux.AntiSpamming_DetectSpammers: boolean
```
Currenlty unused configuration that will use a smart system to detect if a Player is purposely spamming the chat and throttling him in a more strict way if they are found to do so.