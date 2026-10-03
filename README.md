--// ==========================================
--// INVISIBLE HEAD FOR LOCAL PLAYER ONLY
--// ==========================================

local Players = game:GetService("Players")
local player = Players.LocalPlayer

local HEAD_SIZE = Vector3.new(0.1, 0.1, 0.1)
local NECK_HEIGHT = -0.35

local function hideOwnHead(character)
	local head = character:FindFirstChild("Head")
		or character:WaitForChild("Head", 10)

	local torso = character:FindFirstChild("UpperTorso")
		or character:FindFirstChild("Torso")

	if not head or not torso then
		return
	end

	-- Make the head very small
	head.Size = HEAD_SIZE

	-- Move the head inside the neck
	head.CFrame = torso.CFrame * CFrame.new(0, NECK_HEIGHT, 0)

	-- Make the head invisible
	head.Transparency = 1

	-- Hide face and other visual objects
	for _, object in ipairs(head:GetDescendants()) do
		if object:IsA("Decal")
			or object:IsA("Texture")
			or object:IsA("SpecialMesh") then

			object.Transparency = 1
		end
	end

	head.CanCollide = false
	head.CanTouch = false
	head.CanQuery = false
end

-- Apply to your character
if player.Character then
	hideOwnHead(player.Character)
end

-- Apply again after respawning
player.CharacterAdded:Connect(function(character)
	task.wait(0.2)
	hideOwnHead(character)
end)
