local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local toolName = "Machado" -- Nome da ferramenta de corte
local intervalo = 1 -- Tempo entre ações

function encontrarArvores()
    local arvores = {}
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("Model") and obj.Name == "Arvore" then
            table.insert(arvores, obj)
        end
    end
    return arvores
end

function equiparTool()
    local backpack = player:WaitForChild("Backpack")
    local tool = backpack:FindFirstChild(toolName)
    if tool then
        tool.Parent = character
    end
end

function cortarArvore(arvore)
    if not arvore then return end
    local tronco = arvore:FindFirstChild("Tronco") or arvore:FindFirstChildWhichIsA("Part")
    if tronco then
        character:MoveTo(tronco.Position + Vector3.new(0, 3, 0))
        wait(0.5)
        for i = 1, 5 do
            local tool = character:FindFirstChild(toolName)
            if tool and tool:FindFirstChild("Activate") then
                tool:Activate()
            end
            wait(0.5)
        end
    end
end

equiparTool()

while true do
    local arvores = encontrarArvores()
    for _, arvore in ipairs(arvores) do
        cortarArvore(arvore)
        wait(intervalo)
    end
    wait(2)
end
