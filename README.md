-- Criação da ferramenta KillPlayer
local ToolName = "KillPlayer"

game.Players.PlayerAdded:Connect(function(player)
    -- Espera a personagem do jogador carregar
    player.CharacterAdded:Connect(function(character)
        -- Criação da ferramenta
        local tool = Instance.new("Tool")
        tool.Name = ToolName
        tool.RequiresHandle = true

        -- Criação de uma parte "Handle" para a ferramenta
        local handle = Instance.new("Part")
        handle.Name = "Handle"
        handle.Size = Vector3.new(1, 1, 1)
        handle.CanCollide = false
        handle.Anchored = false
        handle.Parent = tool

        -- Define a posição inicial da ferramenta na mochila do jogador
        tool.Parent = player.Backpack

        -- Função para matar o jogador que for clicado com a ferramenta
        tool.Activated:Connect(function()
            -- Verificar se a ferramenta está tocando um jogador
            local target = tool.Parent.HumanoidRootPart  -- Posição de onde a ferramenta foi ativada
            for _, part in pairs(workspace:GetPartsInRegion3(tool.Handle.Position, Vector3.new(5, 5, 5), nil)) do
                local targetPlayer = game.Players:GetPlayerFromCharacter(part.Parent)
                if targetPlayer and targetPlayer ~= player then
                    -- Se o player tocar outro personagem, a saúde dele será definida para 0 (morrendo)
                    local humanoid = part.Parent:FindFirstChildOfClass("Humanoid")
                    if humanoid then
                        humanoid.Health = 0
                    end
                end
            end
        end)
    end)
end)
