local plr = game.Players.LocalPlayer

local function getHRP()
    local char = plr.Character or plr.CharacterAdded:Wait()
    return char:WaitForChild("HumanoidRootPart")
end

local function flyTo(pos, speed)
    local hrp = getHRP()
    local bv = Instance.new("BodyVelocity")
    bv.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    bv.Velocity = Vector3.zero
    bv.Parent = hrp

    while (hrp.Position - pos).Magnitude > 5 do
        local dir = (pos - hrp.Position).Unit
        bv.Velocity = dir * speed
        task.wait()
    end

    bv:Destroy()
    hrp.CFrame = CFrame.new(pos)
end

-- GUI
local gui = Instance.new("ScreenGui", plr:WaitForChild("PlayerGui"))
gui.Name = "EzzHub"
gui.ResetOnSpawn = false

local menu = Instance.new("Frame", gui)
menu.Size = UDim2.new(0, 200, 0, 200) -- aumentei altura para caber o botão extra
menu.Position = UDim2.new(0.3, 0, 0.3, 0)
menu.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
menu.Active, menu.Draggable = true, true

local title = Instance.new("TextLabel", menu)
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
title.Text = "EzzHub"
title.TextColor3 = Color3.new(1, 1, 1)
title.TextScaled = true

local toggle = Instance.new("TextButton", menu)
toggle.Size = UDim2.new(1, -20, 0, 40)
toggle.Position = UDim2.new(0, 10, 0, 50)
toggle.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
toggle.Text = "Auto Farm Gold: OFF"
toggle.TextColor3 = Color3.new(1, 1, 1)
toggle.TextScaled = true

-- Botão de copiar Discord
local copyBtn = Instance.new("TextButton", menu)
copyBtn.Size = UDim2.new(1, -20, 0, 40)
copyBtn.Position = UDim2.new(0, 10, 0, 100)
copyBtn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
copyBtn.Text = "Copiar Discord"
copyBtn.TextColor3 = Color3.new(1, 1, 1)
copyBtn.TextScaled = true

copyBtn.MouseButton1Click:Connect(function()
    setclipboard("https://discord.gg/amvTM9rX6y")
    copyBtn.Text = "Copiado!"
    task.wait(2)
    copyBtn.Text = "Copiar Discord"
end)

local openBtn = Instance.new("ImageButton", gui)
openBtn.Size = UDim2.new(0, 50, 0, 50)
openBtn.Position = UDim2.new(0, 10, 0, 10)
openBtn.Image = "rbxassetid://133393242441626"
openBtn.Active, openBtn.Draggable = true, true

menu.Visible = true
openBtn.MouseButton1Click:Connect(function()
    menu.Visible = not menu.Visible
end)

local farming = false
toggle.MouseButton1Click:Connect(function()
    farming = not farming
    toggle.Text = "Auto Farm Gold: " .. (farming and "ON" or "OFF")
    
    if farming then
        task.spawn(function()
            while farming do
                local hrp = getHRP()
                hrp.Anchored = false
                -- Sequência de farm
                flyTo(Vector3.new(-38, 7, -196), 350)
                flyTo(Vector3.new(-47, 71, 8515), 350)
                flyTo(Vector3.new(-55, -359, 9489), 350)
                task.wait(17)
            end
        end)
    end
end)
