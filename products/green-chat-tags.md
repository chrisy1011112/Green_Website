# Green Chat Tags

Welcome to the setup guide for **Green Chat Tags**. This addon allows players to view their active chat tags using the `/tags` command, and automatically assigns a vibrant green tag to players who own a specific GamePass.

##Settings
```lua
local ChatTagSettings = {}

-- GLOBAL SETTINGS
ChatTagSettings.AllowMultipleTags = true -- If true, shows all matching tags; if false, shows only the top match

-- FALLBACK DEFAULT TAG
ChatTagSettings.UseDefaultTag = true
ChatTagSettings.DefaultTag = {
	Title = "Member",
	TagColor = Color3.fromRGB(180, 180, 180),
	TextColor = Color3.fromRGB(255, 255, 255)
}

-- TAG DEFINITIONS TABLE
-- Tags are evaluated from TOP to BOTTOM when AllowMultipleTags is false.
ChatTagSettings.TagRules = {
	-- 1. SPECIFIC USER ID MATCHES
	{
		Type = "UserId",
		TargetId = 12345678, -- Replace with your UserId
		Title = "Owner",
		TagColor = Color3.fromRGB(255, 60, 60),
		TextColor = Color3.fromRGB(255, 220, 220)
	},
	{
		Type = "UserId",
		TargetId = 87654321,
		Title = "Dev",
		TagColor = Color3.fromRGB(0, 200, 255),
		TextColor = Color3.fromRGB(255, 255, 255)
	},

	-- 2. GROUP RANK MATCHES (Requires GroupId and Minimum Rank 1-255)
	{
		Type = "GroupRank",
		GroupId = 1234567, -- Replace with your Roblox Group ID
		MinRank = 250,      -- Minimum rank required (255 = Owner, 250 = Admin, etc.)
		Title = "Admin",
		TagColor = Color3.fromRGB(255, 170, 0),
		TextColor = Color3.fromRGB(255, 255, 255)
	},

	-- 3. GAMEPASS OWNERSHIP MATCHES
	{
		Type = "GamePass",
		GamePassId = 987654321, -- Replace with your GamePass ID
		Title = "VIP",
		TagColor = Color3.fromRGB(255, 215, 0),
		TextColor = Color3.fromRGB(255, 245, 200)
	}
}

return ChatTagSettings```

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
