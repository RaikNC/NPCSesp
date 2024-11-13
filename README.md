-- Cria um botão na tela
local ScreenGui = Instance.new("ScreenGui")
local Button = Instance.new("TextButton")

ScreenGui.Parent = game.CoreGui
Button.Parent = ScreenGui

Button.Size = UDim2.new(0, 50, 0, 50)
Button.Position = UDim2.new(0, 10, 0, 10)
Button.Text = "⭕"
Button.BackgroundColor3 = Color3.new(1, 1, 1)
Button.TextColor3 = Color3.new(0, 0, 0)
Button.Font = Enum.Font.SourceSans
Button.TextSize = 24
Button.BorderSizePixel = 0
Button.BackgroundTransparency = 0.5
Button.TextWrapped = true

local espEnabled = false

-- Função para criar ESP
local function createESP(npc)
    local espBox = Instance.new("BoxHandleAdornment")
    espBox.Size = npc:GetExtentsSize()
    espBox.Adornee = npc
    espBox.Color3 = Color3.new(1, 0, 0)
    espBox.AlwaysOnTop = true
    espBox.ZIndex = 5
    espBox.Transparency = 0.5
    espBox.Parent = npc
end

-- Função para ativar/desativar ESP
local function toggleESP()
    espEnabled = not espEnabled
    Button.Text = espEnabled and "❌" or "⭕"
    
    local function findNPCs(parent)
        for _, obj in pairs(parent:GetChildren()) do
            if obj:IsA("Model") and obj:FindFirstChild("Humanoid") and not game.Players:GetPlayerFromCharacter(obj) then
                if espEnabled then
                    createESP(obj)
                else
                    for _, adornment in pairs(obj:GetChildren()) do
                        if adornment:IsA("BoxHandleAdornment") then
                            adornment:Destroy()
                        end
                    end
                end
            end
            findNPCs(obj)
        end
    end

    findNPCs(game)
end

Button.MouseButton1Click:Connect(toggleESP)
