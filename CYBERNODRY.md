-- Limpar GUIs antigas para não dar erro
for _, v in pairs(game.Players.LocalPlayer:WaitForChild("PlayerGui"):GetChildren()) do
    if v.Name == "Xeno_Mega_V4" then v:Destroy() end
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "Xeno_Mega_V4"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 220, 0, 310)
MainFrame.Position = UDim2.new(0.5, -110, 0.5, -150)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -60, 0, 35)
Title.Text = " Legends Of Speed Script By CyberNoDry"
Title.TextColor3 = Color3.fromRGB(0, 255, 127)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = MainFrame

-- Lógica de Controle
local orbRunning, gemRunning, hoopRunning = false, false, false
local player = game.Players.LocalPlayer

-- Função para Criar Botões
local function createBtn(name, text, y, color, callback)
    local btn = Instance.new("TextButton")
    btn.Name = name
    btn.Size = UDim2.new(0.9, 0, 0, 35)
    btn.Position = UDim2.new(0.05, 0, 0, y)
    btn.Text = text
    btn.BackgroundColor3 = color
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.SourceSansBold
    btn.Parent = MainFrame
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    btn.MouseButton1Click:Connect(callback)
    return btn
end

-- Teleporte Principal
local function startCycle(targetNames, checkVar)
    while _G[checkVar] do
        local character = player.Character
        local root = character and character:FindFirstChild("HumanoidRootPart")
        
        if root then
            local targets = {}
            for _, v in pairs(game:GetDescendants()) do
                if v:IsA("BasePart") then
                    for _, name in pairs(targetNames) do
                        if v.Name:lower():find(name:lower()) then
                            table.insert(targets, v)
                        end
                    end
                end
            end

            for _, target in pairs(targets) do
                if not _G[checkVar] then break end
                if target.Parent then
                    local touched = false
                    local connection = target.Touched:Connect(function(hit)
                        if hit.Parent == character then touched = true end
                    end)

                    local t = 0
                    while _G[checkVar] and not touched and target.Parent and t < 30 do
                        root.CFrame = target.CFrame
                        task.wait(0.01)
                        t = t + 1
                    end
                    if connection then connection:Disconnect() end
                end
            end
        end
        task.wait(0.1)
    end
end

-- Botões de Ação
createBtn("OrbOn", "INICIAR ORB", 45, Color3.fromRGB(40, 45, 50), function() 
    _G.orbRunning = true startCycle({"outerOrb", "Red", "Blue", "Green", "Yellow"}, "orbRunning") 
end)
createBtn("OrbOff", "PARAR ORB", 85, Color3.fromRGB(80, 30, 30), function() _G.orbRunning = false end)

createBtn("GemOn", "INICIAR GEM", 130, Color3.fromRGB(60, 20, 90), function() 
    _G.gemRunning = true startCycle({"outerGem"}, "gemRunning") 
end)
createBtn("GemOff", "PARAR GEM", 170, Color3.fromRGB(80, 30, 30), function() _G.gemRunning = false end)

createBtn("HoopOn", "INICIAR HOOP", 215, Color3.fromRGB(100, 100, 20), function() 
    _G.hoopRunning = true startCycle({"Hoop"}, "hoopRunning") 
end)
createBtn("HoopOff", "PARAR HOOP", 255, Color3.fromRGB(80, 30, 30), function() _G.hoopRunning = false end)

-- Botões do Topo
local Close = Instance.new("TextButton", MainFrame)
Close.Size = UDim2.new(0, 25, 0, 25)
Close.Position = UDim2.new(1, -30, 0, 5)
Close.Text = "X"
Close.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
Close.TextColor3 = Color3.fromRGB(255, 255, 255)
Close.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

local Min = Instance.new("TextButton", MainFrame)
Min.Size = UDim2.new(0, 25, 0, 25)
Min.Position = UDim2.new(1, -60, 0, 5)
Min.Text = "-"
Min.BackgroundColor3 = Color3.fromRGB(60, 60, 65)
Min.TextColor3 = Color3.fromRGB(255, 255, 255)
Min.MouseButton1Click:Connect(function()
    MainFrame.ClipsDescendants = true
    if MainFrame.Size.Y.Offset > 40 then
        MainFrame:TweenSize(UDim2.new(0, 220, 0, 35), "Out", "Quart", 0.3, true)
    else
        MainFrame:TweenSize(UDim2.new(0, 220, 0, 310), "Out", "Quart", 0.3, true)
    end
end)
