# Green Chat Tags

Welcome to the setup guide for **Green Chat Tags**. This addon allows players to view their active chat tags using the `/tags` command, and automatically assigns a vibrant green tag to players who own a specific GamePass.

## Server Script

Place this script in `ServerScriptService` (or wherever your addons are loaded).

```lua
local MarketplaceService = game:GetService("MarketplaceService")

local GamepassTag_server = {}
GamepassTag_server.Name = "GamepassTag_server"

-- Replace with your Roblox GamePass ID
local GAMEPASS_ID = 00000000 

function GamepassTag_server.Init(api)
	print("Initializing GamepassTag_server...")

	-- Register a dynamic green tag rule safely via AddonAPI
	api:RegisterCustomMatcher({
		Title = "VIP Member",
		TagColor = Color3.fromRGB(0, 255, 128) -- Vibrant Green Tag
	}, function(player)
		local success, hasPass = pcall(function()
			return MarketplaceService:UserOwnsGamePassAsync(player.UserId, GAMEPASS_ID)
		end)
		
		return success and hasPass
	end)
end

function GamepassTag_server.Cleanup()
	print("GamepassTag_server cleaned up.")
end

return GamepassTag_server
