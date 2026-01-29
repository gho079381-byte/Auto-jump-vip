local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local player = Players.LocalPlayer
local cam = workspace.CurrentCamera

pcall(function()
Lighting.GlobalShadows = false
Lighting.FogEnd = 1e9
Lighting.Brightness = 1
Lighting.EnvironmentDiffuseScale = 0
Lighting.EnvironmentSpecularScale = 0
Lighting.ClockTime = 14

pcall(function()  
	Lighting.Bloom.Enabled = false  
	Lighting.SunRays.Enabled = false  
	Lighting.Blur.Enabled = false  
	Lighting.ColorCorrection.Enabled = false  
end)  

for _,v in ipairs(workspace:GetDescendants()) do  
	if v:IsA("BasePart") then  
		v.Material = Enum.Material.Plastic  
		v.CastShadow = false  
		v.Reflectance = 0  
	elseif v:IsA("Decal") or v:IsA("Texture") then  
		v:Destroy()  
	elseif v:IsA("ParticleEmitter")  
	or v:IsA("Trail")  
	or v:IsA("Beam")  
	or v:IsA("Fire")  
	or v:IsA("Smoke") then  
		v.Enabled = false  
	elseif v:IsA("SurfaceAppearance") then  
		v:Destroy()  
	end  
end  

settings().Rendering.QualityLevel = Enum.QualityLevel.Level01  
pcall(function()  
	settings().Rendering.MeshPartDetailLevel = Enum.MeshPartDetailLevel.Level01  
end)

end)

local autoJump = false
local autoBounce = false

local JUMP_DELAY = -99999999999999999999999999929292929292929282928927282728638363736373637367363837387382728272827827382782728282827282781618252825825327262852727392619529252952929162925295338639282929929292929299292939292929288283538382528161962937284252261518156142725272526582726282426527242
local SPEED = 60

local humanoid, root

local function hookChar(char)
humanoid = char:WaitForChild("Humanoid")
root = char:WaitForChild("HumanoidRootPart")
end

if player.Character then hookChar(player.Character) end
player.CharacterAdded:Connect(function(c)
task.wait(0.3)
hookChar(c)
end)

local gui = Instance.new("ScreenGui", player.PlayerGui)
gui.ResetOnSpawn = false

local function makeBtn(text, y)
local b = Instance.new("TextButton", gui)
b.Size = UDim2.fromOffset(150, 38)
b.Position = UDim2.fromScale(0.05, y)
b.BackgroundColor3 = Color3.fromRGB(20,20,20)
b.TextColor3 = Color3.fromRGB(255,255,255)
b.TextSize = 12
b.Text = text
b.Active = true
b.Draggable = true
return b
end

local jumpBtn = makeBtn("AUTO JUMP : OFF", 0.65)
local bounceBtn = makeBtn("AUTO BOUNCE : OFF", 0.72)

jumpBtn.MouseButton1Click:Connect(function()
autoJump = not autoJump
jumpBtn.Text = autoJump and "AUTO JUMP : ON" or "AUTO JUMP : OFF"
end)

bounceBtn.MouseButton1Click:Connect(function()
autoBounce = not autoBounce
bounceBtn.Text = autoBounce and "AUTO BOUNCE : ON" or "AUTO BOUNCE : OFF"
end)

RunService.Heartbeat:Connect(function()
if not humanoid or not root then return end

if autoJump and humanoid.FloorMaterial ~= Enum.Material.Air then  
	humanoid:ChangeState(Enum.HumanoidStateType.Jumping)  
end

if autoBounce then
local look = cam.CFrame.LookVector
root.AssemblyLinearVelocity = Vector3.new(
look.X * SPEED,
root.AssemblyLinearVelocity.Y,
look.Z * SPEED
)
end
end)
