local ProximityPromptService = game:GetService("ProximityPromptService")

local ativo = false

-- GUI
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.ResetOnSpawn = false

local botao = Instance.new("TextButton", gui)
botao.Size = UDim2.new(0,120,0,50)
botao.Position = UDim2.new(0.4,0,0.8,0)
botao.Text = "Prompt: OFF"
botao.BackgroundColor3 = Color3.fromRGB(30,30,30)
botao.TextColor3 = Color3.new(1,1,1)

-- arrastar (mobile)
local dragging, dragInput, dragStart, startPos

botao.InputBegan:Connect(function(input)
    if input.UserInputType.Name == "Touch" then
        dragging = true
        dragStart = input.Position
        startPos = botao.Position
    end
end)

botao.InputEnded:Connect(function(input)
    if input.UserInputType.Name == "Touch" then
        dragging = false
    end
end)

botao.InputChanged:Connect(function(input)
    if dragging and input.UserInputType.Name == "Touch" then
        local delta = input.Position - dragStart
        botao.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end
end)

-- toggle
botao.MouseButton1Click:Connect(function()
    ativo = not ativo
    botao.Text = ativo and "Prompt: ON" or "Prompt: OFF"
end)

-- ativação
ProximityPromptService.PromptShown:Connect(function(prompt)
    if ativo and prompt then
        prompt.HoldDuration = 0
        fireproximityprompt(prompt)
    end
end)
