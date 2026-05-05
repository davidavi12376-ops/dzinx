-- ========================================
-- GRAVITY GUI - Travado Sem Lag
-- ========================================

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
    Name = "🌍 Gravity GUI - Travado",
    LoadingTitle = "Gravity Menu",
})

local Tab = Window:CreateTab("Gravidade", 4483362458)

local currentGravity = workspace.Gravity
local defaultGravity = workspace.Gravity
local antiReset = false
local connection = nil

-- Função para forçar gravidade
local function ForceGravity()
    workspace.Gravity = currentGravity
end

Tab:CreateSlider({
    Name = "Valor da Gravidade",
    Range = {0, 1000},
    Increment = 1,
    CurrentValue = currentGravity,
    Callback = function(Value)
        currentGravity = Value
        workspace.Gravity = Value
    end,
})

Tab:CreateToggle({
    Name = "🔒 Travar Gravidade (Anti-Reset)",
    CurrentValue = false,
    Callback = function(Value)
        antiReset = Value
        
        if Value then
            if connection then connection:Disconnect() end
            
            -- Método leve: verifica a cada 0.2s (quase sem lag)
            connection = game:GetService("RunService").Heartbeat:Connect(function()
                if antiReset then
                    if workspace.Gravity ~= currentGravity then
                        workspace.Gravity = currentGravity
                    end
                end
            end)
            
            Rayfield:Notify({
                Title = "Gravidade Travada",
                Content = "Agora não reseta mais!",
                Duration = 4,
            })
        else
            if connection then
                connection:Disconnect()
                connection = nil
            end
        end
    end,
})

-- Botões Rápidos
Tab:CreateButton({
    Name = "🌕 Gravidade Lua (50)",
    Callback = function()
        currentGravity = 50
        workspace.Gravity = 50
    end,
})

Tab:CreateButton({
    Name = "🌍 Normal (196.2)",
    Callback = function()
        currentGravity = 196.2
        workspace.Gravity = 196.2
    end,
})

Tab:CreateButton({
    Name = "🪐 Gravidade Alta (400)",
    Callback = function()
        currentGravity = 400
        workspace.Gravity = 400
    end,
})

Tab:CreateButton({
    Name = "Resetar Tudo",
    Callback = function()
        antiReset = false
        if connection then connection:Disconnect() connection = nil end
        currentGravity = defaultGravity
        workspace.Gravity = defaultGravity
    end,
})

Rayfield:Notify({
    Title = "Gravity Travado Carregado!",
    Content = "Use o toggle para cravar a gravidade",
    Duration = 5,
})
