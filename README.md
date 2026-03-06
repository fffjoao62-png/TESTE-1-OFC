
local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/HeavenlyScripts/1nig1htmare1234-OrionLib-with-Black-CheckMarks/main/Orion.lua"))()

local Window = OrionLib:MakeWindow({
    Name = "│axon HUB│〄Emergency Hamburg",
    HidePremium = false,... (51 KB restante(s))

axon HUB(1) (1).txt
101 KB
Steve — 03/03/2026, 08:48
@everyone More Source codes????? 

local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/HeavenlyScripts/1nig1htmare1234-OrionLib-with-Black-CheckMarks/main/Orion.lua"))()

local Window = OrionLib:MakeWindow({
    Name = "│axon HUB│〄Emergency Hamburg",
    HidePremium = false,
    SaveConfig = true,
    ConfigFolder = "test",
    IntroEnabled = true,
    IntroText = "Trixo v13",
})

Tab1 = Window:MakeTab({
	Name = "Aimbot | 🔴",
	PremiumOnly = false
})

local Section = Tab1:AddSection({
	Name = "Aimbot-settings"
})



local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

local aimbotEnabled = false
local triggerbotEnabled = false
local aimbotFOV = 100

-- Funktionen
function getClosestTarget()
	local closest, shortest = nil, aimbotFOV
	for _, plr in pairs(Players:GetPlayers()) do
		if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("Head") and plr.Character:FindFirstChild("Humanoid") and plr.Character.Humanoid.Health > 0 then
			local screenPos, onScreen = Camera:WorldToViewportPoint(plr.Character.Head.Position)
			if onScreen then
				local dist = (Vector2.new(UIS:GetMouseLocation().X, UIS:GetMouseLocation().Y) - Vector2.new(screenPos.X, screenPos.Y)).Magnitude
				if dist < shortest then
					closest = plr
					shortest = dist
				end
			end
		end
	end
	return closest
end

RunService.RenderStepped:Connect(function()
	if aimbotEnabled and UIS:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then
		local target = getClosestTarget()
		if target and target.Character and target.Character:FindFirstChild("Head") then
			Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Character.Head.Position)
		end
	end

	if triggerbotEnabled and UIS:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then
		local mouse = LocalPlayer:GetMouse()
		local target = mouse.Target
		if target and target.Parent and target.Parent:FindFirstChild("Humanoid") then
			mouse1click() -- Muss vom Exploit unterstützt werden!
		end
	end
end)

-- Rayfield UI
Tab1:AddToggle({
	Name = "🔴 Aimbot (Right mouse button)",
	CurrentValue = false,
	Flag = "AimbotToggle",
	Callback = function(v) aimbotEnabled = v end,
})


Tab1:AddSlider({
	Name = "🔴 Aimbot-Stärke",
	Min = 20,
	Max = 300,
	Default = 10,
	Color = Color3.fromRGB(255,255,255),
	Increment = 10,
	ValueName = "Stärke",
	Callback = function(v) aimbotFOV = v end,   
})


local showFOV = false
local fovRadius = 100
local fovCircle = Drawing.new("Circle")
fovCircle.Visible = false
fovCircle.Thickness = 2
fovCircle.NumSides = 64
fovCircle.Color = Color3.fromRGB(0, 255, 255)
fovCircle.Transparency = 0.5
fovCircle.Filled = false

Tab1:AddToggle({
	Name = "🔴 Show FOV Circle",
	CurrentValue = false,
	Callback = function(val)
		showFOV = val
		fovCircle.Visible = val
	end,
})


Tab1:AddSlider({
	Name = "🔴 FOV-Radius",
	Min = 50,
	Max = 300,
	Default = 10,
	Color = Color3.fromRGB(255,255,255),
	Increment = 10,
	ValueName = "FOV",
	Callback = function(v)
		fovRadius = v
		fovCircle.Radius = v
	end,  
})


RunService.RenderStepped:Connect(function()
	if showFOV then
		local mouse = UIS:GetMouseLocation()
		fovCircle.Position = Vector2.new(mouse.X, mouse.Y + 36) -- +36 für GUI-Versatz
	end
end)


Tab1:AddToggle({
	Name = "🔴 Triggerbot",
	CurrentValue = false,
	Flag = "TriggerToggle",
	Callback = function(v) triggerbotEnabled = v end,
})

-- FOV-Kreis für Aimbot
local fovCircle = Drawing.new("Circle")
fovCircle.Visible = false
fovCircle.Thickness = 2
fovCircle.NumSides = 64
fovCircle.Color = Color3.fromRGB(0, 255, 255)
fovCircle.Transparency = 0.5
fovCircle.Filled = false

-- 📦 Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- ⚙️ Einstellungen
local aimbotEnabled = false
local aimbotMenuVisible = false -- Menü sichtbar oder nicht
local aimbotFOV = 150
local smoothness = 0.15

-- 🔍 Ziel
local lockedTarget = nil
local allTargets = {}

-- 📱 GUI erstellen
local ScreenGui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
ScreenGui.ResetOnSpawn = false

-- Aim-Button
local AimButton = Instance.new("TextButton", ScreenGui)
AimButton.Size = UDim2.new(0, 80, 0, 80)
AimButton.Position = UDim2.new(0.5, 0, 0.8, 0)
AimButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
AimButton.TextColor3 = Color3.fromRGB(255, 255, 255)
AimButton.Text = "🔴"
AimButton.TextScaled = true
AimButton.BorderSizePixel = 0
AimButton.AnchorPoint = Vector2.new(0.5, 0.5)
AimButton.Active = true
AimButton.Draggable = true
AimButton.Visible = false

-- FOV-Kreis
local FOVCircle = Drawing.new("Circle")
FOVCircle.Visible = false
FOVCircle.Color = Color3.fromRGB(0, 128, 0)
FOVCircle.Thickness = 2
FOVCircle.NumSides = 64
FOVCircle.Radius = aimbotFOV
FOVCircle.Filled = false

-- 🔍 Nächsten Spieler finden
local function getClosestTarget()
	local closest, shortest = nil, aimbotFOV
	allTargets = {}
	for _, plr in pairs(Players:GetPlayers()) do
		if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("Head") and plr.Character:FindFirstChild("Humanoid") then
			if plr.Character.Humanoid.Health > 0 then
				table.insert(allTargets, plr)
				local head = plr.Character.Head
				local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)
				if onScreen then
					local screenCenter = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
					local dist = (screenCenter - Vector2.new(screenPos.X, screenPos.Y)).Magnitude
					if dist < shortest then
						closest = plr
						shortest = dist
					end
				end
			end
		end
	end
	return closest
end

-- 🎯 Kamera nachziehen
RunService.RenderStepped:Connect(function()
	if aimbotEnabled then
		local screenCenter = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
		FOVCircle.Visible = true
		FOVCircle.Position = screenCenter

		if not lockedTarget or not lockedTarget.Character or not lockedTarget.Character:FindFirstChild("Humanoid") or lockedTarget.Character.Humanoid.Health <= 0 then
			lockedTarget = getClosestTarget()
		else
			local head = lockedTarget.Character.Head
			local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)
			local dist = (screenCenter - Vector2.new(screenPos.X, screenPos.Y)).Magnitude
			if dist > aimbotFOV then
				lockedTarget = getClosestTarget()
			end
		end

		if lockedTarget and lockedTarget.Character and lockedTarget.Character:FindFirstChild("Head") then
			local head = lockedTarget.Character.Head
			local targetCF = CFrame.new(Camera.CFrame.Position, head.Position)
			Camera.CFrame = Camera.CFrame:Lerp(targetCF, smoothness)
		end
	else
		FOVCircle.Visible = false
	end
end)

-- 🔘 Aim-Button → Aimbot toggeln
AimButton.MouseButton1Click:Connect(function()
	aimbotEnabled = not aimbotEnabled
	if aimbotEnabled then
		lockedTarget = getClosestTarget()
		AimButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
	else
		lockedTarget = nil
		AimButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
	end
end)

Tab1:AddToggle({
	Name = "🔴 Mobile Aimbot Menu",
	StartingState = false,
	Callback = function(state)
		aimbotMenuVisible = state
		AimButton.Visible = state
	end
})
local Section = Tab1:AddSection({
	Name = "🔴 Weapon Settings"
})

local aimFOVEnabled = false
Tab1:AddToggle({
    Name = "🔴 Aim FOV",
    CurrentValue = false,
    Flag = "AimFOV",
    Callback = function(v)
        aimFOVEnabled = v

        if v then
            OrionLib:MakeNotification({ Title = "Aim FOV", Content = "Aktiviert!", Duration = 2 })
            task.spawn(function()
                while aimFOVEnabled do
                    local tool = plr.Character and plr.Character:FindFirstChildOfClass("Tool")
                    if tool then
                        tool:SetAttribute("AimFieldOfView", 70)
                    end
                    task.wait(0.1)
                end
            end)
        else
            OrionLib:MakeNotification({ Title = "Aim FOV", Content = "Deaktiviert!", Duration = 2 })
        end
    end
})


-- 🧠 Auto Reload Logik
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local VirtualInputManager = game:GetService("VirtualInputManager")

local autoRefillEnabled = false

-- Liste deiner Waffen
local trackedWeapons = {
	"G36",
	"Glock 17",
	"MP5",
	"M4 Carabine",
	"Sniper",
	"M58B Shotgun"
}

-- 🔄 Auto-Check Loop
task.spawn(function()
	while true do
		if autoRefillEnabled then
			pcall(function()
				local char = LocalPlayer.Character
				if char then
					for _, weaponName in ipairs(trackedWeapons) do
						local weapon = char:FindFirstChild(weaponName) or workspace:FindFirstChild(weaponName)
						if weapon then
							local magSize = weapon:GetAttribute("MagCurrentSize") 
								or weapon:GetAttribute("Ammo") 
								or weapon:GetAttribute("Clip")
								or (weapon:FindFirstChild("Ammo") and weapon.Ammo.Value)

							if magSize and magSize == 0 then
								VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.R, false, game)
								task.wait(0.1)
								VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.R, false, game)
								task.wait(1)
							end
						end
					end
				end
			end)
		end
		task.wait(0.5)
	end
end)

-- ✅ Rayfield Toggle
Tab1:AddToggle({
	Name = "🔴 Auto-Reload",
	CurrentValue = false,
	Flag = "AutoReload",
	Callback = function(Value)
		autoRefillEnabled = Value
	end
})

-- 🎯 Toggle Crosshair Feature
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local crosshairActive = false

Tab1:AddToggle({
	Name = "🔴 Small-Crosshair",
	CurrentValue = false,
	Flag = "SmallCrosshair",
	Callback = function(Value)
		crosshairActive = Value

		if crosshairActive then
			-- ⏳ Aktiviert: Prüfe Tool und passe Attribut an
			task.spawn(function()
				while crosshairActive do
					local tool = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Tool")
					if tool then
						tool:SetAttribute("CrosshairSize", 1)
					end
					task.wait(0.1)
				end
			end)
		else
			-- ❌ Deaktiviert: Kein weiteres Verhalten nötig
		end
	end
})

-- ⚡ Rapid Fire Toggle
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local rapidFireEnabled = false

Tab1:AddToggle({
	Name = "🔴 Rapid-Fire",
	CurrentValue = false,
	Flag = "RapidFireToggle",
	Callback = function(Value)
		rapidFireEnabled = Value

		if rapidFireEnabled then
			-- Aktiviere Rapid Fire durch ständiges Setzen der Werte
			task.spawn(function()
				while rapidFireEnabled do
					local Tool = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Tool")
					if Tool then
						Tool:SetAttribute("ShootDelay", 0)
						Tool:SetAttribute("Automatic", true)
					end
					task.wait(0.1)
				end
			end)
		end
	end
})

-- 🎯 Toggle für No Recoil
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local noRecoilEnabled = false

Tab1:AddToggle({
	Name = "🔴 No-Recoil",
	CurrentValue = false,
	Flag = "NoRecoilToggle",
	Callback = function(Value)
		noRecoilEnabled = Value

		if noRecoilEnabled then
			task.spawn(function()
				while noRecoilEnabled do
					local Tool = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Tool")
					if Tool then
						Tool:SetAttribute("Recoil", 0)
						Tool:SetAttribute("Instability", 0)
					end
					task.wait(0.1)
				end
			end)
		end
	end
})

Tab2 = Window:MakeTab({
	Name = "Silent-aim | 🔫",
	PremiumOnly = false
})


Tab2:AddSection({
	Name = "Silent-Aim-Settings"
})


local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- remote safe resolve (timeout)
local function resolveRemote()
	local ok, folder = pcall(function() return ReplicatedStorage:WaitForChild("MW5", 5) end)
	if not ok or not folder then return nil end
	local ok2, remote = pcall(function() return folder:WaitForChild("7ac19887-c231-417a-a139-c150778da985", 5) end)
	if not ok2 or not remote then return nil end
	return remote
end

local GUN_REMOTE = resolveRemote()
if not GUN_REMOTE then
	-- notify but we continue; the toggle will check remote before firing
	Rayfield:Notify({Title = "Warning", Content = "Gun remote not found (8WX/75721fbe...). Silent aim will try to resolve on enable.", Duration = 5})
end

-- Config (defaults)
local FOV_RADIUS = 185
local PREDICTION = 0.33
local SHOOT_DELAY = 0.022
local SILENT_AIM = false

-- Teams: true = hittet, false = ignoriert!
local allowedTeams = {
	["Criminals"] = true,
	["Wanted"] = true,
	["Police"] = true,    -- set true if you want police targeted
	["Fire Department"] = false,
	["Busfahrer"] = false,
	["Civil"] = false,
	["LKW Fahrer"] = false,
}


Tab2:AddSlider({
	Name = "🟤 Silent Aim FOV",
	Min = 50,
	Max = 300,
	Default = 1,
	Color = Color3.fromRGB(255,255,255),
	Increment = 1,
	ValueName = "FOV",
	CurrentValue = FOV_RADIUS,
	Callback = function(val) FOV_RADIUS = val end   
})


Tab2:AddSlider({
	Name = "🟤 Prediction Power",
	Min = 0.10,
	Max = 0.45,
	Default = 0.001,
	Color = Color3.fromRGB(255,255,255),
	Increment = 0.001,
	ValueName = "",
	CurrentValue = PREDICTION,
	Callback = function(val) PREDICTION = val end    
})


Tab2:AddSlider({
	Name = "🟤 Shoot Speed (s)",
	Min = 0.012,
	Max = 0.10,
	Default = 0.001,
	Color = Color3.fromRGB(255,255,255),
	Increment = 0.001,
	ValueName = "s",
	CurrentValue = SHOOT_DELAY,
	Callback = function(val) SHOOT_DELAY = val end   
})

Tab2:AddToggle({
	Name = "🟤 Silent Aim",
	CurrentValue = false,
	Flag = "SilentAim",
	Callback = function(value)
		SILENT_AIM = value
		if value then
			-- try resolve remote again when enabling
			if not GUN_REMOTE then
				GUN_REMOTE = resolveRemote()
				if not GUN_REMOTE then
					OrionLib:MakeNotification({Title = "Silent Aim", Content = "Gun remote not found. Aborting.", Duration = 4})
					SILENT_AIM = false
					toggle:Set(false)
					return
				end
			end
			OrionLib:MakeNotification({Title = "Silent Aim", Content = "Enabled", Duration = 2})
			spawn(function()
				-- run loop in background
				local function inAllowedTeam(plr)
					if not plr.Team then return false end
					local tname = tostring(plr.Team)
					if allowedTeams[tname] ~= nil then
						return allowedTeams[tname]
					end
					return false
				end

				while SILENT_AIM do
					-- ensure character present
					local char = LocalPlayer.Character
					local myHRP = char and char:FindFirstChild("HumanoidRootPart")
					local gun = char and char:FindFirstChildOfClass("Tool")
					-- if no gun, wait
					if not myHRP or not gun then
						task.wait(0.15)
						continue
					end

					-- find target
					local closest, minDist = nil, FOV_RADIUS
					for _, plr in ipairs(Players:GetPlayers()) do
						if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
							local hum = plr.Character:FindFirstChildOfClass("Humanoid")
							local hrp = plr.Character:FindFirstChild("HumanoidRootPart")
							if hum and hum.Health > 0 and hrp then
								local teamOK = inAllowedTeam(plr) or hrp:GetAttribute("IsWanted")
								if teamOK then
									local pos, onScreen = Camera:WorldToViewportPoint(hrp.Position)
									local screenPos = Vector2.new(pos.X, pos.Y)
									local center = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
									local dist = (screenPos - center).Magnitude
									if onScreen and dist < minDist then
										closest = plr
										minDist = dist
									end
								end
							end
						end
					end

					if closest and GUN_REMOTE then
						-- compute predicted position and direction
						local targetHRP = closest.Character and closest.Character:FindFirstChild("HumanoidRootPart")
						if targetHRP and targetHRP:IsA("BasePart") and myHRP and myHRP:IsA("BasePart") then
							local predPos = targetHRP.Position + (targetHRP.Velocity * PREDICTION)
							local dir = (predPos - myHRP.Position)
							if dir.Magnitude > 0 then
								dir = dir.Unit
								-- call remote safely
								pcall(function()
									if GUN_REMOTE and GUN_REMOTE.FireServer then
										GUN_REMOTE:FireServer(gun, predPos, dir)
									end
								end)
							end
						end
						task.wait(SHOOT_DELAY)
					else
						task.wait(0.06)
					end
				end
			end)
		else
			OrionLib:MakeNotification({Title = "Silent Aim", Content = "Disabled", Duration = 2})
		end
	end
})

-- Drawing FOV circle (optional)
local hasDrawing, Drawing = pcall(function() return Drawing end)
local circle
if hasDrawing and Drawing then
	circle = Drawing.new("Circle")
	circle.Color = Color3.fromRGB(255,190,50)
	circle.Radius = FOV_RADIUS
	circle.Thickness = 2
	circle.Filled = false
	circle.Transparency = 0.78
	circle.Visible = false

	RunService.RenderStepped:Connect(function()
		if circle then
			circle.Position = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
			circle.Radius = FOV_RADIUS
			circle.Visible = SILENT_AIM
		end
	end)
end

-- Clean up on unload (optional)
local function cleanup()
	SILENT_AIM = false
	if circle then
		pcall(function() circle:Remove() end)
	end
end

Tab3 = Window:MakeTab({
	Name = "ESP | 👁️",
	PremiumOnly = false
})

Tab3:AddSection({
	Name = "Visuals-Settings"
})

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local plr = Players.LocalPlayer
local Camera = Workspace.CurrentCamera

local state = {
    espEnabled = false,
    showNames = true,
    showTeams = true,
    showDistance = true,
    showHealth = true,
    showEquipped = true,
    showWanted = true,
    espObjects = {},
    espDistance = 1000
}

-- 🟦 Teamfarbe bestimmen
local function teamColorForTeam(team)
    if not team then return Color3.fromRGB(200,200,200) end
    local tn = tostring(team.Name):lower()
    if tn:find("police") then
        return Color3.fromRGB(80,160,255)
    elseif tn:find("fire") then
        return Color3.fromRGB(255,80,80)
    elseif tn:find("hospital") or tn:find("medic") then
        return Color3.fromRGB(120,255,140)
    else
        return Color3.fromRGB(200,200,200)
    end
end

-- 🧱 ESP erstellen
local function createESPForPlayer(other)
    if not other.Character then return end
    if state.espObjects[other.UserId] then return end

    local hrp = other.Character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end

    local billboard = Instance.new("BillboardGui")
    billboard.Name = "ESP_" .. other.Name
    billboard.Adornee = hrp
    billboard.Size = UDim2.new(0,160,0,100)
    billboard.StudsOffset = Vector3.new(0,3.5,0)
    billboard.AlwaysOnTop = true
    billboard.Parent = other.Character

    local function newLabel(yOffset, color)
        local lbl = Instance.new("TextLabel", billboard)
        lbl.Size = UDim2.new(1,0,0,18)
        lbl.Position = UDim2.new(0,0,0,yOffset)
        lbl.BackgroundTransparency = 1
        lbl.Font = Enum.Font.GothamBold
        lbl.TextSize = 13
        lbl.TextStrokeTransparency = 0.6
        lbl.TextColor3 = color or Color3.new(1,1,1)
        return lbl
    end

    local nameLbl = newLabel(0)
    local infoLbl = newLabel(20)
    local healthLbl = newLabel(38, Color3.fromRGB(120,255,120))
    local equipLbl = newLabel(56, Color3.fromRGB(255,255,0))
    local wantedLbl = newLabel(74)

    state.espObjects[other.UserId] = {
        billboard = billboard,
        nameLbl = nameLbl,
        infoLbl = infoLbl,
        healthLbl = healthLbl,
        equipLbl = equipLbl,
        wantedLbl = wantedLbl
    }
end

-- 🔁 ESP updaten
local function updateESPEntry(other)
    local entry = state.espObjects[other.UserId]
    if not entry then return end

    local char = other.Character
    if not char then return end

    local hrp = char:FindFirstChild("HumanoidRootPart")
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not hrp or not hum or hum.Health <= 0 then
        entry.billboard.Enabled = false
        return
    end

    local plrRoot = plr.Character and plr.Character:FindFirstChild("HumanoidRootPart")
    if not plrRoot then return end

    local dist = (plrRoot.Position - hrp.Position).Magnitude
    if dist > state.espDistance then
        entry.billboard.Enabled = false
        return
    else
        entry.billboard.Enabled = true
    end

    local team = other.Team
    local teamColor = teamColorForTeam(team)

    entry.nameLbl.Text = state.showNames and other.Name or ""
    entry.nameLbl.TextColor3 = teamColor

    if state.showDistance then
        entry.infoLbl.Text = (team and ("["..team.Name.."] ") or "") .. math.floor(dist) .. "m"
    else
        entry.infoLbl.Text = team and ("["..team.Name.."]") or ""
    end

    entry.healthLbl.Text = state.showHealth and ("HP: "..math.floor(hum.Health)) or ""

    if state.showEquipped then
        local tool = char:FindFirstChildOfClass("Tool")
        entry.equipLbl.Text = tool and ("Equipped: "..tool.Name) or "Nothing Equipped"
    else
        entry.equipLbl.Text = ""
    end

    if state.showWanted then
        if hrp:GetAttribute("IsWanted") then
            entry.wantedLbl.Text = "Wanted"
            entry.wantedLbl.TextColor3 = Color3.fromRGB(255,140,0)
        else
            entry.wantedLbl.Text = "Not Wanted"
            entry.wantedLbl.TextColor3 = Color3.fromRGB(0,255,0)
        end
    else
        entry.wantedLbl.Text = ""
    end
end

-- 🌀 Render Loop
RunService.RenderStepped:Connect(function()
    if not state.espEnabled then return end
    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= plr then
            if not state.espObjects[p.UserId] then
                createESPForPlayer(p)
            end
            updateESPEntry(p)
        end
    end
end)

-- 🔔 Wenn neuer Spieler joint
Players.PlayerAdded:Connect(function(p)
    p.CharacterAdded:Connect(function()
        if state.espEnabled then
            task.wait(1)
            createESPForPlayer(p)
        end
    end)
end)

-- ❌ Wenn Spieler geht → ESP entfernen
Players.PlayerRemoving:Connect(function(p)
    local entry = state.espObjects[p.UserId]
    if entry then
        if entry.billboard then entry.billboard:Destroy() end
        state.espObjects[p.UserId] = nil
    end
end)

-- ⚙️ Rayfield UI Controls
Tab3:AddToggle({
    Name="⚪ Player ESP",
    CurrentValue=false,
    Callback=function(v)
        state.espEnabled = v
        if not v then
            for _,e in pairs(state.espObjects) do
                e.billboard:Destroy()
            end
            state.espObjects = {}
        else
            for _,p in ipairs(Players:GetPlayers()) do
                if p ~= plr then
                    createESPForPlayer(p)
                end
            end
        end
    end
})

Tab3:AddToggle({Name="⚪ Show Wanted",CurrentValue=true,Callback=function(v)state.showWanted=v end})
Tab3:AddToggle({Name="⚪ Show Names",CurrentValue=true,Callback=function(v)state.showNames=v end})
Tab3:AddToggle({Name="⚪ Show Teams",CurrentValue=true,Callback=function(v)state.showTeams=v end})
Tab3:AddToggle({Name="⚪ Show Distance",CurrentValue=true,Callback=function(v)state.showDistance=v end})
Tab3:AddToggle({Name="⚪ Show Health",CurrentValue=true,Callback=function(v)state.showHealth=v end})
Tab3:AddToggle({Name="⚪ Show Equipped",CurrentValue=true,Callback=function(v)state.showEquipped=v end})


Tab3:AddSlider({
	Name = "⚪ ESP-Distance",
	Min = 100,
	Max = 2000,
	Default = 50,
	Color = Color3.fromRGB(255,255,255),
	Increment = 50,
	ValueName = "Studs",
    CurrentValue = state.espDistance,
    Callback = function(value)
        state.espDistance = value
    end  
})


Tab4 = Window:MakeTab({
	Name = "Teleport | 🌀",
	PremiumOnly = false
})

Tab4:AddSection({
	Name = "Teleport-Options"
})

local teleportActive = false
local mouse = LocalPlayer:GetMouse()

Tab4:AddToggle({
	Name = "🔵 left-click teleport",
	CurrentValue = false,
	Callback = function(v) teleportActive = v end,
})

mouse.Button1Down:Connect(function()
	if teleportActive and mouse.Hit then
		LocalPlayer.Character:MoveTo(mouse.Hit.Position + Vector3.new(0, 3, 0))
	end
end)

RunService.Stepped:Connect(function()
	if spinOn and LocalPlayer.Character then
		local hrp = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
		if hrp then
			hrp.CFrame = hrp.CFrame * CFrame.Angles(0, math.rad(15), 0)
		end
	end
end)


-- 📦 Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer
local VehiclesFolder = workspace:WaitForChild("Vehicles")

-- 🔁 Utils
local function notify(txt)
    OrionLib:MakeNotification({ Title = "Teleport", Content = txt, Duration = 3 })
end

-- 🎯 Hilfsfunktion: Tween Move
local function tweenTo(destination)
    local car = VehiclesFolder:FindFirstChild(LocalPlayer.Name)
    if not car then return notify("Kein Fahrzeug gespawnt!") end

    if not car.PrimaryPart then
        car.PrimaryPart = car:FindFirstChild("DriveSeat", true) or car:FindFirstChildWhichIsA("BasePart")
    end
    if not car.PrimaryPart then return notify("Kein PrimaryPart gefunden!") end

    if typeof(destination) == "CFrame" then
        destination = destination.Position
    end

    local startPosition = car.PrimaryPart.Position
    local steps = { startPosition + Vector3.new(0, -5, 0), destination + Vector3.new(0, -5, 0), destination }

    for _, targetPos in ipairs(steps) do
        local distance = (car.PrimaryPart.Position - targetPos).Magnitude
        local duration = distance / 175
        local tweenInfo = TweenInfo.new(duration, Enum.EasingStyle.Linear, Enum.EasingDirection.Out)

        local value = Instance.new("CFrameValue")
        value.Value = car:GetPivot()

        value.Changed:Connect(function(newCFrame)
            car:PivotTo(newCFrame)
            if car.PrimaryPart then
                car.PrimaryPart.AssemblyLinearVelocity = Vector3.zero
                car.PrimaryPart.AssemblyAngularVelocity = Vector3.zero
            end
        end)

        local tween = TweenService:Create(value, tweenInfo, { Value = CFrame.new(targetPos) })
        tween:Play()
        tween.Completed:Wait()
        value:Destroy()
    end
end

-- 📍 Locations
local function getLocationCFrame(name)
    if name == "Bank" then
        return CFrame.new(-1174.68, 5.87, 3209.03)
    end
    return nil
end

-- 📍 Teleport: Nearest Dealer
local function teleportToNearestDealer()
    local car = VehiclesFolder:FindFirstChild(LocalPlayer.Name)
    if not car then return notify("Spawn zuerst dein Auto!") end

    if not car.PrimaryPart then
        car.PrimaryPart = car:FindFirstChild("DriveSeat", true) or car:FindFirstChildWhichIsA("BasePart")
    end

    local closest, dist = nil, math.huge
    for _, dealer in pairs(workspace:WaitForChild("Dealers"):GetChildren()) do
        if dealer:IsA("Model") and dealer.PrimaryPart then
            local d = (LocalPlayer.Character.PrimaryPart.Position - dealer.PrimaryPart.Position).Magnitude
            if d < dist then
                dist = d
                closest = dealer
            end
        end
    end

    if not closest then return notify("Kein Dealer gefunden!") end

    local dealerPos = closest.PrimaryPart.Position
    local dealerCFrame = closest.PrimaryPart.CFrame
    local tpCFrame = CFrame.new(dealerPos - dealerCFrame.LookVector * -10, dealerPos)

    tweenTo(tpCFrame)
   OrionLib:MakeNotification("Zum nächsten Dealer teleportiert!")
end


Tab4:AddButton({
    Name = "🔵 nearest Dealer",
    Callback = function()
        teleportToNearestDealer()
    end
})


Tab4:AddSection({ Name = "🔵 Work Places" })

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer
local VehiclesFolder = workspace:WaitForChild("Vehicles")

local Locations = {
    ["🔵 Police Station"] = CFrame.new(-1658.55, 5.619, 2735.71),
    ["🔵 Fire Station"] = CFrame.new(-963.32, 5.865, 3895.37),
    ["🔵 Bus Company"] = CFrame.new(-1695.80, 5.882, -1274.29),
    ["🔵 Truck Company"] = CFrame.new(652.55, 5.638, 1510.85),
}

local function tweenTo(destination)
    local car = VehiclesFolder:FindFirstChild(LocalPlayer.Name)
    if not car then return end

    car.PrimaryPart = car:FindFirstChild("DriveSeat", true)
    if not car.PrimaryPart then return end

    car.DriveSeat:Sit(LocalPlayer.Character:WaitForChild("Humanoid"))

    if typeof(destination) == "CFrame" then
        destination = destination.Position
    end

    local start = car.PrimaryPart.Position
    local steps = { start + Vector3.new(0,-5,0), destination + Vector3.new(0,-5,0), destination }

    for _, target in ipairs(steps) do
        local dist = (car.PrimaryPart.Position - target).Magnitude
        local duration = dist / 175
        local tweenInfo = TweenInfo.new(duration, Enum.EasingStyle.Linear)

        local value = Instance.new("CFrameValue")
        value.Value = car:GetPivot()

        value.Changed:Connect(function(newCFrame)
            car:PivotTo(newCFrame)
            car.DriveSeat.AssemblyLinearVelocity = Vector3.zero
            car.DriveSeat.AssemblyAngularVelocity = Vector3.zero
        end)

        local tween = TweenService:Create(value, tweenInfo, { Value = CFrame.new(target) })
        tween:Play()
        tween.Completed:Wait()
        value:Destroy()
    end
end

local locationNames = {}
for name,_ in pairs(Locations) do table.insert(locationNames,name) end

Tab4:AddDropdown({
    Name = "🔵 Work places",
    Options = locationNames,
    CurrentOption = locationNames[1],
    Flag = "TP_Work",
    Callback = function(option) end
})

Tab4:AddButton({
    Name = "🔵 Teleport",
    Callback = function()
        local choice = OrionLib.Flags["TP_Work"].Value
        if choice and Locations[choice] then
            tweenTo(Locations[choice])
        end
    end
})

----------------------------------------------------------
-- Robbable Places
Tab4:AddSection({ Name = "🔵 Robbable Places" })

local RobbableLocations = {
    ["🔵 Bank"] = CFrame.new(-1174.68, 5.87, 3209.03),
    ["🔵 Yellow Container"] = CFrame.new(1178.71, 28.696, 2321.66),
    ["🔵 Green Container"] = CFrame.new(1182.71, 28.696, 2158.84),
    ["🔵 Jewelry"] = CFrame.new(-346.63, 5.87, 3572.74),
    ["🔵 Ares Fuel"] = CFrame.new(-870.86, 5.622, 1505.16),
    ["🔵 Gas n Go Fuel"] = CFrame.new(-1544.4, 5.619, 3802.16),
    ["🔵 Ossu Fuel"] = CFrame.new(-27.55, 5.622, -754.6),
    ["🔵 Night Club"] = CFrame.new(-1844.95, 5.872, 3211.08),
    ["🔵 Tool Shop"] = CFrame.new(-717.23, 5.654, 729.08),
    ["🔵 Food Shop"] = CFrame.new(-911.50, 5.371, -1169.20),
    ["🔵 Clothing Store"] = CFrame.new(479.05, 3.158, -1452.59)
}

local robNames = {}
for name,_ in pairs(RobbableLocations) do table.insert(robNames,name) end

Tab4:AddDropdown({
    Name = "🔵 Robbable places",
    Options = robNames,
    CurrentOption = robNames[1],
    Flag = "TP_Rob",
    Callback = function(option) end
})

Tab4:AddButton({
    Name = "🔵 Teleport (Robbable)",
    Callback = function()
        local choice = OrionLib.Flags["TP_Rob"].Value
        if choice and RobbableLocations[choice] then
            tweenTo(RobbableLocations[choice])
        end
    end
})

----------------------------------------------------------
-- Usable Places
Tab4:AddSection({ Name = "🔵 Usable Places" })

local UsableLocations = {
    ["🔵 Tuning Garage"] = CFrame.new(-1429.04, 5.57, 143.96),
    ["🔵 Car Dealership"] = CFrame.new(-1454.02, 5.615, 940.83),
    ["🔵 Hospital"] = CFrame.new(-293.16, 5.627, 1053.98),
    ["🔵 Prison"] = CFrame.new(-514.34, 5.615, 2795.94),
}

local useNames = {}
for name,_ in pairs(UsableLocations) do table.insert(useNames,name) end

Tab4:AddDropdown({
    Name = "🔵 Usable places",
    Options = useNames,
    CurrentOption = useNames[1],
    Flag = "TP_Use",
    Callback = function(option) end
})

Tab4:AddButton({
    Name = "🔵 Teleport (Usable)",
    Callback = function()
        local choice = OrionLib.Flags["TP_Use"].Value
        if choice and UsableLocations[choice] then
            tweenTo(UsableLocations[choice])
        end
    end
})




Tab4:AddSection({
	Name = "🔵 Player-Teleport"
})


-- 🔧 Services
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local Workspace = game:GetService("Workspace")
local LocalPlayer = Players.LocalPlayer
local VehiclesFolder = Workspace:WaitForChild("Vehicles")

-- 🧭 Utils
local function notify(title, text, dur)
    OrionLib:MakeNotification({ Title = title, Content = text, Duration = dur or 3 })
end

-- 🔹 Spieler-Liste
local selectedPlayer = nil
local function getPlayerList()
    local list = {}
    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= LocalPlayer then
            table.insert(list, p.Name)
        end
    end
    if #list == 0 then
        table.insert(list, "Keine Spieler gefunden")
    end
    return list
end

-- 🔽 Spieler-Auswahl (Bugfix)
Tab4:AddDropdown({
    Name = "🔵 Select target player",
    Options = getPlayerList(),
    CurrentOption = "",
    Flag = "TargetPlayer",
    Callback = function(Value)
        if typeof(Value) == "table" then
            Value = Value[1]
        end

        if not Value or Value == "" or Value == "Keine Spieler gefunden" then
            selectedPlayer = nil
            OrionLib:MakeNotification("Hinweis", "Kein gültiger Spieler ausgewählt!", 2)
            return
        end

        local found = Players:FindFirstChild(Value)
        if found then
            selectedPlayer = found
            OrionLib:MakeNotification("Ziel gesetzt", "Zielspieler: " .. found.Name, 2)
        else
            selectedPlayer = nil
            OrionLib:MakeNotification("Fehler", "Spieler '" .. tostring(Value) .. "' wurde nicht gefunden!", 3)
        end
    end
})


-- 🚗 Teleportfunktion
local function TweenCarToPlayer(targetPlayer)
    local car = VehiclesFolder:FindFirstChild(LocalPlayer.Name)
    if not car then return notify("Fehler", "Kein Fahrzeug gefunden!") end

    local driveSeat = car:FindFirstChild("DriveSeat", true)
    if not driveSeat then return notify("Fehler", "Kein DriveSeat gefunden!") end
    car.PrimaryPart = driveSeat

    -- Spieler ins Auto setzen
    local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local hum = char:FindFirstChildOfClass("Humanoid")
    if hum and hum.SeatPart ~= driveSeat then
        pcall(function() driveSeat:Sit(hum) end)
    end

    -- Zielvalidierung
    if not (targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart")) then
        return notify("Fehler", "Zielspieler ist nicht verfügbar oder nicht geladen.")
    end

    local targetPos = targetPlayer.Character.HumanoidRootPart.Position + Vector3.new(0, 3, 0)
    local distance = (car.PrimaryPart.Position - targetPos).Magnitude
    local duration = math.clamp(distance / 120, 1, 8)

    local cfValue = Instance.new("CFrameValue")
    cfValue.Value = car:GetPivot()

    local connection
    connection = cfValue.Changed:Connect(function(cframe)
        if car and car.Parent then
            car:PivotTo(cframe)
            pcall(function()
                driveSeat.AssemblyLinearVelocity = Vector3.zero
                driveSeat.AssemblyAngularVelocity = Vector3.zero
            end)
        end
    end)

    local tween = TweenService:Create(cfValue, TweenInfo.new(duration, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {
        Value = CFrame.new(targetPos)
    })

    tween:Play()
    tween.Completed:Connect(function()
        connection:Disconnect()
        cfValue:Destroy()
        OrionLib:MakeNotification("Fertig!", "Du bist jetzt bei " .. targetPlayer.Name, 3)
    end)
end

-- 🚀 Teleport-Button
Tab4:AddButton({
    Name = "🔵 Teleport to player",
    Callback = function()
        if selectedPlayer then
            TweenCarToPlayer(selectedPlayer)
        else
            OrionLib:MakeNotification("Fehler", "Bitte wähle zuerst einen Zielspieler aus!")
        end
    end
})

-- 🛰 Auto-Tracking-Toggle
local trackingEnabled = false
Tab4:AddToggle({
    Name = "🔵 Follow automatically to player",
    CurrentValue = false,
    Flag = "AutoTrack",
    Callback = function(v)
        trackingEnabled = v
        if v then
            OrionLib:MakeNotification("Aktiviert", "Automatisches Folgen gestartet.", 2)
            task.spawn(function()
                while trackingEnabled and selectedPlayer and selectedPlayer.Character do
                    TweenCarToPlayer(selectedPlayer)
                    task.wait(3)
                end
            end)
        else
            OrionLib:MakeNotification("Deaktiviert", "Automatisches Folgen beendet.", 2)
        end
    end
})



Tab5 = Window:MakeTab({
	Name = "Player | ⚙️",
	PremiumOnly = false
})

Tab5:AddSection({
	Name = "Misc-Options"
})

-- 📦 Services
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local RunService = game:GetService("RunService")


-- 📊 Variablen
local autoReviveEnabled = false
local healthConnection = nil

-- 🔁 Tween-Funktion
local function tweenTo(destination)
	local VehiclesFolder = Workspace:FindFirstChild("Vehicles")
	local car = VehiclesFolder and VehiclesFolder:FindFirstChild(player.Name)
	if not car then
		OrionLib:MakeNotification({Title="Fehler", Content="Kein Fahrzeug gefunden!", Duration=3})
		return false
	end

	car.PrimaryPart = car:FindFirstChild("DriveSeat", true)
	if car.DriveSeat and player.Character and player.Character:FindFirstChild("Humanoid") then
		car.DriveSeat:Sit(player.Character.Humanoid)
	end

	if typeof(destination) == "CFrame" then
		destination = destination.Position
	end

	local function moveTo(targetPosition)
		local distance = (car.PrimaryPart.Position - targetPosition).Magnitude
		local tweenDuration = distance / 175
		local tweenInfo = TweenInfo.new(tweenDuration, Enum.EasingStyle.Linear, Enum.EasingDirection.Out)

		local value = Instance.new("CFrameValue")
		value.Value = car:GetPivot()

		value.Changed:Connect(function(newCFrame)
			car:PivotTo(newCFrame)
			if car.DriveSeat then
				car.DriveSeat.AssemblyLinearVelocity = Vector3.zero
				car.DriveSeat.AssemblyAngularVelocity = Vector3.zero
			end
		end)

		local tween = TweenService:Create(value, tweenInfo, { Value = CFrame.new(targetPosition) })
		tween:Play()
		tween.Completed:Wait()
		value:Destroy()
	end

	moveTo(car.PrimaryPart.Position + Vector3.new(0, -4, 0))
	moveTo(destination + Vector3.new(0, -4, 0))
	moveTo(destination)
	return true
end

-- 🏥 Auto-Heilung
local function autoHealAndReturn(originalPosition)
	local char = player.Character or player.CharacterAdded:Wait()
	local humanoid = char:WaitForChild("Humanoid")

	local bed = Workspace:FindFirstChild("Buildings")
		and Workspace.Buildings:FindFirstChild("Hospital")
		and Workspace.Buildings.Hospital:FindFirstChild("HospitalBed")
		and Workspace.Buildings.Hospital.HospitalBed:FindFirstChild("Seat")

	if not bed then
		OrionLib:MakeNotification({Title="Self-Revive Fehler", Content="HospitalBed nicht gefunden!", Duration=4})
		return false
	end

	if humanoid.Sit then
		humanoid.Sit = false
		humanoid.Jump = true
		task.wait(0.1)
	end

	if char:FindFirstChild("HumanoidRootPart") then
		local hrp = char.HumanoidRootPart
		hrp.CFrame = bed.CFrame * CFrame.new(0, 3, 0)
		task.wait(0.2)
		hrp.AssemblyLinearVelocity = Vector3.zero
		hrp.AssemblyAngularVelocity = Vector3.zero
		hrp.CFrame = bed.CFrame * CFrame.new(0, 0.5, 0)
		task.wait(0.1)
	end

	local attempts = 0
	while not humanoid.Sit and attempts < 5 do
		bed:Sit(humanoid)
		attempts += 1
		task.wait(0.2)
	end

	repeat task.wait(0.2) until humanoid.Health >= humanoid.MaxHealth * 0.27

	humanoid.Sit = false
	humanoid.Jump = true
	task.wait(0.2)

	local car = Workspace.Vehicles:FindFirstChild(player.Name)
	if car and char:FindFirstChild("HumanoidRootPart") then
		char.HumanoidRootPart.CFrame = car.DriveSeat.CFrame * CFrame.new(0, 2, 0)
		car.DriveSeat:Sit(humanoid)
		task.wait(0.1)
		tweenTo(originalPosition)
	end
end

-- 💉 Hauptprüfung
local function checkHealthAndTeleport()
	local car = Workspace:FindFirstChild("Vehicles") and Workspace.Vehicles:FindFirstChild(player.Name)
	if not car then return end

	local char = player.Character or player.CharacterAdded:Wait()
	local humanoid = char:WaitForChild("Humanoid")
	local originalPos = car:GetPivot().Position
	local hospital = CFrame.new(-120.30, 5.61, 1077.29)

	if humanoid.Health <= humanoid.MaxHealth * 0.27 then
		if tweenTo(hospital) then
			task.wait(2)
			autoHealAndReturn(originalPos)
		end
	else
		OrionLib:MakeNotification({
			Title="Self-Revive",
			Content="You are not dead – no action necessary",
			Duration=3
		})
	end
end

-- ⚙️ Aktivierungslogik
local function enableAutoRevive(val)
	autoReviveEnabled = val
	if val then
		local char = player.Character or player.CharacterAdded:Wait()
		local humanoid = char:WaitForChild("Humanoid")
		healthConnection = humanoid.HealthChanged:Connect(function(hp)
			if autoReviveEnabled and hp <= humanoid.MaxHealth * 0.27 then
				checkHealthAndTeleport()
			end
		end)
		OrionLib:MakeNotification({Title="Self-Revive activated", Content="Automatisches Heilen aktiv!", Duration=3})
	else
		if healthConnection then
			healthConnection:Disconnect()
			healthConnection = nil
		end
		OrionLib:MakeNotification({Title="Self-Revive disabled", Content="Auto-Heal ausgeschaltet.", Duration=3})
	end
end

-- 🔄 Respawn-Reattach
player.CharacterAdded:Connect(function(char)
	if autoReviveEnabled then
		char:WaitForChild("Humanoid").HealthChanged:Connect(function(hp)
			if hp <= char.Humanoid.MaxHealth * 0.27 then
				checkHealthAndTeleport()
			end
		end)
	end
end)


Tab5:AddButton({
	Name = "⚫ Self Revive",
	Callback = function()
		checkHealthAndTeleport()
	end
})



Tab5:AddButton({
    Name = "⚫ Change-Job",
    Callback = function()
        local player = game.Players.LocalPlayer
        if player and player.Team then
            local teams = game:GetService("Teams")
            local newTeam = teams:GetChildren()[math.random(1, #teams:GetChildren())]
            player.Team = newTeam
        end
    end,
})

local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = game.Players.LocalPlayer

local flying = false
local camCFrame = Camera.CFrame
local speed = 2
local sens = 0.2
local pitch, yaw = 0, 0
local velocity = Vector3.zero

local function toggleFreeCam(state)
	flying = state
	if flying then
		Camera.CameraType = Enum.CameraType.Scriptable
		camCFrame = Camera.CFrame
		local _, y = camCFrame:ToEulerAnglesYXZ()
		yaw = y
		pitch = 0
	else
		Camera.CameraType = Enum.CameraType.Custom
	end
end

RunService.RenderStepped:Connect(function()
	if not flying then return end

	local delta = UserInputService:GetMouseDelta()
yaw = yaw - delta.X * sens * 0.01
pitch = math.clamp(pitch - delta.Y * sens * 0.01, -math.pi/2, math.pi/2)

local rot = CFrame.Angles(0, yaw, 0) * CFrame.Angles(pitch, 0, 0)
local move = Vector3.zero

if UserInputService:IsKeyDown(Enum.KeyCode.W) then move = move + Vector3.new(0, 0, -1) end
if UserInputService:IsKeyDown(Enum.KeyCode.S) then move = move + Vector3.new(0, 0, 1) end
if UserInputService:IsKeyDown(Enum.KeyCode.A) then move = move + Vector3.new(-1, 0, 0) end
if UserInputService:IsKeyDown(Enum.KeyCode.D) then move = move + Vector3.new(1, 0, 0) end
if UserInputService:IsKeyDown(Enum.KeyCode.E) then move = move + Vector3.new(0, 1, 0) end
if UserInputService:IsKeyDown(Enum.KeyCode.Q) then move = move + Vector3.new(0, -1, 0) end

	local isFast = UserInputService:IsKeyDown(Enum.KeyCode.LeftShift)
	local moveSpeed = isFast and speed * 3 or speed

	-- Bewegung nur bei Taste, keine Restbewegung = kein Wackeln
	velocity = move.Magnitude > 0 and move.Unit * moveSpeed or Vector3.zero
	camCFrame = CFrame.new(camCFrame.Position + rot:VectorToWorldSpace(velocity)) * rot
	Camera.CFrame = camCFrame
end)

-- 📦 Rayfield Toggle (einbauen in dein VisualsTab oder ähnliches) Character-Settings
Tab5:AddToggle({
	Name = "⚫ FreeCam",
	CurrentValue = false,
	Callback = function(state)
		toggleFreeCam(state)
	end
})

speed = 2 -- Geschwindigkeit der FreeCam
Tab5:AddSlider({
	Name = "⚫ FreeCam Speed",
	Min = 1,
	Max = 10,
	Default = 0.1,
	Color = Color3.fromRGB(255,255,255),
	Increment = 1,
	ValueName = "Geschwindigkeit",
	CurrentValue = speed,
    Callback = function(value)
        speed = value
    end,  
})


local noclip = false
Tab5:AddToggle({
	Name = "⚫ Noclip",
	CurrentValue = false,
	Callback = function(state)
		noclip = state
	end,
})

RunService.Stepped:Connect(function()
	if noclip and LocalPlayer.Character then
		for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
			if part:IsA("BasePart") then
				part.CanCollide = false
			end
		end
	end
end)


local fpsText = Drawing.new("Text")
fpsText.Size = 30
fpsText.Position = Vector2.new(10, 10)
fpsText.Color = Color3.fromRGB(0, 255, 255)
fpsText.Outline = true
fpsText.Visible = true
local lastTime, fps = tick(), 0

RunService.RenderStepped:Connect(function()
	local now = tick()
	fps = math.floor(1 / (now - lastTime))
	lastTime = now
	fpsText.Text = "FPS: " .. tostring(fps)
end)

Tab5:AddSection({
	Name = "Character-Settings"
})

Tab5:AddParagraph("Fly INFO","Press V to Fly")

-- 📦 Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer

-- 🔧 Fly Variablen
local flyingSpeed = 50
local isFlying = false
local attachment, alignPosition, alignOrientation

-- ✈️ Fly Funktionen
local function enableFly()
    local character = player.Character
    if not character then return end
    local root = character:FindFirstChild("HumanoidRootPart")
    local humanoid = character:FindFirstChild("Humanoid")
    if not (root and humanoid) then return end

    attachment = Instance.new("Attachment", root)

    alignPosition = Instance.new("AlignPosition")
    alignPosition.Attachment0 = attachment
    alignPosition.Mode = Enum.PositionAlignmentMode.OneAttachment
    alignPosition.MaxForce = 5000
    alignPosition.Responsiveness = 45
    alignPosition.Parent = root

    alignOrientation = Instance.new("AlignOrientation")
    alignOrientation.Attachment0 = attachment
    alignOrientation.Mode = Enum.OrientationAlignmentMode.OneAttachment
    alignOrientation.MaxTorque = 5000
    alignOrientation.Responsiveness = 45
    alignOrientation.Parent = root

    humanoid.PlatformStand = true
    isFlying = true

    local lastPosition = root.Position
    alignPosition.Position = lastPosition

    task.spawn(function()
        while isFlying and root and humanoid do
            local moveDir = Vector3.zero
            local camCFrame = workspace.CurrentCamera.CFrame

            if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDir += camCFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDir -= camCFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDir -= camCFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDir += camCFrame.RightVector end

            if moveDir.Magnitude > 0 then
                moveDir = moveDir.Unit
                local newPos = lastPosition + (moveDir * flyingSpeed * RunService.Heartbeat:Wait())
                alignPosition.Position = newPos
                lastPosition = newPos
            end

            alignOrientation.CFrame = CFrame.new(Vector3.zero, c... (51 KB restante(s))
axon HUB(1) (1).txt
101 KB
