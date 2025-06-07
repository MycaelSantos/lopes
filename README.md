

-- Configuração
local npcName = "Bandit" -- Nome do inimigo
local arma = "Combat" -- Nome da arma ou fruta (ex: "Combat", "Katana", "Light-Light")

-- Função para equipar arma/fruta
function equiparArma(armaNome)
    local player = game.Players.LocalPlayer
    for _, item in pairs(player.Backpack:GetChildren()) do
        if item.Name == armaNome then
            player.Character.Humanoid:EquipTool(item)
        end
    end
end

-- Função para atacar
function atacar()
    local vim = game:GetService("VirtualInputManager")
    vim:SendKeyEvent(true, "Z", false, game)
    wait(0.3)
    vim:SendKeyEvent(false, "Z", false, game)
end

-- Loop principal de Auto Farm
while task.wait(0.5) do
    local player = game.Players.LocalPlayer
    local char = player.Character or player.CharacterAdded:Wait()
    local humanoidRoot = char:WaitForChild("HumanoidRootPart")
    local inimigos = game:GetService("Workspace").Enemies:GetChildren()

    for _, npc in pairs(inimigos) do
        if npc.Name == npcName and npc:FindFirstChild("HumanoidRootPart") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
            equiparArma(arma)
            humanoidRoot.CFrame = npc.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
            repeat
                atacar()
                task.wait(0.3)
            until npc.Humanoid.Health <= 0 or not npc.Parent
        end
    end
end
