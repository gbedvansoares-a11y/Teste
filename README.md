-- Hub Simples - Noclip e Velocidade
-- GUI estilo Coquette Hub

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/bloodball/-back-ups-for-libs/main/fluxlib.txt"))()
local Window = Library:CreateWindow("Hub Simples")

-- ===== Painel Arrastável =====
local dragging = false
local dragInput, dragStart, startPos

Window.MainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = Window.MainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

Window.MainFrame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = input.Position - dragStart
        Window.MainFrame.Position = startPos + UDim2.new(0, delta.X, 0, delta.Y)
    end
end)

-- ===== Toggle Abrir/Fechar =====
Window:CreateToggle("Abrir/Fechar Hub", function(state)
    Window.MainFrame.Visible = state
end)

-- ===== Categorias =====
local MainTab = Window:CreateTab("Funções")
local Player = game.Players.LocalPlayer

-- ===== Função Velocidade =====
MainTab:CreateButton("Velocidade (WalkSpeed 100)", function()
    if Player.Character and Player.Character:FindFirstChild("Humanoid") then
        Player.Character.Humanoid.WalkSpeed = 100
    end
end)

-- ===== Função Noclip =====
MainTab:CreateButton("Noclip", function()
    if Player.Character then
        for _, part in pairs(Player.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

print("✅ Hub Simples carregado: Noclip + Velocidade")
