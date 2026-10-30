# Green Chat Tags

Welcome to the setup guide for **Green Chat Tags**. This addon allows players to view their active chat tags using the `/tags` command, and automatically assigns a vibrant green tag to players who own a specific GamePass.

## Settings

Configure your tags, group ranks, and gamepasses in this module script.

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

return ChatTagSettings
