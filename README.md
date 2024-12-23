-- Script para Super League Soccer no Roblox

-- Variáveis locais
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local ball = workspace:FindFirstChild("SoccerBall") -- Substitua "SoccerBall" pelo nome real da bola no jogo
local goal = workspace:FindFirstChild("Goal") -- Substitua "Goal" pelo nome real do gol no jogo
local UserInputService = game:GetService("UserInputService")
local UIS = game:GetService("StarterGui")

-- Criar UI
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "CLHHubSLS"

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 300, 0, 200)
frame.Position = UDim2.new(0.5, -150, 0.5, -100)
frame.Visible = false

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0.2, 0)
title.Position = UDim2.new(0, 0, 0, 0)
title.Text = "CLH Hub SLS"
title.TextSize = 24
title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
title.TextColor3 = Color3.fromRGB(255, 255, 255)

local buttonToggle = Instance.new("TextButton", screenGui)
buttonToggle.Size = UDim2.new(0, 100, 0, 50)
buttonToggle.Position = UDim2.new(0, 10, 0, 10)
buttonToggle.Text = "Abrir Painel"
buttonToggle.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
buttonToggle.TextColor3 = Color3.fromRGB(255, 255, 255)

buttonToggle.MouseButton1Click:Connect(function()
    frame.Visible = not frame.Visible
    buttonToggle.Text = frame.Visible and "Fechar Painel" or "Abrir Painel"
end)

-- Função única para teletransportar, dar carrinho e chutar a bola
local function teleportAndKick()
    if ball and goal then
        -- Teletransporta o jogador para a bola
        humanoidRootPart.CFrame = ball.CFrame + Vector3.new(0, 2, 0)

        -- Dá um carrinho na bola
        local direction = (ball.Position - humanoidRootPart.Position).Unit
        humanoidRootPart.Velocity = direction * 50

        -- Chuta a bola com mais força em direção ao gol
        local kickDirection = (goal.Position - ball.Position).Unit
        local ballBodyVelocity = Instance.new("BodyVelocity", ball)
        ballBodyVelocity.Velocity = kickDirection * 100 -- Ajuste a força do chute conforme necessário
        ballBodyVelocity.MaxForce = Vector3.new(1e6, 1e6, 1e6)
        game:GetService("Debris"):AddItem(ballBodyVelocity, 0.5) -- Remove o BodyVelocity após 0.5 segundos
    else
        warn("Bola ou gol não encontrados!")
    end
end

-- Ligações de teclas para acionar a função
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end

    if input.KeyCode == Enum.KeyCode.T then
        teleportAndKick() -- Executa a função ao pressionar T
    end
end)
